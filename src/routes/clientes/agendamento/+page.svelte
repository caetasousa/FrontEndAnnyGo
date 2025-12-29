<script lang="ts">
    import Sidebar from "$lib/components/Sidebar.svelte";
    import { onMount } from "svelte";
    import { page } from "$app/stores";

    // --- Configuração ---
    const CLIENTE_ID = "d599id914a3on4b8osn0"; // Cliente Mockado

    // --- Estado ---
    let professionals: any[] = [];
    let services: any[] = [];
    let selectedProfessional: any = null;
    let selectedService: any = null; // Serviço principal selecionado (hardcoded first for now if list loaded)
    let preSelectedServiceId: string | null = null;

    // Estado do Formulário
    let loading = true;
    let submitting = false;
    let error = "";
    let successMessage = "";

    // Mock para Addons (ainda estático, pois não temos endpoint claro de addons vinculados a serviço ainda)
    const addons = [
        {
            name: "Design de Sobrancelhas",
            duration: "30 min",
            price: "45,00",
            icon: "brush",
            color: "pink",
        },
        {
            name: "Hidratação Facial",
            duration: "20 min",
            price: "60,00",
            icon: "spa",
            color: "purple",
        },
        {
            name: "Massagem nas Mãos",
            duration: "15 min",
            price: "30,00",
            icon: "local_drink",
            color: "blue",
        },
    ];

    // Slots mockados (Ideally should come from API based on availabiltiy)
    const morningSlots = ["09:00", "09:30", "10:00", "11:30"];
    const afternoonSlots = ["13:00", "14:30", "15:00", "16:00", "16:30"];
    let selectedSlot = "13:00";

    // --- State for theme ---
    let isDark = false;

    // --- Calendar Logic ---
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
    const today = new Date();
    let currentMonth = today.getMonth();
    let currentYear = today.getFullYear();
    let selectedDay: number | null = null;

    function getDaysInMonth(month: number, year: number) {
        return new Date(year, month + 1, 0).getDate();
    }

    function getFirstDayOfMonth(month: number, year: number) {
        return new Date(year, month, 1).getDay();
    }

    $: daysInCurrentMonth = getDaysInMonth(currentMonth, currentYear);
    $: firstDayOfWeek = getFirstDayOfMonth(currentMonth, currentYear);

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

    function selectDay(day: number) {
        if (!day) return;
        const date = new Date(currentYear, currentMonth, day);
        const now = new Date();
        now.setHours(0, 0, 0, 0);

        if (date < now) return; // Prevent selecting past days
        selectedDay = day;
    }

    function isPastAndNotToday(day: number) {
        if (!day) return false;
        const date = new Date(currentYear, currentMonth, day);
        const now = new Date();
        now.setHours(0, 0, 0, 0);
        return date < now;
    }

    // --- API Interactions ---

    async function fetchData() {
        loading = true;
        try {
            preSelectedServiceId = $page.url.searchParams.get("serviceId");

            // 1. Fetch Serviço (Pre-selected or List)
            if (preSelectedServiceId) {
                const respService = await fetch(
                    `/api/v1/catalogos/${preSelectedServiceId}`,
                );
                if (respService.ok) {
                    const data = await respService.json();
                    selectedService = data;
                    // No need to fetch list of services if we have one selected, unless specific UI requirements
                }
            } else {
                const respServicos = await fetch("/api/v1/catalogos?limit=5");
                if (respServicos.ok) {
                    const data = await respServicos.json();
                    services = data.data || [];
                    if (services.length > 0) {
                        selectedService = services[0];
                    }
                }
            }

            // 2. Fetch Prestadores
            const respPrestadores = await fetch("/api/v1/prestadores");
            if (respPrestadores.ok) {
                const data = await respPrestadores.json();
                let allProfessionals = data.data || [];

                // Filter if service is selected
                if (selectedService) {
                    professionals = allProfessionals.filter((p: any) => {
                        // Check if provider has this service in their catalog
                        // Note: Assuming 'catalogo' is an array of services or objects with ID
                        // Need to be robust: p.catalogo may be array of objects (full service) or IDs
                        if (!p || !p.catalogo || !Array.isArray(p.catalogo))
                            return false;
                        return p.catalogo.some(
                            (s: any) =>
                                s.id === selectedService.id ||
                                s.ID === selectedService.ID ||
                                s.id === selectedService.ID ||
                                s.ID === selectedService.id,
                        );
                    });
                } else {
                    professionals = allProfessionals;
                }

                if (professionals.length > 0) {
                    selectedProfessional = professionals[0];
                }
            }
        } catch (e) {
            console.error("Erro ao carregar dados:", e);
            error = "Não foi possível carregar as informações.";
        } finally {
            loading = false;
        }
    }

    async function handleConfirmAppointment() {
        if (
            !selectedProfessional ||
            !selectedService ||
            !selectedDay ||
            !selectedSlot
        ) {
            alert("Por favor, selecione todas as opções.");
            return;
        }

        submitting = true;
        error = "";
        successMessage = "";

        // Construct Date: YYYY-MM-DDTHH:MM:00Z
        // Note: simplistic construction, ignoring timezone issues for MVP, assume local -> UTC or send local ISO
        const year = currentYear;
        const month = String(currentMonth + 1).padStart(2, "0");
        const day = String(selectedDay).padStart(2, "0");
        const dateTimeString = `${year}-${month}-${day}T${selectedSlot}:00Z`;

        const payload = {
            catalogo_id: selectedService.ID || selectedService.id, // Handle potential case variance
            cliente_id: CLIENTE_ID,
            data_hora_inicio: dateTimeString,
            notas: "Agendado via Web",
            prestador_id: selectedProfessional.id || selectedProfessional.ID, // Handle potential case variance
        };

        try {
            const response = await fetch("/api/v1/agendamentos", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(payload),
            });

            if (response.ok) {
                successMessage = "Agendamento realizado com sucesso!";
                // Optional: Redirect or reset
                setTimeout(() => {
                    // goto('/clientes/agendamentos'); // Se existir
                    successMessage = ""; // Clear for now
                }, 3000);
            } else {
                const text = await response.text();
                console.error("Erro no agendamento:", text);
                error = "Erro ao agendar: " + text;
            }
        } catch (e) {
            console.error("Erro de conexão:", e);
            error = "Erro de conexão ao tentar agendar.";
        } finally {
            submitting = false;
        }
    }

    onMount(() => {
        // Theme check
        if (document.documentElement.classList.contains("dark")) {
            isDark = true;
        }
        // Load Data
        fetchData();
    });

    function toggleTheme() {
        isDark = !isDark;
        if (isDark) {
            document.documentElement.classList.add("dark");
        } else {
            document.documentElement.classList.remove("dark");
        }
    }
</script>

<div
    class="flex h-screen bg-[hsl(var(--bs-background))] text-text-light dark:text-text-dark font-body overflow-hidden transition-colors duration-200"
>
    <!-- Sidebar -->
    <Sidebar />

    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <!-- Header -->
        <header
            class="h-16 bg-[hsl(var(--bs-card))] border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10 flex-shrink-0"
        >
            <div class="flex items-center flex-1 max-w-2xl">
                <div class="relative w-full">
                    <span
                        class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
                    >
                        <span class="material-icons text-gray-400">search</span>
                    </span>
                    <input
                        type="text"
                        class="block w-full pl-10 pr-3 py-2 border border-border-light dark:border-border-dark rounded-md leading-5 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-1 focus:ring-brand focus:border-brand-orange sm:text-sm"
                        placeholder="Pesquisar serviços ou profissionais..."
                    />
                </div>
            </div>
            <div class="flex items-center space-x-4 ml-4">
                <button
                    class="p-2 rounded-full text-gray-400 hover:text-gray-500 dark:hover:text-gray-300 focus:outline-none transition-colors"
                >
                    <span class="material-icons">notifications</span>
                </button>
                <button
                    class="p-2 rounded-full text-gray-400 hover:text-gray-500 dark:hover:text-gray-300 focus:outline-none transition-colors"
                    on:click={toggleTheme}
                >
                    <span
                        class="material-symbols-outlined {isDark
                            ? 'hidden'
                            : 'block'}">dark_mode</span
                    >
                    <span
                        class="material-symbols-outlined {isDark
                            ? 'block'
                            : 'hidden'}">light_mode</span
                    >
                </button>
            </div>
        </header>

        <!-- Content -->
        <div class="flex-1 overflow-y-auto p-4 md:p-6 lg:p-8">
            <div class="max-w-6xl mx-auto space-y-6">
                <!-- Notifications -->
                {#if error}
                    <div
                        class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative"
                        role="alert"
                    >
                        <strong class="font-bold">Erro!</strong>
                        <span class="block sm:inline">{error}</span>
                    </div>
                {/if}
                {#if successMessage}
                    <div
                        class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative"
                        role="alert"
                    >
                        <strong class="font-bold">Sucesso!</strong>
                        <span class="block sm:inline">{successMessage}</span>
                    </div>
                {/if}

                <!-- Breadcrumbs & Title -->
                <div>
                    <nav
                        class="flex text-sm text-gray-500 dark:text-gray-400 mb-2"
                    >
                        <a
                            href="/"
                            class="hover:text-brand-orange transition-colors"
                            >Início</a
                        >
                        <span class="mx-2">/</span>
                        <span class="text-gray-900 dark:text-white font-medium"
                            >Agendar Serviço</span
                        >
                    </nav>
                    <h1
                        class="text-2xl font-bold text-gray-900 dark:text-white"
                    >
                        Selecione Data e Horário
                    </h1>
                    <p class="text-gray-500 dark:text-gray-400 mt-1">
                        Finalize o agendamento para o seu momento de cuidado.
                    </p>
                </div>

                {#if loading}
                    <div class="flex justify-center py-12">
                        <span
                            class="material-icons animate-spin text-brand-orange text-4xl"
                            >sync</span
                        >
                    </div>
                {:else}
                    <!-- Main Grid -->
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                        <!-- Left Column (Service & Professional) -->
                        <div class="lg:col-span-1 space-y-6">
                            <!-- Service Card (Display Selected Service) -->
                            {#if selectedService}
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark overflow-hidden"
                                >
                                    <div
                                        class="h-32 bg-gray-200 dark:bg-gray-700 relative group"
                                    >
                                        {#if selectedService.ImagemUrl || selectedService.image_url}
                                            <img
                                                src={selectedService.ImagemUrl ||
                                                    selectedService.image_url}
                                                alt={selectedService.Nome ||
                                                    selectedService.nome}
                                                class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
                                            />
                                        {:else}
                                            <div
                                                class="w-full h-full flex items-center justify-center text-gray-400"
                                            >
                                                <span
                                                    class="material-symbols-outlined text-4xl"
                                                    >image</span
                                                >
                                            </div>
                                        {/if}
                                        <div
                                            class="absolute inset-0 bg-black/30 flex items-center justify-center"
                                        >
                                            <h3
                                                class="text-white font-bold text-lg drop-shadow-md text-center px-4"
                                            >
                                                {selectedService.Nome ||
                                                    selectedService.nome}
                                            </h3>
                                        </div>
                                    </div>
                                    <div class="p-5">
                                        <div
                                            class="flex justify-between items-start mb-4"
                                        >
                                            <div>
                                                <p
                                                    class="text-sm text-gray-500 dark:text-gray-400"
                                                >
                                                    Categoria
                                                </p>
                                                <p
                                                    class="font-medium text-gray-900 dark:text-white"
                                                >
                                                    {selectedService.Categoria ||
                                                        selectedService.categoria}
                                                </p>
                                            </div>
                                            <div class="text-right">
                                                <p
                                                    class="text-sm text-gray-500 dark:text-gray-400"
                                                >
                                                    Duração
                                                </p>
                                                <p
                                                    class="font-medium text-gray-900 dark:text-white"
                                                >
                                                    {selectedService.DuracaoPadrao ||
                                                        selectedService.duracao_padrao}
                                                    min
                                                </p>
                                            </div>
                                        </div>
                                        <div
                                            class="flex justify-between items-center border-t border-border-light dark:border-border-dark pt-4"
                                        >
                                            <span
                                                class="text-lg font-bold text-brand-orange"
                                                >R$ {(
                                                    (selectedService.Preco ||
                                                        selectedService.preco) /
                                                    100
                                                )
                                                    .toFixed(2)
                                                    .replace(".", ",")}</span
                                            >
                                            <button
                                                class="text-sm text-brand-orange hover:text-brand-orange-hover font-medium transition-colors"
                                                >Alterar Serviço</button
                                            >
                                        </div>
                                    </div>
                                </div>
                            {/if}

                            <!-- Professional Selection -->
                            <div
                                class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-5"
                            >
                                <h3
                                    class="font-semibold text-gray-900 dark:text-white mb-4"
                                >
                                    Profissional
                                </h3>
                                {#if !selectedDay}
                                    <div class="flex flex-col items-center justify-center py-8 text-center px-4">
                                        <span class="material-icons text-4xl text-gray-300 dark:text-gray-600 mb-2">event</span>
                                        <p class="text-gray-500 dark:text-gray-400 font-medium">
                                            Escolha um dia no calendário
                                        </p>
                                        <p class="text-xs text-gray-400 dark:text-gray-500 mt-1">
                                            Para ver os profissionais e horários disponíveis.
                                        </p>
                                    </div>
                                {:else}
                                    {#if professionals.length === 0}
                                        <p class="text-sm text-gray-500">
                                            Nenhum profissional encontrado.
                                        </p>
                                    {:else}
                                        <div class="space-y-3">
                                            {#each professionals as prof}
                                                <label
                                                    class="flex items-center p-3 rounded-md border cursor-pointer transition-all duration-200
                                                    {selectedProfessional &&
                                                    (selectedProfessional.id ===
                                                        prof?.id ||
                                                        selectedProfessional.ID ===
                                                            prof?.ID)
                                                        ? 'border-brand-orange bg-brand-orange/5'
                                                        : 'border-border-light dark:border-border-dark hover:bg-gray-50 dark:hover:bg-gray-800'}"
                                                >
                                                    <input
                                                        type="radio"
                                                        name="professional"
                                                        value={prof}
                                                        bind:group={
                                                            selectedProfessional
                                                        }
                                                        class="form-radio h-4 w-4 text-brand-orange border-gray-300 focus:ring-brand"
                                                    />
                                                    <div
                                                        class="ml-3 flex items-center"
                                                    >
                                                        {#if prof?.image_url || prof?.avatar}
                                                            <img
                                                                src={prof?.image_url ||
                                                                    prof?.avatar}
                                                                alt={prof?.name ||
                                                                    prof?.nome}
                                                                class="h-8 w-8 rounded-full object-cover border border-gray-200 dark:border-gray-700"
                                                                on:error={(e) => {
                                                                    (
                                                                        e.target as HTMLImageElement
                                                                    ).src =
                                                                        "https://ui-avatars.com/api/?name=" +
                                                                        (prof?.name ||
                                                                            prof?.nome);
                                                                }}
                                                            />
                                                        {:else}
                                                            <div
                                                                class="h-8 w-8 rounded-full bg-gray-300 flex items-center justify-center text-gray-600 font-bold text-xs"
                                                            >
                                                                {(
                                                                    prof?.name ||
                                                                    prof?.nome ||
                                                                    "?"
                                                                )
                                                                    .substring(0, 2)
                                                                    .toUpperCase()}
                                                            </div>
                                                        {/if}
                                                        <div class="ml-2">
                                                            <span
                                                                class="block text-sm font-medium text-gray-900 dark:text-white"
                                                                >{prof?.name ||
                                                                    prof?.nome}</span
                                                            >
                                                            <span
                                                                class="block text-xs text-gray-500 dark:text-gray-400"
                                                                >{prof?.role ||
                                                                    prof?.especialidade ||
                                                                    "Especialista"}</span
                                                            >
                                                        </div>

                                                </div>
                                            </label>
                                        {/each}
                                    </div>
                                {/if}
                                {/if}
                            </div>
                        </div>

                        <!-- Right Column (Calendar & Slots) -->
                        <div class="lg:col-span-2 space-y-6">
                            <!-- Calendar -->
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
                                            class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-400 transition-colors"
                                        >
                                            <span class="material-icons"
                                                >chevron_left</span
                                            >
                                        </button>
                                        <button
                                            on:click={() => changeMonth(1)}
                                            class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-400 transition-colors"
                                        >
                                            <span class="material-icons"
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

                                    <!-- Days Grid -->
                                    {#each calendarDays as day}
                                        {#if day}
                                            {@const dateObj = new Date(
                                                currentYear,
                                                currentMonth,
                                                day,
                                            )}
                                            {@const dayOfWeek =
                                                dateObj.getDay()}
                                            {@const isPast =
                                                isPastAndNotToday(day)}
                                            {@const isSelected =
                                                selectedDay === day}
                                            {@const isSunday = dayOfWeek === 0}
                                            {@const isSaturday =
                                                dayOfWeek === 6}

                                            <button
                                                disabled={isPast || isSunday}
                                                on:click={() => selectDay(day)}
                                                class="aspect-square relative flex items-start justify-end p-2 rounded-lg border transition-all group
                                            {isPast
                                                    ? 'opacity-40 cursor-not-allowed grayscale bg-transparent'
                                                    : 'hover:border-brand-orange bg-gray-50 dark:bg-[hsl(var(--bs-muted))]/20'}
                                            {isSelected
                                                    ? '!bg-blue-100 dark:!bg-blue-900/40 !border-blue-500 ring-1 ring-blue-500'
                                                    : 'border-transparent'}
                                            {isSunday && !isPast
                                                    ? 'bg-red-50/50 dark:bg-red-900/10'
                                                    : ''}
                                            {isSaturday &&
                                                !isPast &&
                                                !isSelected
                                                    ? 'bg-orange-50 dark:bg-orange-900/10'
                                                    : ''}
                                            "
                                            >
                                                <span
                                                    class="z-10 text-sm font-medium
                                                {isSunday ? 'text-red-500' : ''}
                                                {isSaturday && !isSelected
                                                        ? 'text-orange-600 dark:text-orange-400'
                                                        : ''}
                                                {isSelected
                                                        ? 'text-blue-600 dark:text-blue-300'
                                                        : !isSunday &&
                                                            !isSaturday
                                                          ? 'text-gray-700 dark:text-gray-300'
                                                          : ''}
                                                "
                                                >
                                                    {day}
                                                </span>

                                                <!-- Status indicator / Label -->
                                                <div
                                                    class="absolute inset-x-1 bottom-1 flex flex-col items-center"
                                                >
                                                    {#if !isPast && !isSunday}
                                                        <div
                                                            class="w-1.5 h-1.5 rounded-full {isSelected
                                                                ? 'bg-blue-500'
                                                                : isSaturday
                                                                  ? 'bg-orange-400'
                                                                  : 'bg-green-400'} mb-1"
                                                        ></div>
                                                    {/if}
                                                </div>
                                            </button>
                                        {:else}
                                            <div></div>
                                        {/if}
                                    {/each}
                                </div>
                            </div>

                            <!-- Slots -->
                            {#if selectedDay}
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-6"
                                >
                                    <h3
                                        class="font-semibold text-gray-900 dark:text-white mb-4"
                                    >
                                        Horários Disponíveis
                                        <span
                                            class="text-sm font-normal text-gray-500 dark:text-gray-400 ml-2"
                                            >{selectedDay}/{currentMonth+1}</span
                                        >
                                    </h3>

                                    <div class="space-y-4">
                                        <div>
                                            <h4
                                                class="text-xs font-bold text-gray-500 uppercase tracking-wider mb-2"
                                            >
                                                Manhã
                                            </h4>
                                            <div
                                                class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-6 gap-3"
                                            >
                                                {#each morningSlots as slot}
                                                    <button
                                                        class="px-3 py-2 text-sm font-medium border rounded-md transition-all duration-200
                                                        {slot === '09:30'
                                                            ? 'bg-gray-100 dark:bg-gray-800 text-gray-400 dark:text-gray-600 cursor-not-allowed border-border-light dark:border-border-dark'
                                                            : ''}
                                                        {selectedSlot === slot
                                                            ? 'border-brand-orange bg-brand-orange text-white shadow-sm'
                                                            : ''}
                                                        {selectedSlot !== slot &&
                                                        slot !== '09:30'
                                                            ? 'border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:border-brand-orange hover:text-brand-orange dark:hover:text-brand-orange'
                                                            : ''}"
                                                        disabled={slot === "09:30"}
                                                        on:click={() =>
                                                            (selectedSlot = slot)}
                                                    >
                                                        {slot}
                                                    </button>
                                                {/each}
                                            </div>
                                        </div>

                                        <div>
                                            <h4
                                                class="text-xs font-bold text-gray-500 uppercase tracking-wider mb-2"
                                            >
                                                Tarde
                                            </h4>
                                            <div
                                                class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-6 gap-3"
                                            >
                                                {#each afternoonSlots as slot}
                                                    <button
                                                        class="px-3 py-2 text-sm font-medium border rounded-md transition-all duration-200
                                                        {slot === '16:00'
                                                            ? 'bg-gray-100 dark:bg-gray-800 text-gray-400 dark:text-gray-600 cursor-not-allowed border-border-light dark:border-border-dark'
                                                            : ''}
                                                        {selectedSlot === slot
                                                            ? 'border-brand-orange bg-brand-orange text-white shadow-sm'
                                                            : ''}
                                                        {selectedSlot !== slot &&
                                                        slot !== '16:00'
                                                            ? 'border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:border-brand-orange hover:text-brand-orange dark:hover:text-brand-orange'
                                                            : ''}"
                                                        disabled={slot === "16:00"}
                                                        on:click={() =>
                                                            (selectedSlot = slot)}
                                                    >
                                                        {slot}
                                                    </button>
                                                {/each}
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            {/if}

                            <!-- Add-ons -->
                            <div
                                class="mt-8 pt-8 border-t border-border-light dark:border-border-dark"
                            >
                                <h2
                                    class="text-lg font-bold text-gray-900 dark:text-white mb-4"
                                >
                                    Adicione ao seu tratamento
                                </h2>
                                <div
                                    class="grid grid-cols-1 md:grid-cols-3 gap-6"
                                >
                                    {#each addons as addon}
                                        <div
                                            class="flex items-center p-3 rounded-lg border border-border-light dark:border-border-dark bg-[hsl(var(--bs-card))] hover:shadow-md transition-all duration-200 cursor-pointer group"
                                        >
                                            <div
                                                class="h-12 w-12 rounded-lg bg-{addon.color}-100 flex items-center justify-center text-{addon.color}-500 mr-4 group-hover:bg-{addon.color}-200 transition-colors"
                                            >
                                                <span class="material-icons"
                                                    >{addon.icon}</span
                                                >
                                            </div>
                                            <div class="flex-1">
                                                <h4
                                                    class="font-medium text-gray-900 dark:text-white text-sm"
                                                >
                                                    {addon.name}
                                                </h4>
                                                <p
                                                    class="text-xs text-gray-500 dark:text-gray-400"
                                                >
                                                    + {addon.duration} • R$ {addon.price}
                                                </p>
                                            </div>
                                            <button
                                                class="h-8 w-8 rounded-full border border-gray-300 dark:border-gray-600 flex items-center justify-center text-gray-400 hover:text-brand-orange hover:border-brand-orange transition-colors"
                                            >
                                                <span
                                                    class="material-icons text-lg"
                                                    >add</span
                                                >
                                            </button>
                                        </div>
                                    {/each}
                                </div>
                            </div>

                            <!-- Actions -->
                            <div
                                class="flex items-center justify-end space-x-4 pt-4 border-t border-border-light dark:border-border-dark"
                            >
                                <button
                                    class="px-6 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-gray-700 dark:text-gray-300 font-medium hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"
                                >
                                    Cancelar
                                </button>
                                <button
                                    on:click={handleConfirmAppointment}
                                    disabled={submitting}
                                    class="px-8 py-2 bg-brand-orange hover:bg-brand-orange-hover text-white rounded-md font-medium shadow-md transition-all duration-200 flex items-center hover:shadow-lg disabled:opacity-70 disabled:cursor-not-allowed"
                                >
                                    {#if submitting}
                                        <span
                                            class="material-icons animate-spin mr-2 text-sm"
                                            >sync</span
                                        >
                                        Enviando...
                                    {:else}
                                        Confirmar Agendamento
                                        <span
                                            class="material-icons text-sm ml-2"
                                            >arrow_forward</span
                                        >
                                    {/if}
                                </button>
                            </div>
                        </div>
                    </div>
                {/if}
            </div>

            <!-- Footer inside main content to scroll together -->
            <footer
                class="mt-12 text-center text-sm text-gray-500 dark:text-gray-400 pb-6"
            >
                <p>© 2023 BellaVita. Todos os direitos reservados.</p>
                <div class="mt-2 space-x-4">
                    <a href="/" class="hover:text-brand transition-colors"
                        >Termos</a
                    >
                    <a href="/" class="hover:text-brand transition-colors"
                        >Privacidade</a
                    >
                </div>
            </footer>
        </div>
    </main>
</div>
