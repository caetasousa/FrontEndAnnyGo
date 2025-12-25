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
                        placeholder="Pesquisar agendamentos..."
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
                            >Início</a
                        >
                        <span class="mx-2">/</span>
                        <span class="text-gray-900 dark:text-white font-medium"
                            >Meu Perfil</span
                        >
                    </nav>
                    <div class="flex items-center justify-between">
                        <div>
                            <h1
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                Perfil do Prestador
                            </h1>
                            <p class="text-gray-500 dark:text-gray-400 mt-1">
                                Gerencie suas informações pessoais, serviços e
                                disponibilidade.
                            </p>
                        </div>
                        <span
                            class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium bg-green-100 text-green-800 dark:bg-green-900/30 dark:text-green-300"
                        >
                            <span class="w-2 h-2 mr-2 bg-green-500 rounded-full"
                            ></span>
                            Ativo
                        </span>
                    </div>
                </div>
                <div
                    class="bg-surface-light dark:bg-surface-dark rounded-lg shadow-sm border border-border-light dark:border-border-dark overflow-hidden"
                >
                    <div class="relative h-32 bg-primary">
                        <div class="absolute -bottom-10 left-8">
                            <div class="relative">
                                <img
                                    alt="Ana Maria Costa"
                                    class="h-24 w-24 rounded-full border-4 border-white dark:border-surface-dark object-cover bg-white"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuBb0ATcXG4FyKxC1twW6jW1I3PLqBWNywkqLLdNpJ4x01o_EjHRWFY69U_fzxAa2pSXWVWHjFYGBYs34htCvjGS2fz_k_hAX2eGfmgYOb6GfalpvTABqMextdJ2-EFSN-vph8xY2l31Bz-0d4r5jlelA6Dogcle1FRJXYR6PJ9hIT22fwfdjLphhZvByuIIkVULiwZzXITJYwFF1zIyMBG-KPYgE022HBqLcyuU-jIHNafhWecl7NofaMfp_E_HIxlFcgEecoHAQbI"
                                />
                                <button
                                    class="absolute bottom-0 right-0 bg-primary hover:bg-primary-hover text-white rounded-full p-1.5 shadow-md transition-colors"
                                    title="Alterar foto"
                                >
                                    <span
                                        class="material-symbols-outlined text-[16px]"
                                        >photo_camera</span
                                    >
                                </button>
                            </div>
                        </div>
                    </div>
                    <div class="pt-14 px-6 md:px-8 pb-8 space-y-8">
                        <div>
                            <h3
                                class="text-lg font-semibold text-gray-900 dark:text-white flex items-center mb-6"
                            >
                                <span
                                    class="material-symbols-outlined mr-2 text-primary"
                                    >badge</span
                                >
                                Informações Pessoais
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
                                        value="Ana Maria Costa"
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
                                        value="123.456.789-00"
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
                                        value="(11) 98765-4321"
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
                                            value="anamaria@bellavita.com"
                                        />
                                    </div>
                                </div>
                                <div class="col-span-1 md:col-span-2">
                                    <label
                                        class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1"
                                        for="bio">Biografia</label
                                    >
                                    <textarea
                                        class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                        id="bio"
                                        placeholder="Fale um pouco sobre sua experiência..."
                                        rows="3"
                                        >Especialista em cortes modernos e
                                        coloração com mais de 5 anos de
                                        experiência.</textarea
                                    >
                                </div>
                            </div>
                        </div>
                        <div
                            class="border-t border-border-light dark:border-border-dark"
                        ></div>
                        <div>
                            <div class="flex items-center justify-between mb-2">
                                <h3
                                    class="text-lg font-semibold text-gray-900 dark:text-white flex items-center"
                                >
                                    <span
                                        class="material-symbols-outlined mr-2 text-primary"
                                        >content_cut</span
                                    >
                                    Meus Serviços
                                </h3>
                                <button
                                    class="text-sm text-primary hover:text-primary-hover font-medium"
                                >
                                    Gerenciar Serviços
                                </button>
                            </div>
                            <p
                                class="text-sm text-gray-500 dark:text-gray-400 mb-6"
                            >
                                Serviços que você está habilitado a realizar
                                atualmente.
                            </p>
                            <div
                                class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4"
                            >
                                <div
                                    class="relative flex items-start p-4 rounded-lg border border-primary/30 bg-orange-50 dark:bg-orange-900/10 dark:border-primary/20 transition-all"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white flex justify-between"
                                        >
                                            Corte de Cabelo
                                            <span class="text-primary font-bold"
                                                >R$ 80</span
                                            >
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            45 min • Cabeleireiro
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <span
                                            class="material-symbols-outlined text-green-600 text-[18px]"
                                            >check_circle</span
                                        >
                                    </div>
                                </div>
                                <div
                                    class="relative flex items-start p-4 rounded-lg border border-primary/30 bg-orange-50 dark:bg-orange-900/10 dark:border-primary/20 transition-all"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white flex justify-between"
                                        >
                                            Escova
                                            <span class="text-primary font-bold"
                                                >R$ 50</span
                                            >
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            30 min • Cabeleireiro
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <span
                                            class="material-symbols-outlined text-green-600 text-[18px]"
                                            >check_circle</span
                                        >
                                    </div>
                                </div>
                                <div
                                    class="relative flex items-start p-4 rounded-lg border border-primary/30 bg-orange-50 dark:bg-orange-900/10 dark:border-primary/20 transition-all"
                                >
                                    <div class="min-w-0 flex-1 text-sm">
                                        <div
                                            class="font-medium text-gray-900 dark:text-white flex justify-between"
                                        >
                                            Hidratação
                                            <span class="text-primary font-bold"
                                                >R$ 120</span
                                            >
                                        </div>
                                        <p
                                            class="text-gray-500 dark:text-gray-400 text-xs mt-1"
                                        >
                                            60 min • Tratamento
                                        </p>
                                    </div>
                                    <div class="ml-3 flex items-center h-5">
                                        <span
                                            class="material-symbols-outlined text-green-600 text-[18px]"
                                            >check_circle</span
                                        >
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div
                            class="border-t border-border-light dark:border-border-dark"
                        ></div>
                        <div>
                            <h3
                                class="text-lg font-semibold text-gray-900 dark:text-white flex items-center mb-6"
                            >
                                <span
                                    class="material-symbols-outlined mr-2 text-primary"
                                    >schedule</span
                                >
                                Agenda de Trabalho
                            </h3>
                            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                                <div class="space-y-4">
                                    <h4
                                        class="text-sm font-medium text-gray-700 dark:text-gray-300"
                                    >
                                        Horário Semanal
                                    </h4>
                                    <div class="space-y-3">
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Segunda</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >09:00 - 18:00</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-green-500"
                                            ></div>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Terça</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >09:00 - 18:00</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-green-500"
                                            ></div>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Quarta</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >09:00 - 18:00</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-green-500"
                                            ></div>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Quinta</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >09:00 - 19:00</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-green-500"
                                            ></div>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Sexta</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >09:00 - 19:00</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-green-500"
                                            ></div>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md opacity-75"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Sábado</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >09:00 - 14:00</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-green-500"
                                            ></div>
                                        </div>
                                        <div
                                            class="flex items-center justify-between text-sm p-3 bg-gray-50 dark:bg-gray-800/50 rounded-md opacity-50"
                                        >
                                            <span
                                                class="font-medium text-gray-900 dark:text-white w-24"
                                                >Domingo</span
                                            >
                                            <span
                                                class="text-gray-500 dark:text-gray-400"
                                                >Folga</span
                                            >
                                            <div
                                                class="w-2 h-2 rounded-full bg-red-400"
                                            ></div>
                                        </div>
                                    </div>
                                </div>
                                <div class="space-y-4">
                                    <h4
                                        class="text-sm font-medium text-gray-700 dark:text-gray-300"
                                    >
                                        Próximas Ausências
                                    </h4>
                                    <div
                                        class="border border-border-light dark:border-border-dark rounded-lg p-4 bg-gray-50 dark:bg-gray-800/30"
                                    >
                                        <div class="flex items-start">
                                            <span
                                                class="material-symbols-outlined text-orange-400 mr-3 mt-0.5"
                                                >event_busy</span
                                            >
                                            <div>
                                                <p
                                                    class="text-sm font-medium text-gray-900 dark:text-white"
                                                >
                                                    Férias Programadas
                                                </p>
                                                <p
                                                    class="text-xs text-gray-500 dark:text-gray-400 mt-1"
                                                >
                                                    15 de Julho - 30 de Julho
                                                </p>
                                                <button
                                                    class="mt-3 text-xs text-primary font-medium hover:text-primary-hover"
                                                    >Editar Ausência</button
                                                >
                                            </div>
                                        </div>
                                    </div>
                                    <div class="mt-4">
                                        <button
                                            class="w-full flex items-center justify-center px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-sm font-medium text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors"
                                        >
                                            <span
                                                class="material-symbols-outlined text-[18px] mr-2"
                                                >add</span
                                            >
                                            Adicionar Bloqueio de Agenda
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div
                            class="border-t border-border-light dark:border-border-dark"
                        ></div>
                        <div class="space-y-6">
                            <div class="flex items-center justify-between">
                                <h3
                                    class="text-lg font-semibold text-gray-900 dark:text-white flex items-center"
                                >
                                    <span
                                        class="material-symbols-outlined mr-2 text-primary"
                                        >edit_calendar</span
                                    >
                                    Configuração de Disponibilidade
                                </h3>
                                <button
                                    class="text-sm text-primary hover:text-primary-hover font-medium"
                                >
                                    Copiar Semana Anterior
                                </button>
                            </div>
                            <div
                                class="bg-surface-light dark:bg-surface-dark border border-border-light dark:border-border-dark rounded-lg p-6"
                            >
                                <h4
                                    class="text-sm font-medium text-gray-700 dark:text-gray-300 mb-4"
                                >
                                    Adicionar Novo Horário
                                </h4>
                                <div
                                    class="grid grid-cols-1 md:grid-cols-12 gap-4 items-end"
                                >
                                    <div class="md:col-span-3">
                                        <label
                                            class="block text-xs font-medium text-gray-500 dark:text-gray-400 mb-1"
                                            for="day-of-week"
                                            >Dia da Semana</label
                                        >
                                        <select
                                            id="day-of-week"
                                            class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                        >
                                            <option>Segunda-feira</option>
                                            <option>Terça-feira</option>
                                            <option>Quarta-feira</option>
                                            <option>Quinta-feira</option>
                                            <option>Sexta-feira</option>
                                            <option>Sábado</option>
                                            <option>Domingo</option>
                                        </select>
                                    </div>
                                    <div class="md:col-span-2">
                                        <label
                                            class="block text-xs font-medium text-gray-500 dark:text-gray-400 mb-1"
                                            for="start-time">Início</label
                                        >
                                        <input
                                            id="start-time"
                                            class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                            type="time"
                                            value="09:00"
                                        />
                                    </div>
                                    <div class="md:col-span-2">
                                        <label
                                            class="block text-xs font-medium text-gray-500 dark:text-gray-400 mb-1"
                                            for="end-time">Fim</label
                                        >
                                        <input
                                            id="end-time"
                                            class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors"
                                            type="time"
                                            value="18:00"
                                        />
                                    </div>
                                    <div class="md:col-span-3">
                                        <label
                                            class="block text-xs font-medium text-gray-500 dark:text-gray-400 mb-1"
                                            for="break-start"
                                            >Intervalo (Almoço)</label
                                        >
                                        <div
                                            class="flex items-center space-x-2"
                                        >
                                            <input
                                                id="break-start"
                                                aria-label="Início do intervalo"
                                                class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors text-xs"
                                                placeholder="12:00"
                                                type="time"
                                            />
                                            <span class="text-gray-400">-</span>
                                            <input
                                                id="break-end"
                                                aria-label="Fim do intervalo"
                                                class="block w-full rounded-md border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-white shadow-sm focus:border-input-focus focus:ring-input-focus sm:text-sm py-2.5 transition-colors text-xs"
                                                placeholder="13:00"
                                                type="time"
                                            />
                                        </div>
                                    </div>
                                    <div class="md:col-span-2">
                                        <button
                                            class="w-full flex items-center justify-center px-4 py-2.5 bg-primary hover:bg-primary-hover text-white rounded-md font-medium shadow-md transition-colors"
                                        >
                                            <span
                                                class="material-symbols-outlined text-[20px] mr-1"
                                                >add_circle</span
                                            >
                                            Adicionar
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div
                        class="px-6 py-4 bg-gray-50 dark:bg-gray-800/50 border-t border-border-light dark:border-border-dark flex items-center justify-end space-x-3"
                    >
                        <button
                            class="px-4 py-2 border border-gray-300 dark:border-gray-600 rounded-md text-gray-700 dark:text-gray-300 font-medium hover:bg-white dark:hover:bg-gray-700 transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                        >
                            Descartar Alterações
                        </button>
                        <button
                            class="px-6 py-2 bg-primary hover:bg-primary-hover text-white rounded-md font-medium shadow-md transition-colors flex items-center focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary"
                        >
                            <span
                                class="material-symbols-outlined text-[20px] mr-2"
                                >save</span
                            >
                            Salvar Alterações
                        </button>
                    </div>
                </div>
                <div
                    class="bg-orange-50 dark:bg-orange-900/10 border border-orange-100 dark:border-orange-900/30 rounded-lg p-4 flex items-start"
                >
                    <span
                        class="material-symbols-outlined text-orange-500 mt-0.5 mr-3"
                        >security</span
                    >
                    <div>
                        <h4
                            class="text-sm font-medium text-orange-800 dark:text-orange-300"
                        >
                            Segurança da Conta
                        </h4>
                        <p
                            class="text-sm text-orange-700/80 dark:text-orange-400/80 mt-1"
                        >
                            Sua senha foi alterada pela última vez há 45 dias.
                            Recomendamos a troca a cada 90 dias para manter sua
                            conta segura.
                        </p>
                        <a
                            class="inline-block mt-2 text-sm font-medium text-orange-700 dark:text-orange-400 hover:underline"
                            href="/">Alterar senha</a
                        >
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
