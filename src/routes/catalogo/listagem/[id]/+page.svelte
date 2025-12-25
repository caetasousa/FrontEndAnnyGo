<script lang="ts">
    import Sidebar from "$lib/components/Sidebar.svelte";
    import ThemeToggle from "$lib/components/ThemeToggle.svelte";
    import { page } from "$app/stores";

    // Mock data for services (matching the listing page)
    const services = [
        {
            id: 1,
            nome: "Corte de Cabelo Feminino",
            categoria: "Cabelo",
            duracao_padrao: 60,
            preco: 120.0,
            imagem: "https://images.unsplash.com/photo-1560066984-138dadb4c035?auto=format&fit=crop&q=80&w=1200",
            descricao:
                "Um corte personalizado que leva em conta o formato do seu rosto, textura do cabelo e seu estilo de vida. Inclui lavagem com produtos premium e finalização.",
            profissionais: [
                {
                    nome: "Ana Maria Costa",
                    foto: "https://images.unsplash.com/photo-1544005313-94ddf0286df2?auto=format&fit=crop&q=80&w=200",
                },
                {
                    nome: "Fernando Lima",
                    foto: "https://images.unsplash.com/photo-1492562080023-ab3db95bfbce?auto=format&fit=crop&q=80&w=200",
                },
            ],
            detalhes: [
                { label: "Categoria", value: "Cabelo", icon: "content_cut" },
                { label: "Duração", value: "60 min", icon: "schedule" },
                { label: "Preço Base", value: "R$ 120,00", icon: "payments" },
                { label: "Tipo", value: "Principal", icon: "grade" },
            ],
        },
        {
            id: 2,
            nome: "Manicure Completa",
            categoria: "Unhas",
            duracao_padrao: 45,
            preco: 50.0,
            imagem: "https://images.unsplash.com/photo-1560066984-138dadb4c035?auto=format&fit=crop&q=80&w=1200",
            descricao:
                "Tratamento completo para suas mãos, incluindo cutilagem, esfoliação suave e esmaltação com cores vibrantes das melhores marcas.",
            profissionais: [
                {
                    nome: "Juliana Silva",
                    foto: "https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&q=80&w=200",
                },
            ],
            detalhes: [
                { label: "Categoria", value: "Unhas", icon: "front_hand" },
                { label: "Duração", value: "45 min", icon: "schedule" },
                { label: "Preço Base", value: "R$ 50,00", icon: "payments" },
                {
                    label: "Manutenção",
                    value: "Quinzenal",
                    icon: "event_repeat",
                },
            ],
        },
        {
            id: 3,
            nome: "Massagem Relaxante",
            categoria: "Massagem",
            duracao_padrao: 90,
            preco: 150.0,
            imagem: "https://images.unsplash.com/photo-1560066984-138dadb4c035?auto=format&fit=crop&q=80&w=1200",
            descricao:
                "Alivie o estresse e a tensão muscular com nossa massagem relaxante. Utilizamos óleos essenciais e técnicas que promovem o bem-estar profundo.",
            profissionais: [
                {
                    nome: "Marcos Oliveira",
                    foto: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?auto=format&fit=crop&q=80&w=200",
                },
            ],
            detalhes: [
                {
                    label: "Categoria",
                    value: "Massagem",
                    icon: "self_improvement",
                },
                { label: "Duração", value: "90 min", icon: "schedule" },
                { label: "Preço Base", value: "R$ 150,00", icon: "payments" },
                { label: "Intensidade", value: "Média", icon: "tune" },
            ],
        },
    ];

    // Derive the service based on the ID in the URL
    $: id = parseInt($page.params.id ?? "1");
    $: service = services.find((s) => s.id === id) || services[0];
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
                    href="/catalogo/listagem"
                    class="flex items-center text-gray-500 hover:text-brand-orange transition-colors group"
                >
                    <span
                        class="material-symbols-outlined mr-2 group-hover:-translate-x-1 transition-transform"
                        >arrow_back</span
                    >
                    <span class="hidden sm:inline font-medium"
                        >Voltar para o catálogo</span
                    >
                </a>
            </div>
            <div class="flex items-center space-x-4">
                <ThemeToggle />
            </div>
        </header>

        <div class="flex-1 overflow-y-auto p-6 md:p-8">
            <div class="max-w-4xl mx-auto space-y-8">
                <!-- Breadcrumb -->
                <nav class="flex text-sm text-gray-500 dark:text-gray-400 mb-2">
                    <a href="/" class="hover:text-primary transition-colors"
                        >Início</a
                    >
                    <span class="mx-2">/</span>
                    <a
                        href="/catalogo/listagem"
                        class="hover:text-primary transition-colors">Catálogo</a
                    >
                    <span class="mx-2">/</span>
                    <span class="text-gray-900 dark:text-white font-medium"
                        >{service.nome}</span
                    >
                </nav>

                <!-- Service Hero Card -->
                <div
                    class="bg-surface-light dark:bg-surface-dark rounded-3xl shadow-xl border border-border-light dark:border-border-dark overflow-hidden transition-all duration-300"
                >
                    <div
                        class="relative h-64 sm:h-80 bg-gray-200 dark:bg-gray-800"
                    >
                        <img
                            src={service.imagem}
                            alt={service.nome}
                            class="w-full h-full object-cover"
                        />
                        <div
                            class="absolute inset-0 bg-gradient-to-t from-black/60 via-black/20 to-transparent"
                        ></div>
                        <div class="absolute bottom-8 left-8 right-8">
                            <div class="flex items-center gap-3 mb-3">
                                <span
                                    class="px-3 py-1 bg-brand-orange text-white text-[10px] font-black uppercase tracking-widest rounded-full shadow-lg"
                                >
                                    {service.categoria}
                                </span>
                            </div>
                            <h1
                                class="text-3xl md:text-5xl font-black text-white leading-tight drop-shadow-lg"
                            >
                                {service.nome}
                            </h1>
                        </div>
                    </div>

                    <div class="p-8">
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-12">
                            <div class="md:col-span-2 space-y-10">
                                <!-- About Section -->
                                <section>
                                    <h3
                                        class="text-xl font-black text-gray-900 dark:text-white flex items-center mb-6 uppercase tracking-tight"
                                    >
                                        <span
                                            class="material-symbols-outlined mr-3 text-brand-orange text-3xl"
                                            >description</span
                                        >
                                        Sobre o Serviço
                                    </h3>
                                    <p
                                        class="text-gray-600 dark:text-gray-300 leading-relaxed text-lg"
                                    >
                                        {service.descricao}
                                    </p>
                                </section>

                                <!-- Professionals List -->
                                <section>
                                    <h3
                                        class="text-xl font-black text-gray-900 dark:text-white flex items-center mb-6 uppercase tracking-tight"
                                    >
                                        <span
                                            class="material-symbols-outlined mr-3 text-brand-orange text-3xl"
                                            >badge</span
                                        >
                                        Profissionais que Realizam
                                    </h3>
                                    <div
                                        class="grid grid-cols-1 sm:grid-cols-2 gap-4"
                                    >
                                        {#each service.profissionais as prof}
                                            <div
                                                class="flex items-center p-4 rounded-2xl bg-gray-50 dark:bg-gray-800/40 border border-border-light dark:border-border-dark group hover:border-brand-orange/50 transition-colors"
                                            >
                                                <img
                                                    src={prof.foto}
                                                    alt={prof.nome}
                                                    class="h-12 w-12 rounded-full object-cover mr-4 shadow-sm border-2 border-white dark:border-gray-700"
                                                />
                                                <div>
                                                    <p
                                                        class="font-bold text-gray-900 dark:text-white group-hover:text-brand-orange transition-colors"
                                                    >
                                                        {prof.nome}
                                                    </p>
                                                    <p
                                                        class="text-xs text-gray-500 font-medium"
                                                    >
                                                        Especialista {service.categoria}
                                                    </p>
                                                </div>
                                                <span
                                                    class="material-symbols-outlined ml-auto text-gray-300 group-hover:text-brand-orange transition-colors"
                                                    >arrow_forward</span
                                                >
                                            </div>
                                        {/each}
                                    </div>
                                </section>
                            </div>

                            <!-- Detail Sidebar -->
                            <div class="space-y-6">
                                <div
                                    class="bg-gray-50 dark:bg-gray-800/40 rounded-3xl p-6 border border-border-light dark:border-border-dark shadow-inner"
                                >
                                    <h4
                                        class="text-[10px] font-black text-gray-400 dark:text-gray-500 uppercase tracking-[0.2em] mb-6"
                                    >
                                        Especificações
                                    </h4>
                                    <div class="space-y-6">
                                        {#each service.detalhes as detalhe}
                                            <div class="flex items-start gap-4">
                                                <div
                                                    class="h-10 w-10 rounded-xl bg-white dark:bg-gray-800 flex items-center justify-center text-brand-orange shadow-sm border border-border-light/50 dark:border-border-dark/50"
                                                >
                                                    <span
                                                        class="material-symbols-outlined text-[22px]"
                                                        >{detalhe.icon}</span
                                                    >
                                                </div>
                                                <div>
                                                    <p
                                                        class="text-[11px] font-bold text-gray-400 dark:text-gray-500 uppercase tracking-tighter"
                                                    >
                                                        {detalhe.label}
                                                    </p>
                                                    <p
                                                        class="text-md font-black text-gray-900 dark:text-white"
                                                    >
                                                        {detalhe.value}
                                                    </p>
                                                </div>
                                            </div>
                                        {/each}
                                    </div>

                                    <div
                                        class="mt-8 pt-6 border-t border-border-light dark:border-border-dark"
                                    >
                                        <button
                                            class="w-full py-3.5 px-6 bg-brand-orange hover:bg-brand-orange-hover text-white rounded-2xl font-black shadow-lg shadow-brand-orange/20 transition-all hover:scale-[1.02] active:scale-95 flex items-center justify-center gap-3"
                                        >
                                            <span
                                                class="material-symbols-outlined text-2xl"
                                                >calendar_month</span
                                            >
                                            <span class="tracking-widest"
                                                >Agendar</span
                                            >
                                        </button>
                                    </div>
                                </div>

                                <!-- Security/Policy Card -->
                                <div
                                    class="bg-brand-orange/5 rounded-3xl p-6 border border-brand-orange/10"
                                >
                                    <div class="flex items-start gap-4">
                                        <span
                                            class="material-symbols-outlined text-brand-orange"
                                            >verified_user</span
                                        >
                                        <p
                                            class="text-sm text-gray-600 dark:text-gray-400 leading-relaxed font-medium"
                                        >
                                            Cancelamento gratuito até 24h antes
                                            do horário agendado.
                                        </p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</div>
