<script lang="ts">
    import { onMount } from "svelte";
    import Sidebar from "$lib/components/Sidebar.svelte";
    import ThemeToggle from "$lib/components/ThemeToggle.svelte";
    import AvailabilityModal from "$lib/components/AvailabilityModal.svelte";
    import ServiceSelectionModal from "$lib/components/ServiceSelectionModal.svelte";
    import AlertModal from "$lib/components/AlertModal.svelte";

    // --- State ---
    let activeTab: "agenda" | "atendimentos" | "dados" | "servicos" = "agenda";

    // --- Calendar Logic ---
    const today = new Date();
    let currentMonth = today.getMonth();
    let currentYear = today.getFullYear();

    const monthNames = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro",
    ];

    // Mock Data for "Exceptions" (Days with specific status)
    let dayExceptions: Record<
        string,
        { status: "off" | "partial"; label?: string }
    > = {
        "2025-07-28": { status: "off", label: "Férias" }, // Example future date
    };

    // --- Provider Data ---
    let providerId = "d583ek114a3g8q7re3rg"; // Fixed ID as requested
    let availabilityMap: Record<string, any[]> = {};
    let currentProviderData: any = null; // Store full object for PUT updates

    async function fetchProviderData() {
        try {
            const response = await fetch(`/api/v1/prestadores/${providerId}`);
            if (response.ok) {
                const data = await response.json();
                currentProviderData = data;

                // 1. Populate Profile Data
                // Checking both cases just to be safe, or reverting to original lowercase expectation
                profile = {
                    name: data.nome || data.Nome,
                    email: data.email || data.Email,
                    phone: data.telefone || data.Telefone,
                    cpf: data.cpf || data.Cpf,
                    bio:
                        data.bio ||
                        data.Bio ||
                        "Profissional cadastrado na plataforma.",
                    avatarUrl: data.imagem_url || data.ImagemUrl,
                };

                // 2. Populate Availability (Agenda)
                const agendaData = data.agenda || data.Agenda;
                if (agendaData && Array.isArray(agendaData)) {
                    availabilityMap = agendaData.reduce(
                        (acc: Record<string, any[]>, item: any) => {
                            // Map Intervalos to store format check both casings
                            const dateKey = item.data || item.Data;
                            const intervalos =
                                item.intervalos || item.Intervalos || [];

                            acc[dateKey] = intervalos.map((inv: any) => ({
                                hora_inicio: inv.hora_inicio || inv.HoraInicio,
                                hora_fim: inv.hora_fim || inv.HoraFim,
                            }));
                            return acc;
                        },
                        {},
                    );
                }

                // 3. Populate Services (Catalogo)
                const catalogData = data.catalogo || data.Catalogo;
                if (catalogData && Array.isArray(catalogData)) {
                    // Map API response (lowercase snake_case) to component state (TitleCase)
                    services = catalogData.map((s: any) => ({
                        ID: s.id || s.ID,
                        Nome: s.nome || s.Nome,
                        Preco: s.preco || s.Preco,
                        DuracaoPadrao: s.duracao_padrao || s.DuracaoPadrao,
                        Categoria: s.categoria || s.Categoria,
                        ImagemUrl: s.image_url || s.ImagemUrl || s.imagemUrl
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
        // fetchProviderServices(); // No longer needed as separate call if Catalogo comes in main object
    });

    // --- Services Logic ---
    let services: any[] = [];
    let servicesLoading = false;
    let showSelectionModal = false;

    // Optional: Keep this if we need to re-fetch ONLY services later
    async function fetchProviderServices() {
        // logic if needed, otherwise relied on fetchProviderData
    }

    function openSelectionModal() {
        showSelectionModal = true;
    }

    async function saveProviderCatalog(updatedServices: any[]) {
        if (!currentProviderData) return;

        // Extract catalog IDs properly (handle ID or id)
        const catalogIds = updatedServices.map(s => String(s.ID || s.id));

        // Construct payload strictly as requested by user
        const updatedProvider = {
            catalogo_ids: catalogIds,
            email: currentProviderData.email || currentProviderData.Email,
            image_url: currentProviderData.image_url || currentProviderData.ImagemUrl || currentProviderData.imagem_url,
            nome: currentProviderData.nome || currentProviderData.Nome,
            telefone: currentProviderData.telefone || currentProviderData.Telefone
            // Add other mandatory fields if any appear in future errors
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
                // Optional: alert check
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
        if (!services.find((s) => s.ID === newService.ID)) {
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

    async function handleRemoveService(service: any) {
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

    function getDaysInMonth(month: number, year: number) {
        return new Date(year, month + 1, 0).getDate();
    }

    function getFirstDayOfMonth(month: number, year: number) {
        return new Date(year, month, 1).getDay();
    }

    $: daysInCurrentMonth = getDaysInMonth(currentMonth, currentYear);
    $: firstDayOfWeek = getFirstDayOfMonth(currentMonth, currentYear);

    // Create array of days for grid (padding empty slots)
    $: calendarDays = [
        ...Array(firstDayOfWeek).fill(null),
        ...Array.from({ length: daysInCurrentMonth }, (_, i) => i + 1),
    ];

    function changeMonth(step: number) {
        currentMonth += step;
        if (currentMonth > 11) {
            currentMonth = 0;
            currentYear++;
        } else if (currentMonth < 0) {
            currentMonth = 11;
            currentYear--;
        }
    }

    function getDayStatus(day: number, map: Record<string, any[]>) {
        if (!day) return null;

        const date = new Date(currentYear, currentMonth, day);
        const today = new Date();
        today.setHours(0, 0, 0, 0);
        const isPast = date < today;

        // Simple logic: Weekends are "off" by default standard, others "open"
        // Check exceptions first
        const dateStr = `${currentYear}-${String(currentMonth + 1).padStart(2, "0")}-${String(day).padStart(2, "0")}`;
        if (dayExceptions[dateStr])
            return { ...dayExceptions[dateStr], isReal: false, isPast };

        // Check Real Availability
        if (availabilityMap[dateStr]) {
            const rawIntervals = availabilityMap[dateStr];

            // Deduplicate intervals for display (based on start/end time)
            const uniqueIntervals = rawIntervals.filter(
                (v: any, i: number, a: any[]) =>
                    a.findIndex(
                        (t: any) =>
                            t.hora_inicio === v.hora_inicio &&
                            t.hora_fim === v.hora_fim,
                    ) === i,
            );

            // Should duplicate logic from above if needed, but we pass uniqueIntervals
            let label = "Disponível";
            if (uniqueIntervals.length > 0) {
                const first = uniqueIntervals[0];
                label = `${first.hora_inicio} - ${first.hora_fim}`;
                if (uniqueIntervals.length > 1) label += "...";
            }

            return {
                status: "open",
                label: label,
                isReal: true,
                intervals: uniqueIntervals,
                isPast,
            };
        }

        const dayOfWeek = date.getDay();
        if (dayOfWeek === 0)
            return { status: "off", label: "Folga", isReal: false, isPast }; // Sunday
        if (dayOfWeek === 6)
            return {
                status: "partial",
                label: "08:00 - 12:00",
                isReal: false,
                isPast,
            }; // Saturday
        return {
            status: "open",
            label: "08:00 - 12:00",
            isReal: false,
            isPast,
        }; // Weekdays
    }

    // --- Form Data (Existing) ---
    let profile: {
        name: string;
        cpf: string;
        phone: string;
        email: string;
        bio: string;
        avatarUrl?: string;
    } = {
        name: "Ana Maria Costa",
        cpf: "123.456.789-00",
        phone: "(11) 98765-4321",
        email: "anamaria@bellavita.com",
        bio: "Especialista em cortes modernos e coloração com mais de 5 anos de experiência.",
        avatarUrl: "",
    };

    // --- Availability Modal ---
    let showAvailabilityModal = false;
    // providerId is defined in Provider Data section
    let selectedDateForModal = "";
    let selectedIntervalsForModal: { start: string; end: string }[] = [];

    function openAvailabilityModal(day?: number) {
        if (typeof day === "number") {
            const dateObj = new Date(currentYear, currentMonth, day);
            const today = new Date();
            today.setHours(0, 0, 0, 0);

            if (dateObj < today) return;

            const paddedMonth = String(currentMonth + 1).padStart(2, "0");
            const paddedDay = String(day).padStart(2, "0");
            selectedDateForModal = `${currentYear}-${paddedMonth}-${paddedDay}`;

            // Check if there are existing intervals
            if (availabilityMap[selectedDateForModal]) {
                const rawIntervals = availabilityMap[selectedDateForModal];
                // Map to component format format { start, end }
                selectedIntervalsForModal = rawIntervals.map((i: any) => ({
                    start: i.hora_inicio,
                    end: i.hora_fim,
                }));
            } else {
                selectedIntervalsForModal = [];
            }
        } else {
            selectedDateForModal = "";
            selectedIntervalsForModal = [];
        }
        showAvailabilityModal = true;
    }
</script>

<div
    class="font-body bg-[hsl(var(--bs-background))] text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200"
>
    <Sidebar />

    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <header
            class="h-16 bg-[hsl(var(--bs-card))] border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10"
        >
            <div class="flex items-center flex-1 max-w-2xl">
                <h1 class="text-xl font-bold text-gray-900 dark:text-white">
                    Meu Perfil e Agenda
                </h1>
            </div>
            <div class="flex items-center space-x-4 ml-4">
                <button
                    class="p-2 rounded-full text-gray-400 hover:text-gray-500 dark:hover:text-gray-300 focus:outline-none transition-colors"
                >
                    <span class="material-symbols-outlined">notifications</span>
                </button>
                <ThemeToggle />
            </div>
        </header>

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
                    <!-- AGENDA (Availability) VIEW -->
                    <div
                        class="grid grid-cols-1 lg:grid-cols-3 gap-6 animate-fade-in"
                    >
                        <!-- Left: Calendar -->
                        <div class="lg:col-span-2 space-y-6">
                            <div
                                class="bg-[hsl(var(--bs-card))] rounded-xl border border-border-light dark:border-border-dark p-6"
                            >
                                <div
                                    class="flex items-center justify-between mb-6"
                                >
                                    <h2
                                        class="text-xl font-bold text-gray-900 dark:text-white flex items-center"
                                    >
                                        <span class="capitalize"
                                            >{monthNames[currentMonth]}
                                            {currentYear}</span
                                        >
                                    </h2>
                                    <div class="flex space-x-2">
                                        <button
                                            on:click={() => changeMonth(-1)}
                                            class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-400"
                                        >
                                            <span
                                                class="material-symbols-outlined"
                                                >chevron_left</span
                                            >
                                        </button>
                                        <button
                                            on:click={() => changeMonth(1)}
                                            class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-400"
                                        >
                                            <span
                                                class="material-symbols-outlined"
                                                >chevron_right</span
                                            >
                                        </button>
                                    </div>
                                </div>

                                <!-- Calendar Grid -->
                                <div
                                    class="grid grid-cols-7 gap-px lg:gap-2 text-center"
                                >
                                    <!-- Weekday Headers -->
                                    {#each ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sáb"] as day}
                                        <div
                                            class="text-xs font-semibold text-gray-400 uppercase tracking-wider py-2"
                                        >
                                            {day}
                                        </div>
                                    {/each}

                                    <!-- Days -->
                                    {#each calendarDays as day}
                                        {#if day}
                                            {@const status = getDayStatus(
                                                day,
                                                availabilityMap,
                                            )}
                                            <button
                                                on:click={() =>
                                                    openAvailabilityModal(day)}
                                                class="aspect-square relative flex items-start justify-end p-2 rounded-lg border border-transparent transition-all group
                                                {status?.isPast
                                                    ? 'opacity-40 cursor-not-allowed grayscale'
                                                    : 'hover:border-brand-orange'}
                                        {status?.isReal
                                                    ? 'bg-blue-100 dark:bg-blue-900/30 border-blue-200 dark:border-blue-500'
                                                    : status?.status === 'off'
                                                      ? 'bg-red-50 dark:bg-red-900/10'
                                                      : status?.status ===
                                                          'partial'
                                                        ? 'bg-orange-50 dark:bg-orange-900/10'
                                                        : 'bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20'}"
                                            >
                                                <span
                                                    class="z-10 text-sm font-medium {status?.status ===
                                                    'off'
                                                        ? 'text-red-500'
                                                        : 'text-gray-700 dark:text-gray-300'}"
                                                    >{day}</span
                                                >

                                                <!-- Status indicator / Label -->
                                                <div
                                                    class="absolute inset-x-1 bottom-1 flex flex-col items-center"
                                                >
                                                    {#if status?.status === "off"}
                                                        <span
                                                            class="text-[10px] font-bold text-red-500 uppercase tracking-tight truncate w-full"
                                                            >Folga</span
                                                        >
                                                    {:else if status?.status === "partial"}
                                                        <div
                                                            class="w-1.5 h-1.5 rounded-full bg-orange-400 mb-0.5"
                                                        ></div>
                                                        <span
                                                            class="text-[10px] text-orange-600 dark:text-orange-400 truncate w-full hidden sm:block"
                                                            >{status?.label}</span
                                                        >
                                                    {:else}
                                                        <div
                                                            class="w-1.5 h-1.5 rounded-full {status?.isReal
                                                                ? 'bg-blue-500'
                                                                : 'bg-green-400'}"
                                                        ></div>
                                                        {#if status?.isReal && status?.intervals}
                                                            <div
                                                                class="flex flex-col w-full px-0.5 mt-0.5 space-y-0.5 overflow-hidden"
                                                            >
                                                                {#each status.intervals.slice(0, 3) as interval}
                                                                    <span
                                                                        class="text-[9px] leading-tight text-blue-600 dark:text-blue-400 truncate w-full font-medium block"
                                                                    >
                                                                        {interval.hora_inicio}-{interval.hora_fim}
                                                                    </span>
                                                                {/each}
                                                                {#if status.intervals.length > 3}
                                                                    <span
                                                                        class="text-[8px] leading-none text-blue-500 text-center block"
                                                                        >+{status
                                                                            .intervals
                                                                            .length -
                                                                            3}</span
                                                                    >
                                                                {/if}
                                                            </div>
                                                        {:else if status?.isReal}
                                                            <span
                                                                class="text-[10px] text-blue-600 dark:text-blue-400 truncate w-full hidden sm:block font-medium"
                                                                >{status?.label}</span
                                                            >
                                                        {/if}
                                                    {/if}
                                                </div>
                                            </button>
                                        {:else}
                                            <div></div>
                                        {/if}
                                    {/each}
                                </div>
                            </div>
                        </div>

                        <!-- Right: Quick Actions & Standard Week -->
                        <div class="space-y-6">
                            <!-- Standard Week Config -->
                            <div
                                class="bg-[hsl(var(--bs-card))] rounded-xl border border-border-light dark:border-border-dark p-6"
                            >
                                <div
                                    class="flex items-center justify-between mb-4"
                                >
                                    <h3
                                        class="text-lg font-bold text-gray-900 dark:text-white flex items-center"
                                    >
                                        <span
                                            class="material-symbols-outlined text-brand-orange mr-2"
                                            >schedule</span
                                        >
                                        Semana Padrão
                                    </h3>
                                    <button
                                        class="text-xs text-primary font-medium hover:underline"
                                        >Editar</button
                                    >
                                </div>
                                <p class="text-xs text-gray-500 mb-4">
                                    Seus horários recorrentes.
                                </p>

                                <div class="space-y-3">
                                    <div
                                        class="flex justify-between items-center text-sm p-2 rounded hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"
                                    >
                                        <span
                                            class="text-gray-600 dark:text-gray-400"
                                            >Seg - Sex</span
                                        >
                                        <div class="flex flex-col text-right">
                                            <span
                                                class="font-medium text-gray-900 dark:text-white"
                                                >08:00 - 14:00</span
                                            >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white"
                                                >14:00 - 18:00</span
                                            >
                                        </div>
                                    </div>
                                    <div
                                        class="flex justify-between items-center text-sm p-2 rounded hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"
                                    >
                                        <span
                                            class="text-gray-600 dark:text-gray-400"
                                            >Sábado</span
                                        >
                                        <span
                                            class="font-medium text-gray-900 dark:text-white"
                                            >08:00 - 12:00</span
                                        >
                                    </div>
                                    <div
                                        class="flex justify-between items-center text-sm p-2 rounded hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"
                                    >
                                        <span
                                            class="text-gray-600 dark:text-gray-400"
                                            >Domingo</span
                                        >
                                        <span class="font-bold text-red-400"
                                            >Folga</span
                                        >
                                    </div>
                                </div>
                            </div>

                            <!-- Quick Block Actions -->
                            <div
                                class="bg-[hsl(var(--bs-card))] rounded-xl border border-border-light dark:border-border-dark p-6"
                            >
                                <h3
                                    class="text-lg font-bold text-gray-900 dark:text-white mb-4"
                                >
                                    Ações Rápidas
                                </h3>
                                <div class="grid grid-cols-2 gap-3">
                                    <button
                                        class="flex flex-col items-center justify-center p-3 rounded-lg border border-gray-200 dark:border-gray-700 hover:border-brand-orange hover:bg-orange-50 dark:hover:bg-orange-900/10 transition-all text-center"
                                    >
                                        <span
                                            class="material-symbols-outlined text-brand-orange mb-1"
                                            >block</span
                                        >
                                        <span
                                            class="text-xs font-semibold text-gray-700 dark:text-gray-300"
                                            >Bloquear Dia</span
                                        >
                                    </button>
                                    <button
                                        on:click={() => openAvailabilityModal()}
                                        class="flex flex-col items-center justify-center p-3 rounded-lg border border-gray-200 dark:border-gray-700 hover:border-blue-500 hover:bg-blue-50 dark:hover:bg-blue-900/10 transition-all text-center"
                                    >
                                        <span
                                            class="material-symbols-outlined text-blue-500 mb-1"
                                            >edit_calendar</span
                                        >
                                        <span
                                            class="text-xs font-semibold text-gray-700 dark:text-gray-300"
                                            >Ajustar Horário</span
                                        >
                                    </button>
                                    <button
                                        class="flex flex-col items-center justify-center p-3 rounded-lg border border-gray-200 dark:border-gray-700 hover:border-purple-500 hover:bg-purple-50 dark:hover:bg-purple-900/10 transition-all text-center"
                                    >
                                        <span
                                            class="material-symbols-outlined text-purple-500 mb-1"
                                            >flight</span
                                        >
                                        <span
                                            class="text-xs font-semibold text-gray-700 dark:text-gray-300"
                                            >Marcar Férias</span
                                        >
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                {:else if activeTab === "atendimentos"}
                    <!-- ATENDIMENTOS (Appointments) VIEW -->
                    <div class="animate-fade-in space-y-6">
                        <!-- Stats Grid -->
                        <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                            <div
                                class="bg-[hsl(var(--bs-card))] p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center"
                            >
                                <div
                                    class="p-3 rounded-full bg-blue-100 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 mr-4"
                                >
                                    <span class="material-symbols-outlined"
                                        >calendar_today</span
                                    >
                                </div>
                                <div>
                                    <p
                                        class="text-sm text-gray-500 dark:text-gray-400"
                                    >
                                        Hoje
                                    </p>
                                    <p
                                        class="text-2xl font-bold text-gray-900 dark:text-white"
                                    >
                                        8
                                    </p>
                                </div>
                            </div>
                            <div
                                class="bg-[hsl(var(--bs-card))] p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center"
                            >
                                <div
                                    class="p-3 rounded-full bg-yellow-100 dark:bg-yellow-900/30 text-yellow-600 dark:text-yellow-400 mr-4"
                                >
                                    <span class="material-symbols-outlined"
                                        >pending</span
                                    >
                                </div>
                                <div>
                                    <p
                                        class="text-sm text-gray-500 dark:text-gray-400"
                                    >
                                        Pendentes
                                    </p>
                                    <p
                                        class="text-2xl font-bold text-gray-900 dark:text-white"
                                    >
                                        3
                                    </p>
                                </div>
                            </div>
                            <div
                                class="bg-[hsl(var(--bs-card))] p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center"
                            >
                                <div
                                    class="p-3 rounded-full bg-green-100 dark:bg-green-900/30 text-green-600 dark:text-green-400 mr-4"
                                >
                                    <span class="material-symbols-outlined"
                                        >check_circle</span
                                    >
                                </div>
                                <div>
                                    <p
                                        class="text-sm text-gray-500 dark:text-gray-400"
                                    >
                                        Confirmados
                                    </p>
                                    <p
                                        class="text-2xl font-bold text-gray-900 dark:text-white"
                                    >
                                        12
                                    </p>
                                </div>
                            </div>
                            <div
                                class="bg-[hsl(var(--bs-card))] p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center"
                            >
                                <div
                                    class="p-3 rounded-full bg-purple-100 dark:bg-purple-900/30 text-purple-600 dark:text-purple-400 mr-4"
                                >
                                    <span class="material-symbols-outlined"
                                        >attach_money</span
                                    >
                                </div>
                                <div>
                                    <p
                                        class="text-sm text-gray-500 dark:text-gray-400"
                                    >
                                        Receita Est. (Hoje)
                                    </p>
                                    <p
                                        class="text-2xl font-bold text-gray-900 dark:text-white"
                                    >
                                        R$ 450
                                    </p>
                                </div>
                            </div>
                        </div>

                        <!-- Main Content Grid -->
                        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                            <div class="lg:col-span-2 space-y-4">
                                <div class="flex items-center justify-between">
                                    <h3
                                        class="text-lg font-semibold text-gray-900 dark:text-white flex items-center"
                                    >
                                        <span
                                            class="material-symbols-outlined text-primary mr-2"
                                            >schedule</span
                                        >
                                        Atendimentos de Hoje
                                    </h3>
                                    <button
                                        class="text-sm text-primary font-medium hover:underline"
                                        >Ver Todos</button
                                    >
                                </div>

                                <!-- Appointment Card 1 -->
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border-l-4 border-l-warning border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md"
                                >
                                    <div
                                        class="flex flex-col sm:flex-row justify-between items-start sm:items-center"
                                    >
                                        <div
                                            class="flex items-start space-x-4 mb-4 sm:mb-0"
                                        >
                                            <div
                                                class="bg-warning/10 text-warning p-2 rounded-full"
                                            >
                                                <span
                                                    class="material-symbols-outlined"
                                                    >hourglass_top</span
                                                >
                                            </div>
                                            <div>
                                                <div class="flex items-center">
                                                    <span
                                                        class="text-lg font-bold text-gray-900 dark:text-white"
                                                        >09:00</span
                                                    >
                                                    <span
                                                        class="mx-2 text-gray-400"
                                                        >•</span
                                                    >
                                                    <span
                                                        class="text-sm text-gray-600 dark:text-gray-300 font-medium"
                                                        >Corte de Cabelo +
                                                        Hidratação</span
                                                    >
                                                </div>
                                                <p
                                                    class="text-sm text-gray-500 dark:text-gray-400 mt-1"
                                                >
                                                    Cliente: <span
                                                        class="font-medium text-gray-800 dark:text-gray-200"
                                                        >Maria Oliveira</span
                                                    >
                                                </p>
                                                <div
                                                    class="flex items-center mt-2 text-xs text-gray-400"
                                                >
                                                    <span
                                                        class="material-symbols-outlined text-[16px] mr-1"
                                                        >timer</span
                                                    >
                                                    <span>60 min</span>
                                                    <span class="mx-2">•</span>
                                                    <span
                                                        class="text-warning font-medium"
                                                        >Aguardando Confirmação</span
                                                    >
                                                </div>
                                            </div>
                                        </div>
                                        <div
                                            class="flex space-x-2 w-full sm:w-auto"
                                        >
                                            <button
                                                class="flex-1 sm:flex-none px-3 py-1.5 border border-gray-300 dark:border-gray-600 text-gray-600 dark:text-gray-300 rounded text-sm hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
                                                >Cancelar</button
                                            >
                                            <button
                                                class="flex-1 sm:flex-none px-3 py-1.5 bg-primary hover:bg-primary-hover text-white rounded text-sm shadow-sm transition-colors"
                                                >Confirmar</button
                                            >
                                        </div>
                                    </div>
                                </div>

                                <!-- Appointment Card 2 -->
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border-l-4 border-l-success border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md opacity-75 hover:opacity-100"
                                >
                                    <div
                                        class="flex flex-col sm:flex-row justify-between items-start sm:items-center"
                                    >
                                        <div
                                            class="flex items-start space-x-4 mb-4 sm:mb-0"
                                        >
                                            <div
                                                class="bg-success/10 text-success p-2 rounded-full"
                                            >
                                                <span
                                                    class="material-symbols-outlined"
                                                    >check</span
                                                >
                                            </div>
                                            <div>
                                                <div class="flex items-center">
                                                    <span
                                                        class="text-lg font-bold text-gray-900 dark:text-white"
                                                        >10:30</span
                                                    >
                                                    <span
                                                        class="mx-2 text-gray-400"
                                                        >•</span
                                                    >
                                                    <span
                                                        class="text-sm text-gray-600 dark:text-gray-300 font-medium line-through decoration-gray-400"
                                                        >Coloração Raiz</span
                                                    >
                                                    <span
                                                        class="ml-2 text-xs bg-success/20 text-success px-2 py-0.5 rounded-full"
                                                        >Confirmado</span
                                                    >
                                                </div>
                                                <p
                                                    class="text-sm text-gray-500 dark:text-gray-400 mt-1"
                                                >
                                                    Cliente: <span
                                                        class="font-medium text-gray-800 dark:text-gray-200"
                                                        >Fernanda Lima</span
                                                    >
                                                </p>
                                                <div
                                                    class="flex items-center mt-2 text-xs text-gray-400"
                                                >
                                                    <span
                                                        class="material-symbols-outlined text-[16px] mr-1"
                                                        >timer</span
                                                    >
                                                    <span>90 min</span>
                                                </div>
                                            </div>
                                        </div>
                                        <div
                                            class="flex space-x-2 w-full sm:w-auto"
                                        >
                                            <button
                                                class="flex-1 sm:flex-none px-3 py-1.5 text-primary hover:text-primary-hover text-sm font-medium transition-colors"
                                                >Detalhes</button
                                            >
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- Side Notifications -->
                            <div class="space-y-6">
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-6"
                                >
                                    <h3
                                        class="text-lg font-semibold text-gray-900 dark:text-white mb-4"
                                    >
                                        Solicitações Recentes
                                    </h3>
                                    <div class="space-y-4">
                                        <div
                                            class="flex items-start space-x-3 pb-4 border-b border-gray-100 dark:border-gray-800 last:border-0 last:pb-0"
                                        >
                                            <div
                                                class="h-10 w-10 rounded-full bg-blue-100 dark:bg-blue-900/30 flex items-center justify-center text-blue-600 dark:text-blue-400 shrink-0"
                                            >
                                                <span
                                                    class="material-symbols-outlined text-sm"
                                                    >person</span
                                                >
                                            </div>
                                            <div>
                                                <p
                                                    class="text-sm font-medium text-gray-900 dark:text-white"
                                                >
                                                    Novo cliente se cadastrou
                                                </p>
                                                <p
                                                    class="text-xs text-gray-500 dark:text-gray-400 mt-0.5"
                                                >
                                                    Beatriz M. agendou para
                                                    amanhã às 14h
                                                </p>
                                                <p
                                                    class="text-xs text-gray-400 mt-1"
                                                >
                                                    Há 15 min
                                                </p>
                                            </div>
                                        </div>
                                        <div
                                            class="flex items-start space-x-3 pb-4 border-b border-gray-100 dark:border-gray-800 last:border-0 last:pb-0"
                                        >
                                            <div
                                                class="h-10 w-10 rounded-full bg-orange-100 dark:bg-orange-900/30 flex items-center justify-center text-orange-600 dark:text-orange-400 shrink-0"
                                            >
                                                <span
                                                    class="material-symbols-outlined text-sm"
                                                    >star</span
                                                >
                                            </div>
                                            <div>
                                                <p
                                                    class="text-sm font-medium text-gray-900 dark:text-white"
                                                >
                                                    Nova avaliação recebida
                                                </p>
                                                <p
                                                    class="text-xs text-gray-500 dark:text-gray-400 mt-0.5"
                                                >
                                                    5 estrelas em "Corte e
                                                    Escova"
                                                </p>
                                                <p
                                                    class="text-xs text-gray-400 mt-1"
                                                >
                                                    Há 2 horas
                                                </p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                {:else if activeTab === "dados"}
                    <!-- MEUS DADOS -->
                    <div
                        class="bg-[hsl(var(--bs-card))] rounded-xl border border-border-light dark:border-border-dark p-6 md:p-8 animate-fade-in text-left"
                    >
                        <div class="flex items-center space-x-4 mb-8">
                            <img
                                alt={profile.name}
                                class="h-24 w-24 rounded-full border-4 border-gray-100 dark:border-gray-700 object-cover"
                                src="https://ui-avatars.com/api/?name=Ana+Maria&background=random"
                            />
                            <div>
                                <button
                                    class="px-4 py-2 bg-gray-100 dark:bg-gray-800 text-gray-700 dark:text-gray-300 rounded-lg text-sm font-medium hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors"
                                >
                                    Alterar Foto
                                </button>
                            </div>
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="col-span-1 md:col-span-2">
                                <label
                                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                    for="name">Nome Completo</label
                                >
                                <input
                                    class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20 p-2.5"
                                    type="text"
                                    bind:value={profile.name}
                                />
                            </div>
                            <div>
                                <label
                                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                    for="cpf">CPF</label
                                >
                                <input
                                    class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20 p-2.5"
                                    type="text"
                                    bind:value={profile.cpf}
                                />
                            </div>
                            <div>
                                <label
                                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                    for="phone">Telefone</label
                                >
                                <input
                                    class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20 p-2.5"
                                    type="text"
                                    bind:value={profile.phone}
                                />
                            </div>
                            <div class="col-span-1 md:col-span-2">
                                <label
                                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                    for="email">Email</label
                                >
                                <input
                                    class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20 p-2.5"
                                    type="email"
                                    bind:value={profile.email}
                                />
                            </div>
                            <div class="col-span-1 md:col-span-2">
                                <label
                                    class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                    for="bio">Biografia</label
                                >
                                <textarea
                                    class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20 p-2.5"
                                    rows="3"
                                    bind:value={profile.bio}
                                ></textarea>
                            </div>
                        </div>

                        <div class="mt-8 flex justify-end">
                            <button
                                class="px-6 py-2.5 bg-brand-orange hover:bg-brand-orange-hover text-white rounded-lg font-bold shadow-lg shadow-brand-orange/20 transition-all"
                            >
                                Salvar Alterações
                            </button>
                        </div>
                    </div>
                {:else if activeTab === "servicos"}
                    <!-- MEUS SERVIÇOS -->
                    <div class="animate-fade-in space-y-6">
                        <div class="flex justify-between items-center">
                            <h3
                                class="text-lg font-bold text-gray-900 dark:text-white"
                            >
                                Serviços Habilitados
                            </h3>
                            <!-- Catalog management restored -->
                            <button
                                on:click={openSelectionModal}
                                class="text-primary font-medium hover:underline text-sm flex items-center"
                            >
                                <span class="material-symbols-outlined text-sm mr-1">add</span>
                                Adicionar Serviço
                            </button>
                        </div>

                        <div
                            class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4"
                        >
                            {#if servicesLoading}
                                <div
                                    class="col-span-full flex justify-center py-8"
                                >
                                    <span
                                        class="material-symbols-outlined animate-spin text-brand-orange text-3xl"
                                        >sync</span
                                    >
                                </div>
                            {:else if services.length === 0}
                                <div
                                    class="col-span-full text-center py-8 text-gray-500"
                                >
                                    Nenhum serviço encontrado.
                                </div>
                            {:else}
                                {#each services as service}
                                    <div
                                        class="relative flex items-start p-4 rounded-xl border border-border-light dark:border-border-dark bg-[hsl(var(--bs-card))] shadow-sm transition-all hover:border-brand-orange/50 group"
                                    >
                                        {#if service.ImagemUrl}
                                            <div
                                                class="h-12 w-12 rounded-lg bg-gray-100 overflow-hidden shrink-0 mr-3 border border-gray-200 dark:border-gray-700"
                                            >
                                                <img
                                                    src={service.ImagemUrl}
                                                    alt={service.Nome}
                                                    class="h-full w-full object-cover"
                                                />
                                            </div>
                                        {/if}

                                        <div class="min-w-0 flex-1">
                                            <div
                                                class="flex justify-between items-start mb-1"
                                            >
                                                <h4
                                                    class="font-bold text-gray-900 dark:text-white text-sm line-clamp-1"
                                                    title={service.Nome}
                                                >
                                                    {service.Nome}
                                                </h4>
                                                <span
                                                    class="text-brand-orange font-bold text-sm whitespace-nowrap ml-2"
                                                >
                                                    R$ {(service.Preco / 100)
                                                        .toFixed(2)
                                                        .replace(".", ",")}
                                                </span>
                                            </div>
                                            <p
                                                class="text-gray-500 dark:text-gray-400 text-xs"
                                            >
                                                {service.DuracaoPadrao} min • {service.Categoria}
                                            </p>
                                        </div>
                                        <div class="ml-2 flex items-center self-center">
                                            <button 
                                                on:click={() => handleRemoveService(service)}
                                                class="p-1.5 text-gray-400 hover:text-red-500 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/10 transition-colors"
                                                title="Remover Serviço"
                                            >
                                                <span class="material-symbols-outlined text-[20px]">delete</span>
                                            </button>
                                        </div>
                                    </div>
                                {/each}
                            {/if}
                        </div>
                    </div>
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

<style>
    .animate-fade-in {
        animation: fadeIn 0.3s ease-in-out;
    }
    @keyframes fadeIn {
        from {
            opacity: 0;
            transform: translateY(10px);
        }
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>
