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
    }

    interface Stats {
        hoje: number;
        pendentes: number;
        confirmados: number;
        receitaHoje: number;
    }

    // State
    let providerId = "d583ek114a3g8q7re3rg"; // Fixed ID
    let appointments: Appointment[] = [];
    let todayAppointments: Appointment[] = [];
    let stats: Stats = {
        hoje: 0,
        pendentes: 0,
        confirmados: 0,
        receitaHoje: 0
    };
    let loading = true;

    // Calendar state
    let selectedDate = new Date();
    let currentWeekStart = getWeekStart(new Date());
    let currentMonth = selectedDate.getMonth();
    let currentYear = selectedDate.getFullYear();
    const monthNames = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
                        "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
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
        return date.getDate() === today.getDate() &&
               date.getMonth() === today.getMonth() &&
               date.getFullYear() === today.getFullYear();
    }

    // Check if date is selected
    function isSelectedDate(date: Date): boolean {
        return date.getDate() === selectedDate.getDate() &&
               date.getMonth() === selectedDate.getMonth() &&
               date.getFullYear() === selectedDate.getFullYear();
    }

    // Check if date is in the past
    function isPastDate(date: Date): boolean {
        const today = new Date();
        today.setHours(0, 0, 0, 0);
        const checkDate = new Date(date);
        checkDate.setHours(0, 0, 0, 0);
        return checkDate < today;
    }

    // Get the start of the week (Sunday)
    function getWeekStart(date: Date): Date {
        const d = new Date(date);
        const day = d.getDay();
        const diff = d.getDate() - day;
        return new Date(d.setDate(diff));
    }

    // Get current week days (7 days starting from Sunday) - Reactive
    $: currentWeekDays = (() => {
        const days: Date[] = [];
        const weekStart = new Date(currentWeekStart);
        
        for (let i = 0; i < 7; i++) {
            const day = new Date(weekStart);
            day.setDate(weekStart.getDate() + i);
            days.push(day);
        }
        
        return days;
    })();

    // Navigate to previous day
    function previousDay() {
        const newStart = new Date(currentWeekStart);
        newStart.setDate(newStart.getDate() - 1);
        currentWeekStart = newStart;
        currentMonth = newStart.getMonth();
        currentYear = newStart.getFullYear();
    }

    // Navigate to next day
    function nextDay() {
        const newStart = new Date(currentWeekStart);
        newStart.setDate(newStart.getDate() + 1);
        currentWeekStart = newStart;
        currentMonth = newStart.getMonth();
        currentYear = newStart.getFullYear();
    }

    // Select a date and fetch appointments
    function selectDate(date: Date) {
        if (isPastDate(date)) return;
        selectedDate = new Date(date);
        fetchAppointmentsForDate(date);
    }

    // Fetch appointments for today
    async function fetchTodayAppointments() {
        const today = new Date();
        await fetchAppointmentsForDate(today);
    }

    // Fetch appointments for a specific date
    async function fetchAppointmentsForDate(date: Date) {
        loading = true;
        try {
            const dateStr = formatDateString(date);
            const response = await fetch(
                `/api/v1/agendamentos/prestador/${providerId}?data=${dateStr}`
            );

            if (response.ok) {
                const data = await response.json();
                appointments = Array.isArray(data) ? data : [];
                
                // Filter for selected date's appointments
                todayAppointments = appointments.filter(apt => apt.data === dateStr);
                
                // Calculate stats (only for today)
                calculateStats();
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

    // Calculate statistics
    function calculateStats() {
        const today = getTodayDateString();
        const todayApts = appointments.filter(apt => apt.data === today);
        
        stats.hoje = todayApts.length;
        stats.pendentes = todayApts.filter(apt => apt.status === "pendente").length;
        stats.confirmados = todayApts.filter(apt => apt.status === "confirmado").length;
        stats.receitaHoje = todayApts
            .filter(apt => apt.status !== "cancelado")
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
            concluido: "Concluído"
        };
        return labels[status] || status;
    }

    // Get status color classes
    function getStatusClasses(status: string) {
        const classes: Record<string, { border: string; bg: string; text: string; badge: string }> = {
            pendente: {
                border: "border-l-warning",
                bg: "bg-warning/10",
                text: "text-warning",
                badge: "bg-warning/20 text-warning"
            },
            confirmado: {
                border: "border-l-success",
                bg: "bg-success/10",
                text: "text-success",
                badge: "bg-success/20 text-success"
            },
            cancelado: {
                border: "border-l-gray-400",
                bg: "bg-gray-200 dark:bg-gray-700",
                text: "text-gray-500",
                badge: "bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300"
            },
            concluido: {
                border: "border-l-gray-400",
                bg: "bg-gray-200 dark:bg-gray-700",
                text: "text-gray-500",
                badge: "bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300"
            }
        };
        return classes[status] || classes.pendente;
    }

    // Confirm appointment
    async function confirmAppointment(appointmentId: string) {
        try {
            const response = await fetch(`/api/v1/agendamentos/${appointmentId}/confirmar`, {
                method: "PUT"
            });

            if (response.ok) {
                await fetchTodayAppointments(); // Refresh data
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
            const response = await fetch(`/api/v1/agendamentos/${appointmentId}/cancelar`, {
                method: "PUT"
            });

            if (response.ok) {
                await fetchTodayAppointments(); // Refresh data
            } else {
                console.error("Failed to cancel appointment");
            }
        } catch (error) {
            console.error("Error canceling appointment:", error);
        }
    }

    onMount(() => {
        fetchTodayAppointments();
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
                                R$ {stats.receitaHoje.toFixed(2).replace(".", ",")}
                            </p>
                        </div>
                    </div>
                </div>
                <div
                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-4"
                >
                    <div class="flex items-center justify-between mb-4">
                        <h2
                            class="text-lg font-semibold text-gray-900 dark:text-white"
                        >
                            {monthNames[currentMonth]} {currentYear}
                        </h2>
                        <div class="flex space-x-2">
                            <button
                                on:click={previousDay}
                                class="p-1 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-300"
                            >
                                <span class="material-symbols-outlined"
                                    >chevron_left</span
                                >
                            </button>
                            <button
                                on:click={nextDay}
                                class="p-1 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-300"
                            >
                                <span class="material-symbols-outlined"
                                    >chevron_right</span
                                >
                            </button>
                        </div>
                    </div>
                    <div class="grid grid-cols-7 gap-2 text-center">
                        {#each currentWeekDays as day, index}
                            {@const isPast = isPastDate(day)}
                            {@const isTodayDate = isToday(day)}
                            {@const isSelected = isSelectedDate(day)}
                            <button
                                on:click={() => selectDate(day)}
                                disabled={isPast}
                                class="p-2 rounded-lg transition-all {isSelected
                                    ? 'bg-primary text-white shadow-md'
                                    : isTodayDate
                                    ? 'bg-blue-100 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400'
                                    : !isPast
                                    ? 'hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer'
                                    : 'text-gray-400 cursor-not-allowed'}"
                            >
                                <p class="text-xs {isSelected ? 'text-white/80' : 'text-gray-500'} uppercase">{dayNames[index]}</p>
                                <p
                                    class="text-sm font-medium mt-1 {isSelected ? 'text-white font-bold' : isTodayDate ? 'text-blue-600 dark:text-blue-400 font-bold' : !isPast ? 'text-gray-900 dark:text-white' : 'text-gray-400'}"
                                >
                                    {day.getDate()}
                                </p>
                            </button>
                        {/each}
                    </div>
                </div>
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="lg:col-span-2 space-y-4">
                        <h3
                            class="text-lg font-semibold text-gray-900 dark:text-white flex items-center"
                        >
                            <span
                                class="material-symbols-outlined text-primary mr-2"
                                >schedule</span
                            >
                            Atendimentos de {isToday(selectedDate) ? "Hoje" : selectedDate.toLocaleDateString('pt-BR', { day: '2-digit', month: 'long' })}
                        </h3>
                        
                        {#if loading}
                            <div class="flex justify-center py-12">
                                <span class="material-symbols-outlined animate-spin text-brand-orange text-3xl">sync</span>
                            </div>
                        {:else if todayAppointments.length === 0}
                            <div class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border border-border-light dark:border-border-dark p-8 text-center">
                                <span class="material-symbols-outlined text-gray-400 text-5xl mb-2">event_busy</span>
                                <p class="text-gray-500 dark:text-gray-400">Nenhum atendimento agendado para hoje</p>
                            </div>
                        {:else}
                            {#each todayAppointments as apt}
                                {@const statusClasses = getStatusClasses(apt.status)}
                                <div
                                    class="bg-[hsl(var(--bs-card))] rounded-lg shadow-sm border-l-4 {statusClasses.border} border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md {apt.status === 'concluido' ? 'opacity-75' : ''}"
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
                                                <span class="material-symbols-outlined">
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
                                                <div class="flex items-center">
                                                    <span
                                                        class="text-lg font-bold {apt.status === 'concluido' ? 'text-gray-500 dark:text-gray-400' : 'text-gray-900 dark:text-white'}"
                                                        >{formatTime(apt.hora_inicio)}</span
                                                    >
                                                    <span class="mx-2 text-gray-400"
                                                        >•</span
                                                    >
                                                    <span
                                                        class="text-sm {apt.status === 'concluido' ? 'text-gray-500 dark:text-gray-400 line-through' : 'text-gray-600 dark:text-gray-300'} font-medium"
                                                        >{apt.servico_nome || "Serviço"}</span
                                                    >
                                                    {#if apt.status === "confirmado"}
                                                        <span
                                                            class="ml-2 text-xs {statusClasses.badge} px-2 py-0.5 rounded-full"
                                                            >{getStatusLabel(apt.status)}</span
                                                        >
                                                    {/if}
                                                </div>
                                                <p
                                                    class="text-sm text-gray-500 dark:text-gray-400 mt-1"
                                                >
                                                    Cliente: <span
                                                        class="font-medium {apt.status === 'concluido' ? '' : 'text-gray-800 dark:text-gray-200'}"
                                                        >{apt.cliente_nome || "Cliente"}</span
                                                    >
                                                </p>
                                                <div
                                                    class="flex items-center mt-2 text-xs text-gray-400"
                                                >
                                                    <span
                                                        class="material-symbols-outlined text-[16px] mr-1"
                                                        >timer</span
                                                    >
                                                    <span>{apt.duracao || 60} min</span>
                                                    {#if apt.status === "pendente"}
                                                        <span class="mx-2">•</span>
                                                        <span
                                                            class="{statusClasses.text} font-medium"
                                                            >{getStatusLabel(apt.status)}</span
                                                        >
                                                    {/if}
                                                </div>
                                            </div>
                                        </div>
                                        <div class="flex space-x-2 w-full sm:w-auto">
                                            {#if apt.status === "pendente"}
                                                <button
                                                    on:click={() => cancelAppointment(apt.id)}
                                                    class="flex-1 sm:flex-none px-3 py-1.5 border border-gray-300 dark:border-gray-600 text-gray-600 dark:text-gray-300 rounded text-sm hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
                                                    >Cancelar</button
                                                >
                                                <button
                                                    on:click={() => confirmAppointment(apt.id)}
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
