<script lang="ts">
  import Sidebar from '$lib/components/Sidebar.svelte';
  import ThemeToggle from '$lib/components/ThemeToggle.svelte';
</script>

<div class="font-body bg-background-light dark:bg-background-dark text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200">
<Sidebar />
<main class="flex-1 flex flex-col h-full overflow-hidden relative">
<header class="h-16 bg-surface-light dark:bg-surface-dark border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10">
<div class="flex items-center flex-1 max-w-2xl">
<div class="relative w-full md:w-64">
<span class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
<span class="material-symbols-outlined text-gray-400">search</span>
</span>
<input class="block w-full pl-10 pr-3 py-2 border border-border-light dark:border-border-dark rounded-md leading-5 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-1 focus:ring-input-focus focus:border-input-focus sm:text-sm transition-all" placeholder="Buscar na agenda..." type="text"/>
</div>
</div>
            <div class="flex items-center space-x-4 ml-4">
                <button
                    class="p-2 rounded-full text-gray-400 hover:text-gray-500 dark:hover:text-gray-300 focus:outline-none transition-colors relative"
                >
                    <span class="material-symbols-outlined">notifications</span>
                    <span
                        class="absolute top-1.5 right-1.5 block h-2 w-2 rounded-full bg-primary ring-2 ring-white dark:ring-gray-800"
                    ></span>
                </button>
                <ThemeToggle />
            </div>
</header>
<div class="flex-1 overflow-y-auto p-6 md:p-8">
<div class="max-w-6xl mx-auto space-y-6">
<div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
<div>
<nav class="flex text-sm text-gray-500 dark:text-gray-400 mb-2">
<a class="hover:text-primary transition-colors" href="/agenda">Painel</a>
<span class="mx-2">/</span>
<span class="text-gray-900 dark:text-white font-medium">Agenda</span>
</nav>
<h1 class="text-2xl font-bold text-gray-900 dark:text-white">Agenda de Atendimentos</h1>
<p class="text-gray-500 dark:text-gray-400 mt-1">Gerencie seus próximos atendimentos e solicitações.</p>
</div>
<div class="flex space-x-3">
<button class="flex items-center px-4 py-2 bg-white dark:bg-surface-dark border border-border-light dark:border-border-dark rounded-md text-sm font-medium text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors">
<span class="material-symbols-outlined text-[20px] mr-2">filter_list</span>
                            Filtrar
                        </button>
<button class="flex items-center px-4 py-2 bg-primary hover:bg-primary-hover text-white rounded-md text-sm font-medium shadow-md transition-colors">
<span class="material-symbols-outlined text-[20px] mr-2">add</span>
                            Novo Agendamento
                        </button>
</div>
</div>
<div class="grid grid-cols-1 md:grid-cols-4 gap-4">
<div class="bg-surface-light dark:bg-surface-dark p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center">
<div class="p-3 rounded-full bg-blue-100 dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 mr-4">
<span class="material-symbols-outlined">calendar_today</span>
</div>
<div>
<p class="text-sm text-gray-500 dark:text-gray-400">Hoje</p>
<p class="text-2xl font-bold text-gray-900 dark:text-white">8</p>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center">
<div class="p-3 rounded-full bg-yellow-100 dark:bg-yellow-900/30 text-yellow-600 dark:text-yellow-400 mr-4">
<span class="material-symbols-outlined">pending</span>
</div>
<div>
<p class="text-sm text-gray-500 dark:text-gray-400">Pendentes</p>
<p class="text-2xl font-bold text-gray-900 dark:text-white">3</p>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center">
<div class="p-3 rounded-full bg-green-100 dark:bg-green-900/30 text-green-600 dark:text-green-400 mr-4">
<span class="material-symbols-outlined">check_circle</span>
</div>
<div>
<p class="text-sm text-gray-500 dark:text-gray-400">Confirmados</p>
<p class="text-2xl font-bold text-gray-900 dark:text-white">12</p>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark p-4 rounded-lg shadow-sm border border-border-light dark:border-border-dark flex items-center">
<div class="p-3 rounded-full bg-purple-100 dark:bg-purple-900/30 text-purple-600 dark:text-purple-400 mr-4">
<span class="material-symbols-outlined">attach_money</span>
</div>
<div>
<p class="text-sm text-gray-500 dark:text-gray-400">Receita Est. (Hoje)</p>
<p class="text-2xl font-bold text-gray-900 dark:text-white">R$ 450</p>
</div>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border border-border-light dark:border-border-dark p-4">
<div class="flex items-center justify-between mb-4">
<h2 class="text-lg font-semibold text-gray-900 dark:text-white">Outubro 2023</h2>
<div class="flex space-x-2">
<button class="p-1 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-300">
<span class="material-symbols-outlined">chevron_left</span>
</button>
<button class="p-1 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-600 dark:text-gray-300">
<span class="material-symbols-outlined">chevron_right</span>
</button>
</div>
</div>
<div class="grid grid-cols-7 gap-2 text-center">
<div class="p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer">
<p class="text-xs text-gray-500 uppercase">Seg</p>
<p class="text-sm font-medium mt-1 text-gray-900 dark:text-white">23</p>
</div>
<div class="p-2 rounded-lg bg-primary text-white shadow-md cursor-pointer">
<p class="text-xs text-white/80 uppercase">Ter</p>
<p class="text-sm font-bold mt-1">24</p>
</div>
<div class="p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer">
<p class="text-xs text-gray-500 uppercase">Qua</p>
<p class="text-sm font-medium mt-1 text-gray-900 dark:text-white">25</p>
</div>
<div class="p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer">
<p class="text-xs text-gray-500 uppercase">Qui</p>
<p class="text-sm font-medium mt-1 text-gray-900 dark:text-white">26</p>
</div>
<div class="p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer">
<p class="text-xs text-gray-500 uppercase">Sex</p>
<p class="text-sm font-medium mt-1 text-gray-900 dark:text-white">27</p>
</div>
<div class="p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer text-gray-400">
<p class="text-xs uppercase">Sab</p>
<p class="text-sm font-medium mt-1">28</p>
</div>
<div class="p-2 rounded-lg hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer text-gray-400">
<p class="text-xs uppercase">Dom</p>
<p class="text-sm font-medium mt-1">29</p>
</div>
</div>
</div>
<div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
<div class="lg:col-span-2 space-y-4">
<h3 class="text-lg font-semibold text-gray-900 dark:text-white flex items-center">
<span class="material-symbols-outlined text-primary mr-2">schedule</span>
                            Atendimentos de Hoje
                        </h3>
<div class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border-l-4 border-l-warning border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md">
<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center">
<div class="flex items-start space-x-4 mb-4 sm:mb-0">
<div class="bg-warning/10 text-warning p-2 rounded-full">
<span class="material-symbols-outlined">hourglass_top</span>
</div>
<div>
<div class="flex items-center">
<span class="text-lg font-bold text-gray-900 dark:text-white">09:00</span>
<span class="mx-2 text-gray-400">•</span>
<span class="text-sm text-gray-600 dark:text-gray-300 font-medium">Corte de Cabelo + Hidratação</span>
</div>
<p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Cliente: <span class="font-medium text-gray-800 dark:text-gray-200">Maria Oliveira</span></p>
<div class="flex items-center mt-2 text-xs text-gray-400">
<span class="material-symbols-outlined text-[16px] mr-1">timer</span>
<span>60 min</span>
<span class="mx-2">•</span>
<span class="text-warning font-medium">Aguardando Confirmação</span>
</div>
</div>
</div>
<div class="flex space-x-2 w-full sm:w-auto">
<button class="flex-1 sm:flex-none px-3 py-1.5 border border-gray-300 dark:border-gray-600 text-gray-600 dark:text-gray-300 rounded text-sm hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">Cancelar</button>
<button class="flex-1 sm:flex-none px-3 py-1.5 bg-primary hover:bg-primary-hover text-white rounded text-sm shadow-sm transition-colors">Confirmar</button>
</div>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border-l-4 border-l-warning border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md">
<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center">
<div class="flex items-start space-x-4 mb-4 sm:mb-0">
<div class="bg-warning/10 text-warning p-2 rounded-full">
<span class="material-symbols-outlined">hourglass_top</span>
</div>
<div>
<div class="flex items-center">
<span class="text-lg font-bold text-gray-900 dark:text-white">11:30</span>
<span class="mx-2 text-gray-400">•</span>
<span class="text-sm text-gray-600 dark:text-gray-300 font-medium">Manicure Completa</span>
</div>
<p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Cliente: <span class="font-medium text-gray-800 dark:text-gray-200">Juliana Santos</span></p>
<div class="flex items-center mt-2 text-xs text-gray-400">
<span class="material-symbols-outlined text-[16px] mr-1">timer</span>
<span>45 min</span>
<span class="mx-2">•</span>
<span class="text-warning font-medium">Aguardando Confirmação</span>
</div>
</div>
</div>
<div class="flex space-x-2 w-full sm:w-auto">
<button class="flex-1 sm:flex-none px-3 py-1.5 border border-gray-300 dark:border-gray-600 text-gray-600 dark:text-gray-300 rounded text-sm hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">Cancelar</button>
<button class="flex-1 sm:flex-none px-3 py-1.5 bg-primary hover:bg-primary-hover text-white rounded text-sm shadow-sm transition-colors">Confirmar</button>
</div>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border-l-4 border-l-success border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md opacity-75 hover:opacity-100">
<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center">
<div class="flex items-start space-x-4 mb-4 sm:mb-0">
<div class="bg-success/10 text-success p-2 rounded-full">
<span class="material-symbols-outlined">check</span>
</div>
<div>
<div class="flex items-center">
<span class="text-lg font-bold text-gray-900 dark:text-white">10:30</span>
<span class="mx-2 text-gray-400">•</span>
<span class="text-sm text-gray-600 dark:text-gray-300 font-medium line-through decoration-gray-400">Coloração Raiz</span>
<span class="ml-2 text-xs bg-success/20 text-success px-2 py-0.5 rounded-full">Confirmado</span>
</div>
<p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Cliente: <span class="font-medium text-gray-800 dark:text-gray-200">Fernanda Lima</span></p>
<div class="flex items-center mt-2 text-xs text-gray-400">
<span class="material-symbols-outlined text-[16px] mr-1">timer</span>
<span>90 min</span>
</div>
</div>
</div>
<div class="flex space-x-2 w-full sm:w-auto">
<button class="flex-1 sm:flex-none px-3 py-1.5 text-primary hover:text-primary-hover text-sm font-medium transition-colors">Detalhes</button>
</div>
</div>
</div>
<div class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border-l-4 border-l-success border-y border-r border-gray-200 dark:border-gray-700 p-4 transition-all hover:shadow-md">
<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center">
<div class="flex items-start space-x-4 mb-4 sm:mb-0">
<div class="bg-success/10 text-success p-2 rounded-full">
<span class="material-symbols-outlined">check</span>
</div>
<div>
<div class="flex items-center">
<span class="text-lg font-bold text-gray-900 dark:text-white">13:00</span>
<span class="mx-2 text-gray-400">•</span>
<span class="text-sm text-gray-600 dark:text-gray-300 font-medium">Escova Modeladora</span>
<span class="ml-2 text-xs bg-success/20 text-success px-2 py-0.5 rounded-full">Confirmado</span>
</div>
<p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Cliente: <span class="font-medium text-gray-800 dark:text-gray-200">Patricia Souza</span></p>
<div class="flex items-center mt-2 text-xs text-gray-400">
<span class="material-symbols-outlined text-[16px] mr-1">timer</span>
<span>45 min</span>
</div>
</div>
</div>
<div class="flex space-x-2 w-full sm:w-auto">
<button class="flex-1 sm:flex-none px-3 py-1.5 text-primary hover:text-primary-hover text-sm font-medium transition-colors">Detalhes</button>
</div>
</div>
</div>
<div class="bg-gray-50 dark:bg-gray-800/50 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 p-4 opacity-75">
<div class="flex flex-col sm:flex-row justify-between items-start sm:items-center">
<div class="flex items-start space-x-4 mb-4 sm:mb-0">
<div class="bg-gray-200 dark:bg-gray-700 text-gray-500 p-2 rounded-full">
<span class="material-symbols-outlined">done_all</span>
</div>
<div>
<div class="flex items-center">
<span class="text-lg font-bold text-gray-500 dark:text-gray-400">08:00</span>
<span class="mx-2 text-gray-400">•</span>
<span class="text-sm text-gray-500 dark:text-gray-400 font-medium line-through">Design de Sobrancelhas</span>
<span class="ml-2 text-xs bg-gray-200 dark:bg-gray-700 text-gray-600 dark:text-gray-300 px-2 py-0.5 rounded-full">Concluído</span>
</div>
<p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Cliente: <span class="font-medium">Carla Dias</span></p>
</div>
</div>
</div>
</div>
</div>
<div class="space-y-6">
<div class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border border-border-light dark:border-border-dark p-6">
<h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-4">Solicitações Recentes</h3>
<div class="space-y-4">
<div class="flex items-start space-x-3 pb-4 border-b border-gray-100 dark:border-gray-800 last:border-0 last:pb-0">
<div class="h-10 w-10 rounded-full bg-blue-100 dark:bg-blue-900/30 flex items-center justify-center text-blue-600 dark:text-blue-400 shrink-0">
<span class="material-symbols-outlined text-sm">person</span>
</div>
<div>
<p class="text-sm font-medium text-gray-900 dark:text-white">Novo cliente se cadastrou</p>
<p class="text-xs text-gray-500 dark:text-gray-400 mt-0.5">Beatriz M. agendou para amanhã às 14h</p>
<p class="text-xs text-gray-400 mt-1">Há 15 min</p>
</div>
</div>
<div class="flex items-start space-x-3 pb-4 border-b border-gray-100 dark:border-gray-800 last:border-0 last:pb-0">
<div class="h-10 w-10 rounded-full bg-orange-100 dark:bg-orange-900/30 flex items-center justify-center text-orange-600 dark:text-orange-400 shrink-0">
<span class="material-symbols-outlined text-sm">star</span>
</div>
<div>
<p class="text-sm font-medium text-gray-900 dark:text-white">Nova avaliação recebida</p>
<p class="text-xs text-gray-500 dark:text-gray-400 mt-0.5">5 estrelas em "Corte e Escova"</p>
<p class="text-xs text-gray-400 mt-1">Há 2 horas</p>
</div>
</div>
</div>
<button class="w-full mt-4 text-center text-sm text-primary hover:text-primary-hover font-medium transition-colors">
                                Ver todas as notificações
                            </button>
</div>
<div class="bg-blue-50 dark:bg-blue-900/10 border border-blue-100 dark:border-blue-900/30 rounded-lg p-5">
<div class="flex items-center mb-2">
<span class="material-symbols-outlined text-blue-500 mr-2">info</span>
<h4 class="text-sm font-semibold text-blue-800 dark:text-blue-300">Lembrete do Sistema</h4>
</div>
<p class="text-sm text-blue-600 dark:text-blue-400 mb-3">
                                Não se esqueça de confirmar os agendamentos pendentes até às 18:00 para evitar cancelamentos automáticos.
                            </p>
</div>
</div>
</div>
</div>
<footer class="mt-12 text-center text-sm text-gray-500 dark:text-gray-400 pb-6">
<p>© 2023 BellaVita Salon. Todos os direitos reservados.</p>
<div class="mt-2 space-x-4">
<a class="hover:text-primary transition-colors" href="/">Suporte</a>
<a class="hover:text-primary transition-colors" href="/">Termos</a>
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
        background: #D1D5DB;
        border-radius: 4px;
    }
    :global(.dark) ::-webkit-scrollbar-thumb {
        background: #4B5563;
    }
</style>
