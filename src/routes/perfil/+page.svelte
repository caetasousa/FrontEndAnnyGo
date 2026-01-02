<script lang="ts">
    import { onMount } from "svelte";
    import Sidebar from "$lib/components/Sidebar.svelte";
    import DashboardNavbar from "$lib/components/DashboardNavbar.svelte";
    import AvailabilityModal from "$lib/components/profile/AvailabilityModal.svelte";
    import ServiceSelectionModal from "$lib/components/profile/ServiceSelectionModal.svelte";
    import AlertModal from "$lib/components/AlertModal.svelte";
    
    // Import shared types
    import type { 
        Service, 
        AgendaInterval, 
        AgendaDay, 
        ProviderProfile, 
        ProviderData 
    } from '$lib/types/profile';

    // Import Refactored Components
    import ProfileAgendaTab from "$lib/components/profile/ProfileAgendaTab.svelte";
    import ProfileAppointmentsTab from "$lib/components/profile/ProfileAppointmentsTab.svelte";
    import ProfileDataTab from "$lib/components/profile/ProfileDataTab.svelte";
    import ProfileServicesTab from "$lib/components/profile/ProfileServicesTab.svelte";

    import { uploadPrestadorImageToSupabase, deleteImageFromSupabase } from "$lib/utils/imageUpload";

    // --- State ---
    let activeTab: "agenda" | "atendimentos" | "dados" | "servicos" = "agenda";

    // --- Calendar Logic (for modal only, main logic moved to component) ---
    const today = new Date();
    let currentMonth = today.getMonth();
    let currentYear = today.getFullYear();

    // Mock Data for "Exceptions" (Days with specific status)
    let dayExceptions: Record<
        string,
        { status: "off" | "partial"; label?: string }
    > = {
        "2025-07-28": { status: "off", label: "Férias" }, // Example future date
    };

    // --- Provider Data ---
    let providerId = "d583ek114a3g8q7re3rg"; // Fixed ID as requested
    let availabilityMap: Record<string, AgendaInterval[]> = {};
    let currentProviderData: ProviderData | null = null; // Store full object for PUT updates

    // --- Services Logic ---
    let services: any[] = []; // Converted structure for view
    let servicesLoading = false;
    let showSelectionModal = false;
    
    // --- Edit Mode Logic ---
    let isEditing = false;
    let selectedFile: File | null = null;

    // --- Form Data ---
    let profile: ProviderProfile = {
        name: "Ana Maria Costa",
        cpf: "123.456.789-00",
        phone: "(11) 98765-4321",
        email: "anamaria@bellavita.com",
        avatarUrl: "",
    };

    async function fetchProviderData() {
        try {
            const response = await fetch(`/api/v1/prestadores/${providerId}`);
            if (response.ok) {
                const data: ProviderData = await response.json();
                currentProviderData = data;

                // 1. Populate Profile Data
                profile = {
                    name: data.nome,
                    email: data.email,
                    phone: data.telefone,
                    cpf: data.cpf,
                    avatarUrl: data.image_url,
                };

                // 2. Populate Availability (Agenda)
                const agendaData = data.agenda;
                if (agendaData && Array.isArray(agendaData)) {
                    availabilityMap = agendaData.reduce(
                        (acc: Record<string, AgendaInterval[]>, item: AgendaDay) => {
                            // Map Intervalos to store format
                            const dateKey = item.data;
                            const intervalos = item.intervalos || [];

                            acc[dateKey] = intervalos.map((inv: AgendaInterval) => ({
                                id: inv.id,
                                hora_inicio: inv.hora_inicio,
                                hora_fim: inv.hora_fim,
                            }));
                            return acc;
                        },
                        {},
                    );
                }

                // 3. Populate Services (Catalogo)
                const catalogData = data.catalogo;
                if (catalogData && Array.isArray(catalogData)) {
                    // Map API response to component state (TitleCase)
                    // Note: Ideally ProfileServicesTab should handle raw data, 
                    // but for compatibility with existing modals we map it here
                    services = catalogData.map((s: Service) => ({
                        ID: s.id || (s as any).ID, // handle potential variations
                        Nome: s.nome || (s as any).Nome,
                        Preco: s.preco || (s as any).Preco,
                        DuracaoPadrao: s.duracao_padrao || (s as any).DuracaoPadrao,
                        Categoria: s.categoria || (s as any).Categoria,
                        ImagemUrl: s.image_url || (s as any).ImagemUrl || ""
                    }));
                }
            } else {
                console.error("Failed to fetch provider data");
            }
        } catch (error) {
            console.error("Error fetching provider data:", error);
        }
    }

    onMount(() => {
        fetchProviderData();
    });

    function toggleEdit() {
        isEditing = true;
    }

    function cancelEdit() {
        isEditing = false;
        selectedFile = null;
        fetchProviderData(); // Reset data
    }

    function handleImageSelect(event: CustomEvent<{ file: File; preview: string }>) {
        selectedFile = event.detail.file;
        profile.avatarUrl = event.detail.preview;
    }

    function handleImageClear() {
        selectedFile = null;
        profile.avatarUrl = currentProviderData?.image_url || "";
    }

    function openSelectionModal() {
        showSelectionModal = true;
    }

    async function saveProviderCatalog(updatedServices: any[]) {
        if (!currentProviderData) return;

        // Extract catalog IDs properly (handle ID or id)
        const catalogIds = updatedServices.map(s => String(s.ID));

        // Construct payload strictly as requested by user
        const updatedProvider: ProviderData = {
            ...currentProviderData,
            catalogo_ids: catalogIds,
            // Ensure other fields are strings to match interface, even if not changed here
            catalogo: currentProviderData.catalogo || [],
            agenda: currentProviderData.agenda || []
        };

        try {
            const response = await fetch(`/api/v1/prestadores/${providerId}`, {
                method: "PUT",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(updatedProvider),
            });

            if (response.ok) {
                // Update local state and currentProviderData
                services = updatedServices;
                currentProviderData = updatedProvider;
            } else {
                const errorText = await response.text();
                console.error("Failed to update catalog", response.status, errorText);
                alert(`Erro ao atualizar catálogo (${response.status}): ${errorText}`);
            }
        } catch (e) {
            console.error("Error saving catalog:", e);
            alert("Erro de conexão.");
        }
    }

    async function saveProfileData() {
        if (!currentProviderData) return;

        try {
            let imageUrl = profile.avatarUrl;

            // Upload image if a new file was selected
            if (selectedFile) {
                const uploadedUrl = await uploadPrestadorImageToSupabase(selectedFile);
                if (uploadedUrl) {
                    imageUrl = uploadedUrl;
                } else {
                    throw new Error("Falha ao fazer upload da imagem.");
                }
            }

            // Use current services for the ID list to prevent data loss or changes to catalog
            const catalogIds = services.map((s) => String(s.ID));

            const updatedProvider: ProviderData = {
                ...currentProviderData,
                catalogo_ids: catalogIds,
                email: profile.email,
                image_url: imageUrl,
                nome: profile.name,
                telefone: profile.phone,
                cpf: profile.cpf,
                catalogo: currentProviderData.catalogo || [],
                agenda: currentProviderData.agenda || []
            };

            const response = await fetch(`/api/v1/prestadores/${providerId}`, {
                method: "PUT",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(updatedProvider),
            });

            if (response.ok) {
                // If we successfully updated and had a new image, delete the old one
                if (selectedFile && currentProviderData.image_url && currentProviderData.image_url !== imageUrl) {
                    // Fire-and-forget delete logic
                    deleteImageFromSupabase(currentProviderData.image_url).then(success => {
                        if (success) console.log("Old image deleted successfully");
                        else console.warn("Failed to delete old image");
                    });
                }

                currentProviderData = { ...currentProviderData, ...updatedProvider };
                isEditing = false; // Exit edit mode
                
                alertConfig = {
                    title: "Sucesso",
                    message: "Dados atualizados com sucesso!",
                    type: "success",
                    confirmText: "OK",
                    showCancel: false,
                };
                onConfirmAction = () => { showAlert = false; };
                showAlert = true;
            } else {
                const errorText = await response.text();
                console.error("Failed to update profile", response.status, errorText);
                
                alertConfig = {
                    title: "Erro",
                    message: `Erro ao atualizar dados: ${errorText}`,
                    type: "error",
                    confirmText: "Fechar",
                    showCancel: false,
                };
                showAlert = true;
            }
        } catch (e) {
            console.error("Error saving profile:", e);
            alertConfig = {
                title: "Erro",
                message: "Erro de conexão ao tentar salvar.",
                type: "error",
                confirmText: "Fechar",
                showCancel: false,
            };
            showAlert = true;
        }
    }

    // --- Alert Modal Logic ---
    let showAlert = false;
    let alertConfig = {
        title: "",
        message: "",
        type: "info" as "info" | "success" | "warning" | "error" | "primary",
        confirmText: "Confirmar",
        showCancel: true,
    };
    let onConfirmAction: () => void = () => {};

    function handleConfirmAction() {
        if (onConfirmAction) onConfirmAction();
        showAlert = false;
    }

    function handleAddService(event: CustomEvent) {
        const newService = event.detail;
        // Check duplication by ID
        if (!services.find((s) => s.ID === (newService.ID || newService.id))) {
            alertConfig = {
                title: "Confirmar Adição",
                message: `Deseja adicionar o serviço "${newService.Nome || newService.nome}" aos seus serviços?`,
                type: "primary",
                confirmText: "Adicionar",
                showCancel: true,
            };
            onConfirmAction = () => {
                const serviceToAdd = {
                    ID: newService.ID || newService.id,
                    Nome: newService.Nome || newService.nome,
                    Preco: newService.Preco || newService.preco,
                    DuracaoPadrao:
                        newService.DuracaoPadrao || newService.duracao_padrao,
                    Categoria: newService.Categoria || newService.categoria,
                    ImagemUrl: newService.ImagemUrl || newService.image_url,
                };
                const updatedServices = [...services, serviceToAdd];
                saveProviderCatalog(updatedServices);
            };
            showAlert = true;
        }
    }

    function handleRemoveService(event: CustomEvent<{ service: any}>) {
        const service = event.detail.service;
        alertConfig = {
            title: "Remover Serviço",
            message: `Tem certeza que deseja remover "${service.Nome}"?`,
            type: "error",
            confirmText: "Remover",
            showCancel: true,
        };
        onConfirmAction = () => {
            const updatedServices = services.filter((s) => s.ID !== service.ID);
            saveProviderCatalog(updatedServices);
        };
        showAlert = true;
    }

    // --- Availability Modal ---
    let showAvailabilityModal = false;
    // providerId is defined in Provider Data section
    let selectedDateForModal = "";
    let selectedIntervalsForModal: { start: string; end: string }[] = [];

    function openAvailabilityModal(day?: number, month?: number, year?: number) {
        // Logic to setup modal state
        const targetDate = new Date(); // Use today as default base
        let targetDay = day;

        if (typeof targetDay !== "number") {
             // If no day passed, maybe default to today or clear
             selectedDateForModal = "";
             selectedIntervalsForModal = [];
             showAvailabilityModal = true;
             return;
        }

        // Use passed month/year or fallback to current state (though event should provide them)
        const m = typeof month === 'number' ? month : currentMonth;
        const y = typeof year === 'number' ? year : currentYear;

        const dateObj = new Date(y, m, targetDay);
        // Date check already done in UI but safety check
        const today = new Date();
        today.setHours(0, 0, 0, 0);

        if (dateObj < today) return;

        const paddedMonth = String(m + 1).padStart(2, "0");
        const paddedDay = String(targetDay).padStart(2, "0");
        selectedDateForModal = `${y}-${paddedMonth}-${paddedDay}`;

        // Check if there are existing intervals
        if (availabilityMap[selectedDateForModal]) {
            const rawIntervals = availabilityMap[selectedDateForModal];
            // Map to component format { start, end }
            selectedIntervalsForModal = rawIntervals.map((i: AgendaInterval) => ({
                start: i.hora_inicio,
                end: i.hora_fim,
            }));
        } else {
            selectedIntervalsForModal = [];
        }
        showAvailabilityModal = true;
    }
    
    // Handler for event from AccountAgendaTab
    function handleEditAvailability(event: CustomEvent<{ day: number; month: number; year: number }>) {
        openAvailabilityModal(event.detail.day, event.detail.month, event.detail.year);
    }

</script>

<div
    class="font-body bg-[hsl(var(--bs-background))] text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200"
>
    <Sidebar />

    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <DashboardNavbar />

        <div class="flex-1 overflow-y-auto p-6 md:p-8">
            <div class="max-w-6xl mx-auto space-y-6">
                <!-- Header / Navigation Tabs -->
                <div
                    class="bg-[hsl(var(--bs-card))] rounded-xl shadow-sm border border-border-light dark:border-border-dark p-1"
                >
                    <div class="grid grid-cols-4 gap-1 p-1">
                        <button
                            on:click={() => (activeTab = "agenda")}
                            class="flex items-center justify-center py-2.5 text-sm font-medium rounded-lg transition-all {activeTab ===
                            'agenda'
                                ? 'bg-brand-orange text-white shadow-md'
                                : 'text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800 dark:text-gray-400'}"
                        >
                            <span class="material-symbols-outlined mr-2"
                                >calendar_month</span
                            >
                            Minha Disponibilidade
                        </button>
                        <button
                            on:click={() => (activeTab = "atendimentos")}
                            class="flex items-center justify-center py-2.5 text-sm font-medium rounded-lg transition-all {activeTab ===
                            'atendimentos'
                                ? 'bg-brand-orange text-white shadow-md'
                                : 'text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800 dark:text-gray-400'}"
                        >
                            <span class="material-symbols-outlined mr-2"
                                >event_note</span
                            >
                            Meus Atendimentos
                        </button>
                        <button
                            on:click={() => (activeTab = "dados")}
                            class="flex items-center justify-center py-2.5 text-sm font-medium rounded-lg transition-all {activeTab ===
                            'dados'
                                ? 'bg-brand-orange text-white shadow-md'
                                : 'text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800 dark:text-gray-400'}"
                        >
                            <span class="material-symbols-outlined mr-2"
                                >person</span
                            >
                            Meus Dados
                        </button>
                        <button
                            on:click={() => (activeTab = "servicos")}
                            class="flex items-center justify-center py-2.5 text-sm font-medium rounded-lg transition-all {activeTab ===
                            'servicos'
                                ? 'bg-brand-orange text-white shadow-md'
                                : 'text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800 dark:text-gray-400'}"
                        >
                            <span class="material-symbols-outlined mr-2"
                                >content_cut</span
                            >
                            Meus Serviços
                        </button>
                    </div>
                </div>

                {#if activeTab === "agenda"}
                    <ProfileAgendaTab 
                        {availabilityMap}
                        {dayExceptions}
                        on:editAvailability={handleEditAvailability}
                    />
                {:else if activeTab === "atendimentos"}
                    <ProfileAppointmentsTab />
                {:else if activeTab === "dados"}
                    <ProfileDataTab 
                        {profile} 
                        {isEditing}

                        on:toggleEdit={toggleEdit}
                        on:cancelEdit={cancelEdit}
                        on:save={saveProfileData}
                        on:imageSelect={handleImageSelect}
                        on:imageClear={handleImageClear}
                    />
                {:else if activeTab === "servicos"}
                    <ProfileServicesTab 
                        {services} 
                        {servicesLoading} 
                        on:addService={openSelectionModal}
                        on:removeService={handleRemoveService}
                    />
                {/if}
            </div>
        </div>
        <!-- Availability Modal -->
        <AvailabilityModal
            bind:show={showAvailabilityModal}
            {providerId}
            initialDate={selectedDateForModal}
            initialIntervals={selectedIntervalsForModal}
            on:success={fetchProviderData}
        />

        <!-- Service Selection Modal restored -->
        <ServiceSelectionModal
            bind:show={showSelectionModal}
            {providerId}
            existingServiceIds={services.map(s => s.ID)}
            on:add={handleAddService}
        />

        <AlertModal
            bind:show={showAlert}
            title={alertConfig.title}
            message={alertConfig.message}
            type={alertConfig.type}
            confirmText={alertConfig.confirmText}
            showCancel={alertConfig.showCancel}
            on:confirm={handleConfirmAction}
        />
    </main>
</div>
