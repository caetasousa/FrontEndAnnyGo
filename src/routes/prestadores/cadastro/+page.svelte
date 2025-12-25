<script lang="ts">
    import Sidebar from "$lib/components/Sidebar.svelte";
    import ThemeToggle from "$lib/components/ThemeToggle.svelte";
</script>

<div
    class="font-body bg-background-light dark:bg-background-dark text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200"
>
    <Sidebar />
    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <header
            class="h-16 bg-surface-light dark:bg-surface-dark border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10"
        >
            <div class="flex items-center flex-1 max-w-2xl">
                <div class="relative w-full">
                    <span
                        class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
                    >
                        <span class="material-symbols-outlined text-gray-400"
                            >search</span
                        >
                    </span>
                    <input
                        class="block w-full pl-10 pr-3 py-2 border border-border-light dark:border-border-dark rounded-md leading-5 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-1 focus:ring-input-focus focus:border-input-focus sm:text-sm transition-all"
                        placeholder="Pesquisar prestadores..."
                        type="text"
                    />
                </div>
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
            <div class="max-w-4xl mx-auto space-y-6">
                <div>
                    <nav
                        class="flex text-sm text-gray-500 dark:text-gray-400 mb-2"
                    >
                        <a class="hover:text-primary transition-colors" href="/"
                            >Admin</a
                        >
                        <span class="mx-2">/</span>
                        <span class="text-gray-900 dark:text-white font-medium"
                            >Novo Prestador</span
                        >
                    </nav>
                    <h1
                        class="text-2xl font-bold text-gray-900 dark:text-white"
                    >
                        Registro de Prestadores
                    </h1>
                    <p class="text-gray-500 dark:text-gray-400 mt-1">
                        Adicione um novo profissional e atribua seus serviços.
                    </p>
                </div>
                <div
                    class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border border-border-light dark:border-border-dark overflow-hidden"
                >
                    <div class="p-6 md:p-8 space-y-8">
                        <div>
                            <h3
                                class="text-lg font-semibold text-gray-900 dark:text-white flex items-center mb-6"
                            >
                                <span
                                    class="material-symbols-outlined mr-2 text-primary"
                                    >person</span
                                >
                                Dados Pessoais
                            </h3>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                                <div class="col-span-1 md:col-span-2">
                                    <label
                                        class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                        for="name">Nome Completo</label
                                    >
                                    <input
                                        class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                        id="name"
                                        placeholder="Ex: Ana Maria Costa"
                                        type="text"
                                    />
                                </div>
                                <div>
                                    <label
                                        class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                        for="cpf">CPF</label
                                    >
                                    <input
                                        class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                        id="cpf"
                                        placeholder="000.000.000-00"
                                        type="text"
                                    />
                                </div>
                                <div>
                                    <label
                                        class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                        for="phone">Telefone / WhatsApp</label
                                    >
                                    <input
                                        class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                        id="phone"
                                        placeholder="(00) 00000-0000"
                                        type="tel"
                                    />
                                </div>
                                <div class="col-span-1 md:col-span-2">
                                    <label
                                        class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                        for="email">Endereço de Email</label
                                    >
                                    <div class="relative">
                                        <div
                                            class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
                                        >
                                            <span
                                                class="material-symbols-outlined text-gray-400 text-[20px]"
                                                >mail</span
                                            >
                                        </div>
                                        <input
                                            class="block w-full pl-10 rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                            id="email"
                                            placeholder="prestador@bellavita.com"
                                            type="email"
                                        />
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div
                            class="border-t border-border-light dark:border-border-dark"
                        ></div>
                        <div>
                            <h3
                                class="text-lg font-semibold text-gray-900 dark:text-white flex items-center mb-2"
                            >
                                <span
                                    class="material-symbols-outlined mr-2 text-primary"
                                    >spa</span
                                >
                                Serviços Atribuídos
                            </h3>
                            <p
                                class="text-sm text-gray-500 dark:text-gray-400 mb-6"
                            >
                                Selecione todos os serviços que este
                                profissional está qualificado para realizar.
                            </p>
                            <div
                                class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4"
                            >
                                <label
                                    class="relative flex items-start p-4 rounded-lg border border-border-light dark:border-border-dark bg-gray-50 dark:bg-gray-800/50 hover:bg-white dark:hover:bg-gray-800 cursor-pointer transition-all group"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white"
                                        >
                                            Corte de Cabelo
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            45 min • Cabeleireiro
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <input
                                            class="h-5 w-5 text-input-focus border-gray-300 rounded focus:ring-input-focus dark:bg-gray-700 dark:border-gray-600"
                                            type="checkbox"
                                        />
                                    </div>
                                </label>
                                <label
                                    class="relative flex items-start p-4 rounded-lg border border-border-light dark:border-border-dark bg-gray-50 dark:bg-gray-800/50 hover:bg-white dark:hover:bg-gray-800 cursor-pointer transition-all group"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white"
                                        >
                                            Manicure Completa
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            60 min • Unhas
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <input
                                            checked
                                            class="h-5 w-5 text-input-focus border-gray-300 rounded focus:ring-input-focus dark:bg-gray-700 dark:border-gray-600"
                                            type="checkbox"
                                        />
                                    </div>
                                </label>
                                <label
                                    class="relative flex items-start p-4 rounded-lg border border-border-light dark:border-border-dark bg-gray-50 dark:bg-gray-800/50 hover:bg-white dark:hover:bg-gray-800 cursor-pointer transition-all group"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white"
                                        >
                                            Design de Sobrancelhas
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            30 min • Estética
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <input
                                            class="h-5 w-5 text-input-focus border-gray-300 rounded focus:ring-input-focus dark:bg-gray-700 dark:border-gray-600"
                                            type="checkbox"
                                        />
                                    </div>
                                </label>
                                <label
                                    class="relative flex items-start p-4 rounded-lg border border-border-light dark:border-border-dark bg-gray-50 dark:bg-gray-800/50 hover:bg-white dark:hover:bg-gray-800 cursor-pointer transition-all group"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white"
                                        >
                                            Extensão de Cílios
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            90 min • Estética
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <input
                                            class="h-5 w-5 text-input-focus border-gray-300 rounded focus:ring-input-focus dark:bg-gray-700 dark:border-gray-600"
                                            type="checkbox"
                                        />
                                    </div>
                                </label>
                                <label
                                    class="relative flex items-start p-4 rounded-lg border border-border-light dark:border-border-dark bg-gray-50 dark:bg-gray-800/50 hover:bg-white dark:hover:bg-gray-800 cursor-pointer transition-all group"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white"
                                        >
                                            Maquiagem Social
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            60 min • Maquiagem
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <input
                                            checked
                                            class="h-5 w-5 text-input-focus border-gray-300 rounded focus:ring-input-focus dark:bg-gray-700 dark:border-gray-600"
                                            type="checkbox"
                                        />
                                    </div>
                                </label>
                                <label
                                    class="relative flex items-start p-4 rounded-lg border border-border-light dark:border-border-dark bg-gray-50 dark:bg-gray-800/50 hover:bg-white dark:hover:bg-gray-800 cursor-pointer transition-all group"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white"
                                        >
                                            Hidratação Facial
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            45 min • Estética
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <input
                                            class="h-5 w-5 text-input-focus border-gray-300 rounded focus:ring-input-focus dark:bg-gray-700 dark:border-gray-600"
                                            type="checkbox"
                                        />
                                    </div>
                                </label>
                            </div>
                        </div>
                    </div>
                    <div
                        class="px-6 py-4 bg-gray-50 dark:bg-gray-800/50 border-t border-border-light dark:border-border-dark flex items-center justify-end space-x-3"
                    >
                        <button
                            class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-gray-700 dark:text-gray-300 font-medium hover:bg-white dark:hover:bg-gray-700 transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                        >
                            Cancelar
                        </button>
                        <button
                            class="px-6 py-2 bg-primary hover:bg-primary-hover text-white rounded-md font-medium shadow-md transition-colors flex items-center focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary"
                        >
                            <span
                                class="material-symbols-outlined text-[20px] mr-2"
                                >save</span
                            >
                            Salvar Prestador
                        </button>
                    </div>
                </div>
                <div
                    class="bg-blue-50 dark:bg-blue-900/10 border border-blue-100 dark:border-blue-900/30 rounded-lg p-4 flex items-start"
                >
                    <span
                        class="material-symbols-outlined text-blue-500 mt-0.5 mr-3"
                        >info</span
                    >
                    <div>
                        <h4
                            class="text-sm font-medium text-blue-800 dark:text-blue-300"
                        >
                            Informação Importante
                        </h4>
                        <p
                            class="text-sm text-blue-600 dark:text-blue-400 mt-1"
                        >
                            O novo prestador receberá um email com as
                            credenciais de acesso provisórias. Certifique-se de
                            que o email inserido está correto.
                        </p>
                    </div>
                </div>
            </div>
            <footer
                class="mt-12 text-center text-sm text-gray-500 dark:text-gray-400 pb-6"
            >
                <p>© 2023 BellaVita Salon. Todos os direitos reservados.</p>
                <div class="mt-2 space-x-4">
                    <a class="hover:text-primary transition-colors" href="/"
                        >Termos</a
                    >
                    <a class="hover:text-primary transition-colors" href="/"
                        >Privacidade</a
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
