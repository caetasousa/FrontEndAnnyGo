<script lang="ts">
    import Sidebar from "$lib/components/Sidebar.svelte";
    import ThemeToggle from "$lib/components/ThemeToggle.svelte";
    import { page } from "$app/stores";

    // Mock data for providers (matching the listing page)
    const providers = [
        {
            id: 1,
            nome: "Ana Maria Costa",
            especialidade: "Cabeleireira Especialista",
            foto: "https://images.unsplash.com/photo-1544005313-94ddf0286df2?auto=format&fit=crop&q=80&w=400",
            rating: 4.9,
            reviews: 128,
            status: "disponivel",
            biografia:
                "Especialista em cortes modernos e coloração com mais de 5 anos de experiência nas melhores academias do país.",
            email: "ana.costa@bellavita.com",
            telefone: "(11) 98765-4321",
            servicos: [
                {
                    nome: "Corte de Cabelo",
                    preco: 80,
                    duracao: 45,
                    categoria: "Cabelo",
                },
                { nome: "Escova", preco: 50, duracao: 30, categoria: "Cabelo" },
                {
                    nome: "Hidratação Profunda",
                    preco: 120,
                    duracao: 60,
                    categoria: "Tratamento",
                },
            ],
        },
        {
            id: 2,
            nome: "Ricardo Santos",
            especialidade: "Barbeiro & Visagista",
            foto: "https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?auto=format&fit=crop&q=80&w=400",
            rating: 4.8,
            reviews: 95,
            status: "disponivel",
            biografia:
                "Mestre em barbas clássicas e degradês modernos, com foco em simetria e estilo pessoal.",
            email: "ricardo.santos@bellavita.com",
            telefone: "(11) 98765-4322",
            servicos: [
                {
                    nome: "Corte Masculino",
                    preco: 60,
                    duracao: 30,
                    categoria: "Barba",
                },
                {
                    nome: "Design de Barba",
                    preco: 40,
                    duracao: 20,
                    categoria: "Barba",
                },
            ],
        },
        {
            id: 3,
            nome: "Juliana Silva",
            especialidade: "Manicure & Nail Art",
            foto: "https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&q=80&w=400",
            rating: 5.0,
            reviews: 210,
            status: "ocupado",
            biografia:
                "Apaixonada por detalhes e tendências em unhas, especialista em alongamentos e artes personalizadas.",
            email: "juliana.silva@bellavita.com",
            telefone: "(11) 98765-4323",
            servicos: [
                {
                    nome: "Manicure Simples",
                    preco: 45,
                    duracao: 40,
                    categoria: "Unhas",
                },
                {
                    nome: "Nail Art Especial",
                    preco: 25,
                    duracao: 20,
                    categoria: "Unhas",
                },
            ],
        },
    ];

    // Derive the provider based on the ID in the URL
    $: id = parseInt($page.params.id ?? "1");
    $: provider = providers.find((p) => p.id === id) || providers[0]; // Fallback to first for demo

</script>

<div
    class="font-body bg-background-light dark:bg-background-dark text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200"
>
    <Sidebar />

    <main class="flex-1 flex flex-col h-full overflow-hidden relative">
        <header
            class="h-16 bg-surface-light dark:bg-surface-dark border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10"
        >
            <div class="flex items-center">
                <a
                    href="/prestadores"
                    class="flex items-center text-gray-500 hover:text-brand-orange transition-colors"
                >
                    <span class="material-symbols-outlined mr-2"
                        >arrow_back</span
                    >
                    <span class="hidden sm:inline">Voltar para a lista</span>
                </a>
            </div>
            <div class="flex items-center space-x-4">
                <ThemeToggle />
            </div>
        </header>

        <div class="flex-1 overflow-y-auto p-6 md:p-8">
            <div class="max-w-4xl mx-auto space-y-6">
                <!-- Breadcrumb -->
                <nav class="flex text-sm text-gray-500 dark:text-gray-400 mb-2">
                    <a href="/" class="hover:text-primary transition-colors"
                        >Início</a
                    >
                    <span class="mx-2">/</span>
                    <a
                        href="/prestadores"
                        class="hover:text-primary transition-colors"
                        >Profissionais</a
                    >
                    <span class="mx-2">/</span>
                    <span class="text-gray-900 dark:text-white font-medium"
                        >{provider.nome}</span
                    >
                </nav>

                <!-- Profile Card -->
                <div
                    class="bg-surface-light dark:bg-surface-dark rounded-xl shadow-sm border border-border-light dark:border-border-dark overflow-hidden"
                >
                    <div
                        class="relative h-40 bg-gradient-to-r from-brand-orange to-amber-500"
                    >
                        <div class="absolute -bottom-12 left-8 flex items-end">
                            <div class="relative">
                                <img
                                    src={provider.foto}
                                    alt={provider.nome}
                                    class="h-32 w-32 rounded-full border-4 border-white dark:border-surface-dark object-cover bg-white shadow-lg"
                                />
                                <span
                                    class="absolute bottom-1 right-1 w-6 h-6 rounded-full border-4 border-white dark:border-surface-dark {provider.status ===
                                    'disponivel'
                                        ? 'bg-green-500'
                                        : 'bg-gray-400'}"
                                ></span>
                            </div>
                            <div class="ml-6 mb-2 hidden sm:block">
                                <h1
                                    class="text-2xl font-bold text-gray-900 dark:text-white"
                                >
                                    {provider.nome}
                                </h1>
                                <p class="text-brand-orange font-medium">
                                    {provider.especialidade}
                                </p>
                            </div>
                        </div>
                    </div>

                    <div class="pt-16 px-8 pb-8 space-y-8">
                        <!-- Mobile Header -->
                        <div class="sm:hidden text-center">
                            <h1
                                class="text-2xl font-bold text-gray-900 dark:text-white"
                            >
                                {provider.nome}
                            </h1>
                            <p class="text-brand-orange font-medium">
                                {provider.especialidade}
                            </p>
                        </div>

                        <!-- Stats & Rating -->
                        <div
                            class="flex flex-wrap items-center justify-center sm:justify-start gap-6 py-4 border-y border-border-light dark:border-border-dark"
                        >
                            <div class="flex items-center text-amber-500">
                                <span class="material-icons text-xl mr-1"
                                    >star</span
                                >
                                <span class="text-lg font-bold"
                                    >{provider.rating}</span
                                >
                                <span class="text-gray-400 ml-1"
                                    >({provider.reviews} reviews)</span
                                >
                            </div>
                            <div class="flex items-center text-gray-500">
                                <span class="material-symbols-outlined mr-2"
                                    >verified</span
                                >
                                <span class="text-sm font-medium"
                                    >Verificado</span
                                >
                            </div>
                            <div class="ml-auto w-full sm:w-auto">
                                <button
                                    class="w-full sm:w-auto px-6 py-2.5 bg-brand-orange hover:bg-brand-orange-hover text-white rounded-md font-bold shadow-md transition-all flex items-center justify-center"
                                >
                                    <span class="material-symbols-outlined mr-2"
                                        >calendar_month</span
                                    >
                                    Agendar com {provider.nome.split(" ")[0]}
                                </button>
                            </div>
                        </div>

                        <!-- Info Sections -->
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                            <div class="md:col-span-2 space-y-8">
                                <!-- Biography -->
                                <section>
                                    <h3
                                        class="text-lg font-bold text-gray-900 dark:text-white flex items-center mb-4"
                                    >
                                        <span
                                            class="material-symbols-outlined mr-2 text-primary"
                                            >info</span
                                        >
                                        Sobre
                                    </h3>
                                    <p
                                        class="text-gray-600 dark:text-gray-300 leading-relaxed text-lg italic"
                                    >
                                        "{provider.biografia}"
                                    </p>
                                </section>

                                <!-- Services -->
                                <section>
                                    <div
                                        class="flex items-center justify-between mb-4"
                                    >
                                        <h3
                                            class="text-lg font-bold text-gray-900 dark:text-white flex items-center"
                                        >
                                            <span
                                                class="material-symbols-outlined mr-2 text-primary"
                                                >content_cut</span
                                            >
                                            Serviços Atribuídos
                                        </h3>
                                    </div>
                                    <div class="grid grid-cols-1 gap-3">
                                        {#each provider.servicos as servico}
                                            <div
                                                class="flex items-center justify-between p-4 rounded-lg bg-gray-50 dark:bg-gray-800/50 border border-border-light dark:border-border-dark hover:border-brand-orange transition-colors"
                                            >
                                                <div>
                                                    <p
                                                        class="font-bold text-gray-900 dark:text-white uppercase tracking-tight"
                                                    >
                                                        {servico.nome}
                                                    </p>
                                                    <p
                                                        class="text-xs text-gray-500"
                                                    >
                                                        {servico.duracao} min • {servico.categoria}
                                                    </p>
                                                </div>
                                                <div class="text-right">
                                                    <p
                                                        class="text-lg font-bold text-brand-orange"
                                                    >
                                                        R$ {servico.preco}
                                                    </p>
                                                </div>
                                            </div>
                                        {/each}
                                    </div>
                                </section>
                            </div>

                            <!-- Sidebar/Contact Info -->
                            <div class="space-y-6">
                                <section
                                    class="p-6 rounded-xl bg-gray-50 dark:bg-gray-800/30 border border-border-light dark:border-border-dark"
                                >
                                    <h3
                                        class="text-sm font-bold text-gray-500 uppercase tracking-widest mb-4"
                                    >
                                        Contato
                                    </h3>
                                    <div class="space-y-4">
                                        <div class="flex items-center text-sm">
                                            <span
                                                class="material-symbols-outlined mr-3 text-gray-400"
                                                >mail</span
                                            >
                                            <span
                                                class="text-gray-700 dark:text-gray-300"
                                                >{provider.email}</span
                                            >
                                        </div>
                                        <div class="flex items-center text-sm">
                                            <span
                                                class="material-symbols-outlined mr-3 text-gray-400"
                                                >call</span
                                            >
                                            <span
                                                class="text-gray-700 dark:text-gray-300"
                                                >{provider.telefone}</span
                                            >
                                        </div>
                                    </div>
                                </section>

                                <section
                                    class="p-6 rounded-xl bg-brand-orange/5 border border-brand-orange/20"
                                >
                                    <h3
                                        class="text-sm font-bold text-brand-orange uppercase tracking-widest mb-4"
                                    >
                                        Disponibilidade
                                    </h3>
                                    <div class="space-y-2 text-sm">
                                        <div class="flex justify-between">
                                            <span class="text-gray-500"
                                                >Seg - Sex</span
                                            >
                                            <span class="font-medium"
                                                >09:00 - 18:00</span
                                            >
                                        </div>
                                        <div class="flex justify-between">
                                            <span class="text-gray-500"
                                                >Sáb</span
                                            >
                                            <span class="font-medium"
                                                >09:00 - 14:00</span
                                            >
                                        </div>
                                        <div
                                            class="flex justify-between text-red-400"
                                        >
                                            <span>Dom</span>
                                            <span>Folga</span>
                                        </div>
                                    </div>
                                </section>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</div>
