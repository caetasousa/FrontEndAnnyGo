<script lang="ts">
    import { onMount } from "svelte";
    import Sidebar from "$lib/components/Sidebar.svelte";
    import DashboardNavbar from "$lib/components/DashboardNavbar.svelte";

    // Types
    interface Appointment {
        id: string;
        data: string; // YYYY-MM-DD
        data_inicio: string; // ISO
        data_fim: string; // ISO
        hora_inicio: string; // HH:mm:ss
        hora_fim: string; // HH:mm:ss
        status: string | number;
        notas?: string;
        cliente: {
            id: string;
            nome: string;
            email: string;
            telefone: string;
        };
        prestador: {
            id: string;
            nome: string;
            cpf: string;
            email: string;
            telefone: string;
            ativo: boolean;
        };
        servico: {
            id: string;
            nome: string;
            duracao: number;
            preco: number;
            categoria: string;
        };
    }

    interface Stats {
        total: number;
        pendentes: number;
        confirmados: number;
        concluidos: number;
    }

    // State
    const clientId = "d5du4rcb85jv9boc1ch0"; // Fixed ID
    let appointments: Appointment[] = [];
    let todayAppointments: Appointment[] = [];
    let stats: Stats = {
        total: 0,
        pendentes: 0,
        confirmados: 0,
        concluidos: 0,
    };
    let loading = true;

    // Calendar state
    let selectedDate = new Date();
    let currentMonth = selectedDate.getMonth();
    let currentYear = selectedDate.getFullYear();

    // Monthly Grid Logic
    $: daysInCurrentMonth = new Date(
        currentYear,
        currentMonth + 1,
        0,
    ).getDate();
    $: firstDayOfWeek = new Date(currentYear, currentMonth, 1).getDay();

    // Create array of days for grid (padding empty slots)
    // 0 = null padding, number = day of month
    $: calendarDays = [
        ...Array(firstDayOfWeek).fill(null),
        ...Array.from({ length: daysInCurrentMonth }, (_, i) => i + 1),
    ];

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
    const dayNames = ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sáb"];

    // Utilities
    function formatDateString(date: Date): string {
        const year = date.getFullYear();
        const month = String(date.getMonth() + 1).padStart(2, "0");
        const day = String(date.getDate()).padStart(2, "0");
        return `${year}-${month}-${day}`;
    }

    function isToday(date: Date): boolean {
        const today = new Date();
        return formatDateString(date) === formatDateString(today);
    }

    function isPastDate(day: number, month: number, year: number): boolean {
        if (!day) return false;
        const checkDate = new Date(year, month, day);
        const today = new Date();
        today.setHours(0, 0, 0, 0); // Reset time to compare dates only
        return checkDate < today;
    }

    // Navigation
    function changeMonth(step: number) {
        // Prevent going to past months
        const today = new Date();
        if (
            step < 0 &&
            currentYear === today.getFullYear() &&
            currentMonth === today.getMonth()
        ) {
            return;
        }

        currentMonth += step;
        if (currentMonth > 11) {
            currentMonth = 0;
            currentYear++;
        } else if (currentMonth < 0) {
            currentMonth = 11;
            currentYear--;
        }
        // Fetch new month data
        const date = new Date(currentYear, currentMonth, 1);
        fetchAppointmentsForMonth(date);
    }

    function selectDate(day: number) {
        if (!day) return;
        if (isPastDate(day, currentMonth, currentYear)) return;

        const newDate = new Date(currentYear, currentMonth, day);
        selectedDate = newDate;
        updateTodayAppointments();
    }

    function isSelectedDay(
        day: number,
        selected: Date,
        month: number,
        year: number,
    ): boolean {
        if (!day) return false;
        const checkDate = new Date(year, month, day);
        return formatDateString(checkDate) === formatDateString(selected);
    }

    function isTodayDay(day: number): boolean {
        if (!day) return false;
        const checkDate = new Date(currentYear, currentMonth, day);
        return isToday(checkDate);
    }

    // API
    async function fetchAppointmentsForMonth(date: Date) {
        loading = true;
        try {
            // Calculate start and end of the month for the given date's month
            const year = date.getFullYear();
            const month = date.getMonth();

            const today = new Date();
            today.setHours(0, 0, 0, 0);

            let startDate = new Date(year, month, 1);

            // If current month, start from today to avoid fetching past days unnecessary
            if (year === today.getFullYear() && month === today.getMonth()) {
                startDate = today;
            }

            const endDate = new Date(year, month + 1, 0); // Last day of month

            const startStr = formatDateString(startDate);
            const endStr = formatDateString(endDate);

            // Calculate limit based on days difference (approx days + buffer or strict days?)
            // User requested not to be fixed at 1000. Matching the "days" count or a reasonable multiple.
            // Using days count as per User's earlier example (limit=31 for 31 days).
            const diffTime = Math.abs(endDate.getTime() - startDate.getTime());
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)) + 1;

            const response = await fetch(
                `/api/v1/agendamentos/cliente/${clientId}/periodo?data_inicio=${startStr}&data_fim=${endStr}&page=1&limit=${diffDays}`,
            );

            if (response.ok) {
                const json = await response.json();
                const rawData = Array.isArray(json.data) ? json.data : [];

                appointments = rawData.map((item: any) => {
                    let data = item.data;

                    if (!data && item.data_inicio) {
                        data = item.data_inicio.split("T")[0];
                    }

                    const hora_inicio = item.data_inicio
                        ? item.data_inicio.split("T")[1].substring(0, 5)
                        : "00:00";

                    const hora_fim = item.data_fim
                        ? item.data_fim.split("T")[1].substring(0, 5)
                        : "00:00";

                    let status = item.status;
                    if (typeof status === "number") {
                        const statusMap: Record<number, string> = {
                            1: "pendente",
                            2: "confirmado",
                            3: "concluido",
                            4: "cancelado",
                        };
                        status = statusMap[status] || "pendente";
                    }

                    return {
                        ...item,
                        data,
                        hora_inicio,
                        hora_fim,
                        status,
                    };
                });

                updateTodayAppointments();
            } else {
                console.error("Failed to fetch appointments");
                appointments = [];
                todayAppointments = [];
            }
        } catch (error) {
            console.error("Error fetching appointments:", error);
            appointments = [];
            todayAppointments = [];
        } finally {
            loading = false;
        }
    }

    function updateTodayAppointments() {
        if (!selectedDate) return;
        const dateStr = formatDateString(selectedDate);
        todayAppointments = appointments.filter((apt) => apt.data === dateStr);
        calculateStats();
    }

    function calculateStats() {
        // This stats logic might need adjustment if api returns only one day.
        // For now, let's just count what we have for the current view (day).
        // Real-world, you might want a separate endpoint for monthly stats.
        stats.total = todayAppointments.length;
        stats.pendentes = todayAppointments.filter(
            (a) => a.status === "pendente",
        ).length;
        stats.confirmados = todayAppointments.filter(
            (a) => a.status === "confirmado",
        ).length;
        stats.concluidos = todayAppointments.filter(
            (a) => a.status === "concluido",
        ).length;
    }

    async function cancelAppointment(appointmentId: string) {
        if (!confirm("Tem certeza que deseja cancelar este agendamento?"))
            return;

        try {
            const response = await fetch(
                `/api/v1/agendamentos/${appointmentId}/cancelar`,
                { method: "PUT" },
            );

            if (response.ok) {
                await fetchAppointmentsForMonth(selectedDate); // Refresh
            } else {
                alert("Erro ao cancelar agendamento");
            }
        } catch (error) {
            console.error("Error canceling:", error);
        }
    }

    // Formatting
    function getStatusLabel(status: string): string {
        const labels: Record<string, string> = {
            pendente: "Aguardando Confirmação",
            confirmado: "Confirmado",
            cancelado: "Cancelado",
            concluido: "Concluído",
        };
        return labels[status] || status;
    }

    function getStatusClasses(status: string) {
        const classes: Record<
            string,
            { border: string; bg: string; text: string; badge: string }
        > = {
            pendente: {
                border: "border-l-warning",
                bg: "bg-warning/10",
                text: "text-warning",
                badge: "bg-warning/20 text-warning",
            },
            confirmado: {
                border: "border-l-success",
                bg: "bg-success/10",
                text: "text-success",
                badge: "bg-success/20 text-success",
            },
            cancelado: {
                border: "border-l-gray-400",
                bg: "bg-gray-200 dark:bg-gray-700",
                text: "text-gray-500",
                badge: "bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300",
            },
            concluido: {
                border: "border-l-gray-400",
                bg: "bg-gray-200 dark:bg-gray-700",
                text: "text-gray-500",
                badge: "bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300",
            },
        };
        return classes[status] || classes.pendente;
    }

    onMount(() => {
        fetchAppointmentsForMonth(selectedDate);
    });
</script>

<div
    class="font-body bg-[hsl(var(--bs-background))] text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200"
>
    <!-- Assuming Client also has sidebar? Or maybe a different layout. 
         Using standard Sidebar for now based on request to be "simiar". -->
    <Sidebar />
    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <DashboardNavbar />
        <div class="flex-1 overflow-y-auto p-6 md:p-8">
            <div class="max-w-6xl mx-auto space-y-6">
                <!-- Header -->
                <div
                    class="flex flex-col md:flex-row md:items-center justify-between gap-4"
                >
                    <div>
                        <nav
                            class="flex text-sm text-gray-500 dark:text-gray-400 mb-2"
                        >
                            <span
                                class="hover:text-primary transition-colors cursor-pointer"
                                >Meus Agendamentos</span
                            >
                        </nav>
                        <h1
                            class="text-2xl font-bold text-gray-900 dark:text-white"
                        >
                            Minha Agenda
                        </h1>
                        <p class="text-gray-500 dark:text-gray-400 mt-1">
                            Acompanhe seus horários marcados.
                        </p>
                    </div>
                    <button
                        class="flex items-center px-4 py-2 bg-primary hover:bg-primary-hover text-white rounded-md text-sm font-medium shadow-md transition-colors"
                        on:click={() => {
                            /* Navigate to booking flow? */
                        }}
                    >
                        <span class="material-symbols-outlined text-[20px] mr-2"
                            >add</span
                        >
                        Novo Agendamento
                    </button>
                </div>

                <!-- Stats Cards (Simplified for Client) -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
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
                            <p class="text-sm text-gray-500 dark:text-gray-400">
                                Total (Dia)
                            </p>
                            <p
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                {stats.total}
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
                                >hourglass_top</span
                            >
                        </div>
                        <div>
                            <p class="text-sm text-gray-500 dark:text-gray-400">
                                Pendentes
                            </p>
                            <p
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                {stats.pendentes}
                            </p>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Left: Calendar (Compact) -->
                    <div class="lg:col-span-1">
                        <div
                            class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-6 sticky top-6"
                        >
                            <div class="flex items-center justify-between mb-6">
                                <h2
                                    class="text-lg font-bold text-gray-900 dark:text-white capitalize"
                                >
                                    {monthNames[currentMonth]}
                                    {currentYear}
                                </h2>
                                <div class="flex space-x-1">
                                    <button
                                        on:click={() => changeMonth(-1)}
                                        class="w-8 h-8 flex items-center justify-center rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-300 transition-colors disabled:opacity-30 disabled:cursor-not-allowed disabled:hover:bg-transparent"
                                        disabled={currentYear ===
                                            new Date().getFullYear() &&
                                            currentMonth ===
                                                new Date().getMonth()}
                                    >
                                        <span
                                            class="material-symbols-outlined text-[20px]"
                                            >chevron_left</span
                                        >
                                    </button>
                                    <button
                                        on:click={() => changeMonth(1)}
                                        class="w-8 h-8 flex items-center justify-center rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-300 transition-colors"
                                    >
                                        <span
                                            class="material-symbols-outlined text-[20px]"
                                            >chevron_right</span
                                        >
                                    </button>
                                </div>
                            </div>

                            <div class="grid grid-cols-7 gap-1 text-center">
                                <!-- Weekday Headers -->
                                {#each dayNames as day}
                                    <div
                                        class="text-[10px] font-semibold text-gray-400 uppercase tracking-wider py-1"
                                    >
                                        {day}
                                    </div>
                                {/each}

                                <!-- Days -->
                                {#each calendarDays as day}
                                    {#if day}
                                        {@const isSelected = isSelectedDay(
                                            day,
                                            selectedDate,
                                            currentMonth,
                                            currentYear,
                                        )}
                                        {@const isToday = isTodayDay(day)}
                                        {@const isPast = isPastDate(
                                            day,
                                            currentMonth,
                                            currentYear,
                                        )}
                                        <button
                                            on:click={() => selectDate(day)}
                                            disabled={isPast}
                                            class="aspect-square relative flex items-center justify-center rounded-md transition-all text-sm
                                            {isPast
                                                ? 'opacity-30 cursor-not-allowed hover:bg-transparent text-gray-400'
                                                : ''}
                                            {!isPast && isSelected
                                                ? 'bg-primary text-white shadow-sm font-bold'
                                                : !isPast && isToday
                                                  ? 'bg-blue-100 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 font-bold border border-blue-200 dark:border-blue-800'
                                                  : !isPast
                                                    ? 'hover:bg-gray-50 dark:hover:bg-gray-800 text-gray-700 dark:text-gray-300'
                                                    : ''}"
                                        >
                                            <span>{day}</span>
                                            {#if isSelected && !isPast}
                                                <div
                                                    class="absolute bottom-1 w-1 h-1 bg-white rounded-full"
                                                ></div>
                                            {/if}
                                        </button>
                                    {:else}
                                        <div class="aspect-square"></div>
                                    {/if}
                                {/each}
                            </div>
                        </div>
                    </div>

                    <!-- Right: Appointments List -->
                    <div class="lg:col-span-2 space-y-4">
                        <h3
                            class="text-lg font-semibold text-gray-900 dark:text-white flex items-center"
                        >
                            <span
                                class="material-symbols-outlined text-primary mr-2"
                                >schedule</span
                            >
                            Agendamentos de {isToday(selectedDate)
                                ? "Hoje"
                                : selectedDate.toLocaleDateString("pt-BR", {
                                      day: "2-digit",
                                      month: "long",
                                  })}
                        </h3>

                        {#if loading}
                            <div class="flex justify-center py-12">
                                <span
                                    class="material-symbols-outlined animate-spin text-brand-orange text-3xl"
                                    >sync</span
                                >
                            </div>
                        {:else if todayAppointments.length === 0}
                            <div
                                class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-8 text-center"
                            >
                                <span
                                    class="material-symbols-outlined text-gray-400 text-5xl mb-2"
                                    >event_busy</span
                                >
                                <p class="text-gray-500 dark:text-gray-400">
                                    Nenhum agendamento encontrado para esta
                                    data.
                                </p>
                            </div>
                        {:else}
                            {#each todayAppointments as apt}
                                {@const statusClasses = getStatusClasses(
                                    apt.status.toString(),
                                )}
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border-l-4 {statusClasses.border} border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md"
                                >
                                    <div
                                        class="flex flex-col sm:flex-row justify-between items-start sm:items-center"
                                    >
                                        <!-- Left Side: Time & Provider Info -->
                                        <div
                                            class="flex items-start space-x-4 mb-4 sm:mb-0"
                                        >
                                            <div
                                                class="{statusClasses.bg} {statusClasses.text} p-2 rounded-full"
                                            >
                                                <span
                                                    class="material-symbols-outlined"
                                                >
                                                    {#if apt.status === "pendente"}
                                                        hourglass_top
                                                    {:else if apt.status === "confirmado"}
                                                        check
                                                    {:else if apt.status === "concluido"}
                                                        done_all
                                                    {:else}
                                                        close
                                                    {/if}
                                                </span>
                                            </div>
                                            <div>
                                                <div
                                                    class="flex items-center flex-wrap gap-2"
                                                >
                                                    <span
                                                        class="text-lg font-bold text-gray-900 dark:text-white"
                                                    >
                                                        {apt.hora_inicio} - {apt.hora_fim}
                                                    </span>
                                                    <span
                                                        class="hidden sm:inline text-gray-400"
                                                        >•</span
                                                    >
                                                    <span
                                                        class="text-sm text-gray-600 dark:text-gray-300 font-medium"
                                                    >
                                                        {apt.servico?.nome ||
                                                            "Serviço"}
                                                    </span>
                                                    {#if apt.status === "confirmado"}
                                                        <span
                                                            class="text-xs {statusClasses.badge} px-2 py-0.5 rounded-full"
                                                        >
                                                            {getStatusLabel(
                                                                apt.status.toString(),
                                                            )}
                                                        </span>
                                                    {/if}
                                                </div>

                                                <p
                                                    class="text-sm text-gray-500 dark:text-gray-400 mt-1"
                                                >
                                                    Profissional: <span
                                                        class="font-medium text-gray-800 dark:text-gray-200"
                                                        >{apt.prestador
                                                            ?.nome}</span
                                                    >
                                                </p>

                                                <div
                                                    class="flex items-center mt-2 text-xs text-gray-400"
                                                >
                                                    <span
                                                        class="material-symbols-outlined text-[16px] mr-1"
                                                        >attach_money</span
                                                    >
                                                    <span
                                                        >R$ {(
                                                            (apt.servico
                                                                ?.preco || 0) /
                                                            100
                                                        )
                                                            .toFixed(2)
                                                            .replace(
                                                                ".",
                                                                ",",
                                                            )}</span
                                                    >
                                                    <span class="mx-2">•</span>
                                                    <span
                                                        class="material-symbols-outlined text-[16px] mr-1"
                                                        >timer</span
                                                    >
                                                    <span
                                                        >{apt.servico
                                                            ?.duracao || 60} min</span
                                                    >
                                                </div>
                                            </div>
                                        </div>

                                        <!-- Right Side: Actions -->
                                        <div
                                            class="flex space-x-2 w-full sm:w-auto"
                                        >
                                            {#if apt.status === "pendente"}
                                                <button
                                                    on:click={() =>
                                                        cancelAppointment(
                                                            apt.id,
                                                        )}
                                                    class="flex-1 sm:flex-none px-3 py-1.5 border border-red-300 text-red-600 rounded text-sm hover:bg-red-50 transition-colors"
                                                >
                                                    Cancelar
                                                </button>
                                            {/if}
                                        </div>
                                    </div>
                                </div>
                            {/each}
                        {/if}
                    </div>
                </div>
            </div>
        </div>
    </main>
</div>
