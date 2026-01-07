<script lang="ts">
    import { onMount } from "svelte";
    import Sidebar from "$lib/components/Sidebar.svelte";
    import DashboardNavbar from "$lib/components/DashboardNavbar.svelte";

    // Types
    interface Appointment {
        id: string;
        data: string;
        hora_inicio: string;
        hora_fim: string;
        status: "pendente" | "confirmado" | "cancelado" | "concluido";
        cliente_id: string;
        cliente_nome?: string;
        prestador_id: string;
        servico_id: string;
        servico_nome?: string;
        duracao?: number;
        preco?: number;
        servico?: {
            nome: string;
        };
    }

    interface Stats {
        hoje: number;
        pendentes: number;
        confirmados: number;
        receitaHoje: number;
    }

    // State
    let providerId = "d5dtvusb85jv9boc1cb0"; // Fixed ID
    let appointments: Appointment[] = [];
    let todayAppointments: Appointment[] = [];
    let stats: Stats = {
        hoje: 0,
        pendentes: 0,
        confirmados: 0,
        receitaHoje: 0,
    };
    let loading = true;

    // Calendar state
    let selectedDate = new Date();

    let currentMonth = selectedDate.getMonth();
    let currentYear = selectedDate.getFullYear();
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

    // Get today's date in YYYY-MM-DD format
    function getTodayDateString(): string {
        const today = new Date();
        const year = today.getFullYear();
        const month = String(today.getMonth() + 1).padStart(2, "0");
        const day = String(today.getDate()).padStart(2, "0");
        return `${year}-${month}-${day}`;
    }

    // Format date to YYYY-MM-DD
    function formatDateString(date: Date): string {
        const year = date.getFullYear();
        const month = String(date.getMonth() + 1).padStart(2, "0");
        const day = String(date.getDate()).padStart(2, "0");
        return `${year}-${month}-${day}`;
    }

    // Check if date is today
    function isToday(date: Date): boolean {
        const today = new Date();
        return formatDateString(date) === formatDateString(today);
    }

    // Check if date is selected
    function isSelectedDate(date: Date, selected: Date): boolean {
        return formatDateString(date) === formatDateString(selected);
    }

    // Check if date is in the past
    function isPastDate(
        day: number | Date,
        month?: number,
        year?: number,
    ): boolean {
        const today = new Date();
        today.setHours(0, 0, 0, 0);

        if (day instanceof Date) {
            const checkDate = new Date(day);
            checkDate.setHours(0, 0, 0, 0);
            return checkDate < today;
        }

        if (
            typeof day === "number" &&
            month !== undefined &&
            year !== undefined
        ) {
            const checkDate = new Date(year, month, day);
            return checkDate < today;
        }

        return false;
    }

    // Calendar logic
    $: daysInCurrentMonth = new Date(
        currentYear,
        currentMonth + 1,
        0,
    ).getDate();
    $: firstDayOfWeek = new Date(currentYear, currentMonth, 1).getDay();

    $: calendarDays = (() => {
        const days = [];
        for (let i = 0; i < firstDayOfWeek; i++) {
            days.push(null);
        }
        for (let i = 1; i <= daysInCurrentMonth; i++) {
            days.push(i);
        }
        return days;
    })();

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

        let newMonth = currentMonth + step;
        let newYear = currentYear;

        if (newMonth > 11) {
            newMonth = 0;
            newYear++;
        } else if (newMonth < 0) {
            newMonth = 11;
            newYear--;
        }

        currentMonth = newMonth;
        currentYear = newYear;

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
        const today = new Date();
        return formatDateString(checkDate) === formatDateString(today);
    }

    // Fetch appointments for today/current month
    async function fetchCurrentMonthAppointments() {
        const today = new Date();
        await fetchAppointmentsForMonth(today);
    }

    // Fetch appointments for the month
    async function fetchAppointmentsForMonth(date: Date) {
        loading = true;
        try {
            const year = date.getFullYear();
            const month = date.getMonth();

            const today = new Date();
            today.setHours(0, 0, 0, 0);

            let startDate = new Date(year, month, 1);

            // If current month, start from today
            if (year === today.getFullYear() && month === today.getMonth()) {
                startDate = today;
            }

            const endDate = new Date(year, month + 1, 0);

            const startStr = formatDateString(startDate);
            const endStr = formatDateString(endDate);

            const diffTime = Math.abs(endDate.getTime() - startDate.getTime());
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)) + 1;

            const response = await fetch(
                `/api/v1/agendamentos/prestador/${providerId}/periodo?data_inicio=${startStr}&data_fim=${endStr}&limit=${diffDays}`,
            );

            if (response.ok) {
                const json = await response.json();
                const rawData = Array.isArray(json.data)
                    ? json.data
                    : Array.isArray(json)
                      ? json
                      : [];

                appointments = rawData.map((item: any) => {
                    let data = item.data;
                    let hora_inicio = item.hora_inicio;
                    let hora_fim = item.hora_fim;
                    let status = item.status;

                    // Normalize Dates & Times from ISO
                    if (!data && item.data_inicio) {
                        data = item.data_inicio.split("T")[0];
                    }
                    if (!hora_inicio && item.data_inicio) {
                        hora_inicio = item.data_inicio
                            .split("T")[1]
                            .substring(0, 8); // hh:mm:ss
                    }
                    if (!hora_fim && item.data_fim) {
                        hora_fim = item.data_fim.split("T")[1].substring(0, 8);
                    }

                    // Normalize Status (Number -> String)
                    if (typeof status === "number") {
                        const statusMap: Record<number, string> = {
                            1: "pendente",
                            2: "confirmado",
                            3: "concluido",
                            4: "cancelado",
                        };
                        status = statusMap[status] || "pendente";
                    }

                    // Extract price from servico if not at root
                    let preco = item.preco;
                    if (
                        (preco === undefined || preco === null) &&
                        item.servico &&
                        item.servico.preco
                    ) {
                        preco = item.servico.preco;
                    }

                    return {
                        ...item,
                        data,
                        hora_inicio,
                        hora_fim,
                        status,
                        preco,
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

    // Calculate statistics
    function calculateStats() {
        const today = getTodayDateString();
        const todayApts = appointments.filter((apt) => apt.data === today);

        stats.hoje = todayApts.length;
        stats.pendentes = todayApts.filter(
            (apt) => apt.status === "pendente",
        ).length;
        stats.confirmados = todayApts.filter(
            (apt) => apt.status === "confirmado",
        ).length;
        stats.receitaHoje =
            todayApts
                .filter((apt) => apt.status !== "cancelado")
                .reduce((sum, apt) => sum + (apt.preco || 0), 0) / 100; // Convert from cents
    }

    // Format time from HH:MM:SS to HH:MM
    function formatTime(time: string): string {
        if (!time) return "";
        return time.substring(0, 5);
    }

    // Get status label
    function getStatusLabel(status: string): string {
        const labels: Record<string, string> = {
            pendente: "Aguardando Confirmação",
            confirmado: "Confirmado",
            cancelado: "Cancelado",
            concluido: "Concluído",
        };
        return labels[status] || status;
    }

    // Get status color classes
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

    // Confirm appointment
    async function confirmAppointment(appointmentId: string) {
        try {
            const response = await fetch(
                `/api/v1/agendamentos/${appointmentId}/confirmar`,
                {
                    method: "PUT",
                },
            );

            if (response.ok) {
                await fetchCurrentMonthAppointments(); // Refresh data
            } else {
                console.error("Failed to confirm appointment");
            }
        } catch (error) {
            console.error("Error confirming appointment:", error);
        }
    }

    // Cancel appointment
    async function cancelAppointment(appointmentId: string) {
        try {
            const response = await fetch(
                `/api/v1/agendamentos/${appointmentId}/cancelar`,
                {
                    method: "PUT",
                },
            );

            if (response.ok) {
                await fetchCurrentMonthAppointments(); // Refresh data
            } else {
                console.error("Failed to cancel appointment");
            }
        } catch (error) {
            console.error("Error canceling appointment:", error);
        }
    }

    onMount(() => {
        fetchCurrentMonthAppointments();
    });
</script>

<div
    class="font-body bg-[hsl(var(--bs-background))] text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200"
>
    <Sidebar />
    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <DashboardNavbar />
        <div class="flex-1 overflow-y-auto p-6 md:p-8">
            <div class="max-w-6xl mx-auto space-y-6">
                <div
                    class="flex flex-col md:flex-row md:items-center justify-between gap-4"
                >
                    <div>
                        <nav
                            class="flex text-sm text-gray-500 dark:text-gray-400 mb-2"
                        >
                            <a
                                class="hover:text-primary transition-colors"
                                href="/agenda">Painel</a
                            >
                            <span class="mx-2">/</span>
                            <span
                                class="text-gray-900 dark:text-white font-medium"
                                >Agenda</span
                            >
                        </nav>
                        <h1
                            class="text-2xl font-bold text-gray-900 dark:text-white"
                        >
                            Agenda de Atendimentos
                        </h1>
                        <p class="text-gray-500 dark:text-gray-400 mt-1">
                            Gerencie seus próximos atendimentos e solicitações.
                        </p>
                    </div>
                    <div class="flex space-x-3">
                        <button
                            class="flex items-center px-4 py-2 bg-white dark:bg-[hsl(var(--bs-card))] border border-border-light dark:border-border-dark rounded-md text-sm font-medium text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"
                        >
                            <span
                                class="material-symbols-outlined text-[20px] mr-2"
                                >filter_list</span
                            >
                            Filtrar
                        </button>
                        <button
                            class="flex items-center px-4 py-2 bg-primary hover:bg-primary-hover text-white rounded-md text-sm font-medium shadow-md transition-colors"
                        >
                            <span
                                class="material-symbols-outlined text-[20px] mr-2"
                                >add</span
                            >
                            Novo Agendamento
                        </button>
                    </div>
                </div>
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
                            <p class="text-sm text-gray-500 dark:text-gray-400">
                                Hoje
                            </p>
                            <p
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                {stats.hoje}
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
                            <p class="text-sm text-gray-500 dark:text-gray-400">
                                Confirmados
                            </p>
                            <p
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                {stats.confirmados}
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
                            <p class="text-sm text-gray-500 dark:text-gray-400">
                                Receita Est. (Hoje)
                            </p>
                            <p
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                R$ {stats.receitaHoje
                                    .toFixed(2)
                                    .replace(".", ",")}
                            </p>
                        </div>
                    </div>
                </div>
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Left Column: Calendar -->
                    <div class="space-y-6">
                        <div
                            class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-4"
                        >
                            <div class="flex items-center justify-between mb-4">
                                <h2
                                    class="text-lg font-semibold text-gray-900 dark:text-white capitalize"
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

                            <!-- Weekday Headers -->
                            <div
                                class="grid grid-cols-7 gap-1 text-center mb-2"
                            >
                                {#each dayNames as dayName}
                                    <div
                                        class="text-xs font-semibold text-gray-400 uppercase tracking-wider"
                                    >
                                        {dayName}
                                    </div>
                                {/each}
                            </div>

                            <!-- Days Grid -->
                            <div class="grid grid-cols-7 gap-1">
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
                                            class="aspect-square flex flex-col items-center justify-center rounded-full text-sm font-medium transition-all relative
                                            {isPast
                                                ? 'opacity-30 cursor-not-allowed hover:bg-transparent text-gray-400'
                                                : ''}
                                            {!isPast && isSelected
                                                ? 'bg-primary text-white shadow-md'
                                                : !isPast && isToday
                                                  ? 'bg-blue-100 text-blue-600 dark:bg-blue-900/30 dark:text-blue-400'
                                                  : !isPast
                                                    ? 'text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800'
                                                    : ''}"
                                        >
                                            {day}
                                            {#if isToday && !isSelected && !isPast}
                                                <div
                                                    class="absolute bottom-1 w-1 h-1 bg-current rounded-full opacity-50"
                                                ></div>
                                            {/if}
                                        </button>
                                    {:else}
                                        <div class="aspect-square"></div>
                                    {/if}
                                {/each}
                            </div>
                        </div>

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
                                            Beatriz M. agendou para amanhã às
                                            14h
                                        </p>
                                        <p class="text-xs text-gray-400 mt-1">
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
                                            5 estrelas em "Corte e Escova"
                                        </p>
                                        <p class="text-xs text-gray-400 mt-1">
                                            Há 2 horas
                                        </p>
                                    </div>
                                </div>
                            </div>
                            <button
                                class="w-full mt-4 text-center text-sm text-primary hover:text-primary-hover font-medium transition-colors"
                            >
                                Ver todas as notificações
                            </button>
                        </div>

                        <div
                            class="bg-blue-50 dark:bg-blue-900/10 border border-blue-100 dark:border-blue-900/30 rounded-lg p-5"
                        >
                            <div class="flex items-center mb-2">
                                <span
                                    class="material-symbols-outlined text-blue-500 mr-2"
                                    >info</span
                                >
                                <h4
                                    class="text-sm font-semibold text-blue-800 dark:text-blue-300"
                                >
                                    Lembrete do Sistema
                                </h4>
                            </div>
                            <p
                                class="text-sm text-blue-600 dark:text-blue-400 mb-3"
                            >
                                Não se esqueça de confirmar os agendamentos
                                pendentes até às 18:00 para evitar cancelamentos
                                automáticos.
                            </p>
                        </div>
                    </div>

                    <!-- Right Column: Appointments List -->
                    <div class="lg:col-span-2 space-y-4">
                        <h3
                            class="text-lg font-semibold text-gray-900 dark:text-white flex items-center"
                        >
                            <span
                                class="material-symbols-outlined text-primary mr-2"
                                >schedule</span
                            >
                            Atendimentos de {isToday(selectedDate)
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
                                    Nenhum atendimento agendado para hoje
                                </p>
                            </div>
                        {:else}
                            {#each todayAppointments as apt}
                                {@const statusClasses = getStatusClasses(
                                    apt.status,
                                )}
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border-l-4 {statusClasses.border} border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md {apt.status ===
                                    'concluido'
                                        ? 'opacity-75'
                                        : ''}"
                                >
                                    <div
                                        class="flex flex-col sm:flex-row justify-between items-start sm:items-center"
                                    >
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
                                                            apt.servico_nome ||
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
                                                    Cliente: <span
                                                        class="font-medium text-gray-800 dark:text-gray-200"
                                                        >{apt.cliente_nome ||
                                                            "Cliente"}</span
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
                                                            (apt.preco || 0) /
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
                                                        >{apt.duracao || 60} min</span
                                                    >
                                                </div>
                                            </div>
                                        </div>
                                        <div
                                            class="flex space-x-2 w-full sm:w-auto"
                                        >
                                            {#if apt.status === "pendente"}
                                                <button
                                                    on:click={() =>
                                                        cancelAppointment(
                                                            apt.id,
                                                        )}
                                                    class="flex-1 sm:flex-none px-3 py-1.5 border border-gray-300 dark:border-gray-600 text-gray-600 dark:text-gray-300 rounded text-sm hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
                                                    >Cancelar</button
                                                >
                                                <button
                                                    on:click={() =>
                                                        confirmAppointment(
                                                            apt.id,
                                                        )}
                                                    class="flex-1 sm:flex-none px-3 py-1.5 bg-primary hover:bg-primary-hover text-white rounded text-sm shadow-sm transition-colors"
                                                    >Confirmar</button
                                                >
                                            {:else if apt.status === "confirmado" || apt.status === "concluido"}
                                                <button
                                                    class="flex-1 sm:flex-none px-3 py-1.5 text-primary hover:text-primary-hover text-sm font-medium transition-colors"
                                                    >Detalhes</button
                                                >
                                            {/if}
                                        </div>
                                    </div>
                                </div>
                            {/each}
                        {/if}
                    </div>
                </div>
            </div>
            <footer
                class="mt-12 text-center text-sm text-gray-500 dark:text-gray-400 pb-6"
            >
                <p>© 2023 BellaVita Salon. Todos os direitos reservados.</p>
                <div class="mt-2 space-x-4">
                    <a class="hover:text-primary transition-colors" href="/"
                        >Suporte</a
                    >
                    <a class="hover:text-primary transition-colors" href="/"
                        >Termos</a
                    >
                </div>
            </footer>
        </div>
    </main>
</div>

<style>
    /* Custom scrollbar to match original style */
    ::-webkit-scrollbar {
        width: 8px;
        height: 8px;
    }
    ::-webkit-scrollbar-track {
        background: transparent;
    }
    ::-webkit-scrollbar-thumb {
        background: #d1d5db;
        border-radius: 4px;
    }
    :global(.dark) ::-webkit-scrollbar-thumb {
        background: #4b5563;
    }
</style>
