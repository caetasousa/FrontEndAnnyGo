<script lang="ts">
    import Sidebar from "$lib/components/Sidebar.svelte";
    import ThemeToggle from "$lib/components/ThemeToggle.svelte";
</script>

<div
    class="bg-background-light dark:bg-background-dark text-gray-800 dark:text-gray-200 font-sans antialiased transition-colors duration-200"
>
    <div class="flex h-screen overflow-hidden">
        <Sidebar />
        <main class="flex-1 flex flex-col h-full overflow-hidden relative">
            <header
                class="h-16 bg-surface-light dark:bg-surface-dark border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 flex-shrink-0 z-10"
            >
                <div class="flex items-center flex-1 max-w-2xl gap-4">
                    <div class="relative w-full">
                        <div
                            class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none"
                        >
                            <span class="material-icons-outlined text-gray-400"
                            ></span>
                        </div>
                        <input
                            class="block w-full pl-10 pr-4 py-2 rounded-lg bg-gray-100 dark:bg-gray-800 border-transparent focus:border-primary focus:bg-white dark:focus:bg-gray-700 text-sm text-gray-900 dark:text-white focus:ring-0 placeholder-gray-500 dark:placeholder-gray-400 transition-colors"
                            placeholder="Busque por serviço, profissional ou categoria..."
                            type="text"
                        />
                    </div>
                    <button
                        class="p-2 text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800 rounded-lg md:hidden"
                    >
                        <span class="material-symbols-outlined">filter_list</span>
                    </button>
                </div>
                <div class="flex items-center gap-4 ml-4">
                    <ThemeToggle />
                    <button
                        class="relative p-2 text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-800 rounded-full"
                    >
                        <span class="material-symbols-outlined"
                            >notifications</span
                        >
                        <span
                            class="absolute top-2 right-2 w-2 h-2 bg-red-500 rounded-full border-2 border-white dark:border-gray-800"
                        ></span>
                    </button>
                    <button class="flex items-center gap-2">
                        <img
                            alt="User Profile"
                            class="w-8 h-8 rounded-full border border-gray-200 dark:border-gray-700 object-cover"
                            src="https://lh3.googleusercontent.com/aida-public/AB6AXuAZTw1c-yjUq0YdRFLzOa9lzVbwYTsa0rNBJ0iRXfdJ0ucz-g-ytvtpjP6RPDojxo_sQXfIiw2eyo93E14KXTSBt1M_cuMT-_Vvy-I32S-_BICwkdyojVRCd6cmDn0ZRCZ6geIZ4ikx5I8DhWDLBqyuzo0INYKJzNqOhVsFDj33UJQR41oZwdquJz_zmNif-VryEH7kVcG3X_oksqUjpDkqvYDkIy1kjwfL5m8Yf9t-uw19MQZuPLYTlNarjeii1eJ-ElptwMvmlmU"
                        />
                    </button>
                </div>
            </header>
            <div class="flex-1 overflow-y-auto p-6 md:p-8">
                <div
                    class="relative rounded-2xl overflow-hidden bg-gradient-to-r from-orange-500 to-amber-600 dark:from-orange-600 dark:to-amber-800 shadow-lg mb-10 text-white"
                >
                    <div
                        class="absolute inset-0 bg-[url('https://www.transparenttextures.com/patterns/cubes.png')] opacity-10"
                    ></div>
                    <div
                        class="relative z-10 p-8 md:p-12 flex flex-col md:flex-row items-center justify-between gap-8"
                    >
                        <div class="max-w-xl space-y-4">
                            <span
                                class="inline-block px-3 py-1 bg-white/20 backdrop-blur-sm rounded-full text-xs font-semibold tracking-wide uppercase"
                                >Oferta da Semana</span
                            >
                            <h1
                                class="text-3xl md:text-5xl font-bold font-display leading-tight"
                            >
                                Renove sua beleza com nossos especialistas
                            </h1>
                            <p class="text-white/90 text-lg max-w-md">
                                Descubra os melhores tratamentos capilares e
                                estéticos selecionados para você.
                            </p>
                            <div class="pt-4 flex gap-3">
                                <button
                                    class="px-6 py-3 bg-white text-orange-600 font-semibold rounded-lg shadow-sm hover:bg-gray-50 transition"
                                    >Agendar Agora</button
                                >
                                <button
                                    class="px-6 py-3 bg-transparent border border-white text-white font-semibold rounded-lg hover:bg-white/10 transition"
                                    >Ver Promoções</button
                                >
                            </div>
                        </div>
                        <div class="hidden md:block relative w-80 h-64">
                            <div
                                class="absolute top-0 right-0 w-64 h-48 bg-gray-900 rounded-lg rotate-3 shadow-2xl overflow-hidden border-4 border-white/20"
                            >
                                <img
                                    alt="Hair styling"
                                    class="w-full h-full object-cover opacity-80"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuCYxtIrdDpKj4dz4C07_cfWm0fxJ2SvIE1Zx3hpSCCNfZ1s81dDodesar6jXm1yeDKsi5KDsz5teIegxCm_3tPCdafgyTDbGD2dvKkGff75y7ghdBAP8YrsSshcpmAl2T8XYzbHFKFhIDtvz-CEFaXRGrsXoNics974oaCLIhQDimGQNRYyCpGZ8FzS9vfNt_NanQ2xDXKAUgG4osP3ifwEvHBY_QFu0pvgHpUjsKpX8_zJarrWTLTaNxWvABA3eivnvNdyPnRmypg"
                                />
                            </div>
                            <div
                                class="absolute bottom-0 left-4 w-48 h-36 bg-orange-700 rounded-lg -rotate-6 shadow-xl overflow-hidden border-4 border-white/20 z-20"
                            >
                                <img
                                    alt="Makeup"
                                    class="w-full h-full object-cover"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuCVqEmwaujVPrnjTd8HxPLpAqQMN9g6nrZiCfnHhptuRvhgO5h7ZS4l0G1CD70g0NkKNEK0bWQJoSCvcMJ3wGHOGJUzK-fER2p6jTMlIKOHvwf-R6VzkS_5shKPV8K79XldmB851IJLkvg_DUvRgrlmx9FJCka0ZuKnQRjLRYEHbitvjZ9idVnNaC3nZ4qoHzW-6en_s56qqbR1NiXH5cRi1xdfwOE7X45e-4lSD3HDhNbGX1ZI4awIB6hNaTxyU5vnkKcng-wQuHg"
                                />
                            </div>
                            <span
                                class="material-icons-outlined absolute -top-4 left-10 text-white/40 text-6xl animate-pulse"
                                >spa</span
                            >
                        </div>
                    </div>
                </div>
                <div
                    class="flex overflow-x-auto pb-4 gap-3 mb-8 no-scrollbar scroll-smooth"
                >
                    <button
                        class="whitespace-nowrap px-4 py-2 bg-primary text-white text-sm font-medium rounded-full shadow-md shadow-primary/30"
                    >
                        Todos os Serviços
                    </button>
                    <button
                        class="whitespace-nowrap px-4 py-2 bg-surface-light dark:bg-surface-dark border border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 text-sm font-medium rounded-full transition-colors"
                    >
                        Cabelos
                    </button>
                    <button
                        class="whitespace-nowrap px-4 py-2 bg-surface-light dark:bg-surface-dark border border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 text-sm font-medium rounded-full transition-colors"
                    >
                        Manicure &amp; Pedicure
                    </button>
                    <button
                        class="whitespace-nowrap px-4 py-2 bg-surface-light dark:bg-surface-dark border border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 text-sm font-medium rounded-full transition-colors"
                    >
                        Estética Facial
                    </button>
                    <button
                        class="whitespace-nowrap px-4 py-2 bg-surface-light dark:bg-surface-dark border border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 text-sm font-medium rounded-full transition-colors"
                    >
                        Cílios &amp; Sobrancelhas
                    </button>
                    <button
                        class="whitespace-nowrap px-4 py-2 bg-surface-light dark:bg-surface-dark border border-border-light dark:border-border-dark text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 text-sm font-medium rounded-full transition-colors"
                    >
                        Maquiagem
                    </button>
                </div>
                <div class="mb-10">
                    <div class="flex items-center justify-between mb-6">
                        <h2
                            class="text-xl font-bold text-gray-900 dark:text-white flex items-center gap-2"
                        >
                            <span class="material-symbols-outlined text-primary"
                                >local_fire_department</span
                            >
                            Mais Procurados
                        </h2>
                        <a
                            class="text-sm font-medium text-primary hover:text-orange-600 dark:hover:text-orange-400"
                            href="/">Ver tudo</a
                        >
                    </div>
                    <div
                        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6"
                    >
                        <div
                            class="group bg-surface-light dark:bg-surface-dark rounded-xl border border-border-light dark:border-border-dark overflow-hidden hover:shadow-lg transition-all duration-300 flex flex-col"
                        >
                            <div class="relative h-48 overflow-hidden">
                                <div
                                    class="absolute top-3 right-3 z-10 bg-white/90 dark:bg-black/80 backdrop-blur px-2 py-1 rounded text-xs font-bold text-gray-900 dark:text-white shadow-sm flex items-center gap-1"
                                >
                                    <span
                                        class="material-symbols-outlined text-yellow-500 text-xs"
                                        >star</span
                                    > 4.9
                                </div>
                                <img
                                    alt="Corte de Cabelo"
                                    class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuCxXP3_nwScNevenWB4-fGAhGXc7iA5jxBdhn_V40PRRdJaCvfYn0d-_zAbgBS7w-rMU-MZvK0RPwT6I3trFkBoY6bifrOPYlLmD3vmZv8ZvBq8emoX6iuCtyb7os2a8XLkMbAJWpItw8L7M-xyEbEQDkVjuDJpa6qztpq4MRSsh1HW-AGMh9UaVI3He6_dX_wBlNV_eIOxHNI04X82juFyAEzhfRFPgGehuH1hu65BYmamQsY4qEN0Lyl0LUBm4oHVhUkTSEQYmCg"
                                />
                                <div
                                    class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/60 to-transparent h-20"
                                ></div>
                            </div>
                            <div class="p-4 flex-1 flex flex-col">
                                <div
                                    class="mb-1 text-xs font-semibold text-primary uppercase tracking-wide"
                                >
                                    Cabelos
                                </div>
                                <h3
                                    class="text-lg font-bold text-gray-900 dark:text-white mb-2"
                                >
                                    Corte e Hidratação
                                </h3>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 line-clamp-2 mb-4"
                                >
                                    Corte personalizado com visagismo e
                                    hidratação profunda.
                                </p>
                                <div
                                    class="mt-auto flex items-center justify-between pt-4 border-t border-border-light dark:border-border-dark"
                                >
                                    <div>
                                        <span
                                            class="text-xs text-gray-400 block"
                                            >A partir de</span
                                        >
                                        <span
                                            class="text-lg font-bold text-gray-900 dark:text-white"
                                            >R$ 120,00</span
                                        >
                                    </div>
                                    <button
                                        class="p-2 bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 rounded-lg hover:bg-primary hover:text-white dark:hover:bg-primary dark:hover:text-white transition-colors"
                                    >
                                        <span class="material-symbols-outlined"
                                            >arrow_forward</span
                                        >
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div
                            class="group bg-surface-light dark:bg-surface-dark rounded-xl border border-border-light dark:border-border-dark overflow-hidden hover:shadow-lg transition-all duration-300 flex flex-col"
                        >
                            <div class="relative h-48 overflow-hidden">
                                <div
                                    class="absolute top-3 right-3 z-10 bg-white/90 dark:bg-black/80 backdrop-blur px-2 py-1 rounded text-xs font-bold text-gray-900 dark:text-white shadow-sm flex items-center gap-1"
                                >
                                    <span
                                        class="material-icons-outlined text-yellow-500 text-xs"
                                        >star</span
                                    > 5.0
                                </div>
                                <img
                                    alt="Manicure"
                                    class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuBhFiOln_rEyvs4CsRnLviJYo5q-e1z4xjSdSu5HEhfOo1ZFwRs8Yz33SWo6Hc3qAbvoe7p2t44wR4ZGNmRr4qgzsbiD0eE0HXxtKG6rG6mCzipkLAjY-0OQpwlnnBAKJFXF9Nv_v3i1-gSCxxhzZN1qQowsCrXSyURMPImq7Jv9SUm36zZsLla8IhbNrnXzwQj_RqX9uG25jaEGdBEcthS_iZ00mOdpXXdZY_WT3Mn0nt9mAtsa2Wx6tOmgJYuW3UD5CQVjhLlLUQ"
                                />
                                <div
                                    class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/60 to-transparent h-20"
                                ></div>
                            </div>
                            <div class="p-4 flex-1 flex flex-col">
                                <div
                                    class="mb-1 text-xs font-semibold text-primary uppercase tracking-wide"
                                >
                                    Unhas
                                </div>
                                <h3
                                    class="text-lg font-bold text-gray-900 dark:text-white mb-2"
                                >
                                    Manicure em Gel
                                </h3>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 line-clamp-2 mb-4"
                                >
                                    Aplicação de unhas em gel com acabamento
                                    natural e duradouro.
                                </p>
                                <div
                                    class="mt-auto flex items-center justify-between pt-4 border-t border-border-light dark:border-border-dark"
                                >
                                    <div>
                                        <span
                                            class="text-xs text-gray-400 block"
                                            >A partir de</span
                                        >
                                        <span
                                            class="text-lg font-bold text-gray-900 dark:text-white"
                                            >R$ 85,00</span
                                        >
                                    </div>
                                    <button
                                        class="p-2 bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 rounded-lg hover:bg-primary hover:text-white dark:hover:bg-primary dark:hover:text-white transition-colors"
                                    >
                                        <span class="material-symbols-outlined"
                                            >arrow_forward</span
                                        >
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div
                            class="group bg-surface-light dark:bg-surface-dark rounded-xl border border-border-light dark:border-border-dark overflow-hidden hover:shadow-lg transition-all duration-300 flex flex-col"
                        >
                            <div class="relative h-48 overflow-hidden">
                                <div
                                    class="absolute top-3 right-3 z-10 bg-white/90 dark:bg-black/80 backdrop-blur px-2 py-1 rounded text-xs font-bold text-gray-900 dark:text-white shadow-sm flex items-center gap-1"
                                >
                                    <span
                                        class="material-icons-outlined text-yellow-500 text-xs"
                                        >star</span
                                    > 4.8
                                </div>
                                <img
                                    alt="Sobrancelhas"
                                    class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuDTaYu7tGMsiaGIhwjQJ7I-fUsGrLaGv8HZQ8l8s9UyrKDmiLAZtKmIh5m_3diMXlr_ddAGKKyE1CDjJ8qSNIM4xWe1Oo6FIAVHAycYBA9SqAVZfTQ8wGkeEuqPgPFj_g_jS3I-De2AjZfl34XYX5ldsQLPTzkyD4_9uAEYeYutXpU-srSpkCqnF0NCVEi0fxPwPKuW-H5htCxomFgsgB7V-jXah3NNMpL0FEffKEIyOzpCkCYgbn7yG8Cbl6C92qEk8Leo2H2-v8k"
                                />
                                <div
                                    class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/60 to-transparent h-20"
                                ></div>
                            </div>
                            <div class="p-4 flex-1 flex flex-col">
                                <div
                                    class="mb-1 text-xs font-semibold text-primary uppercase tracking-wide"
                                >
                                    Rosto
                                </div>
                                <h3
                                    class="text-lg font-bold text-gray-900 dark:text-white mb-2"
                                >
                                    Design de Sobrancelha
                                </h3>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 line-clamp-2 mb-4"
                                >
                                    Design com henna para realçar o olhar de
                                    forma harmônica.
                                </p>
                                <div
                                    class="mt-auto flex items-center justify-between pt-4 border-t border-border-light dark:border-border-dark"
                                >
                                    <div>
                                        <span
                                            class="text-xs text-gray-400 block"
                                            >A partir de</span
                                        >
                                        <span
                                            class="text-lg font-bold text-gray-900 dark:text-white"
                                            >R$ 45,00</span
                                        >
                                    </div>
                                    <button
                                        class="p-2 bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 rounded-lg hover:bg-primary hover:text-white dark:hover:bg-primary dark:hover:text-white transition-colors"
                                    >
                                        <span class="material-symbols-outlined"
                                            >arrow_forward</span
                                        >
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div
                            class="group bg-surface-light dark:bg-surface-dark rounded-xl border border-border-light dark:border-border-dark overflow-hidden hover:shadow-lg transition-all duration-300 flex flex-col"
                        >
                            <div class="relative h-48 overflow-hidden">
                                <div
                                    class="absolute top-3 right-3 z-10 bg-white/90 dark:bg-black/80 backdrop-blur px-2 py-1 rounded text-xs font-bold text-gray-900 dark:text-white shadow-sm flex items-center gap-1"
                                >
                                    <span
                                        class="material-icons-outlined text-yellow-500 text-xs"
                                        >star</span
                                    > 5.0
                                </div>
                                <img
                                    alt="Maquiagem"
                                    class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuBhqBmwiqx0qeJAkn2_oDnsxFdk2O6MBSTN4f_OYijB_FGRQs_4hIyMqk4az8fnQgHRvJbSr2bXr8hIwddyhOSNe5whOcyl5jDoExKEWM684viMgEKT5KCA-q0O7KIndgZpVs06fJsx77Adnjb-7Ro-EGECp_bwZR8_4jThsXRUMnmyz8ImgT7Ob_Kh0S7QOokTCQ2QijEypXX5X8o1BzfPLQ111vLgmj_TfSSxvMEFKOSMxEwgSU6eteEaj8i59EL3eFKN6kUkNdg"
                                />
                                <div
                                    class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-black/60 to-transparent h-20"
                                ></div>
                            </div>
                            <div class="p-4 flex-1 flex flex-col">
                                <div
                                    class="mb-1 text-xs font-semibold text-primary uppercase tracking-wide"
                                >
                                    Make-up
                                </div>
                                <h3
                                    class="text-lg font-bold text-gray-900 dark:text-white mb-2"
                                >
                                    Maquiagem Social
                                </h3>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 line-clamp-2 mb-4"
                                >
                                    Maquiagem profissional para eventos e festas
                                    com produtos premium.
                                </p>
                                <div
                                    class="mt-auto flex items-center justify-between pt-4 border-t border-border-light dark:border-border-dark"
                                >
                                    <div>
                                        <span
                                            class="text-xs text-gray-400 block"
                                            >A partir de</span
                                        >
                                        <span
                                            class="text-lg font-bold text-gray-900 dark:text-white"
                                            >R$ 150,00</span
                                        >
                                    </div>
                                    <button
                                        class="p-2 bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 rounded-lg hover:bg-primary hover:text-white dark:hover:bg-primary dark:hover:text-white transition-colors"
                                    >
                                        <span class="material-symbols-outlined"
                                            >arrow_forward</span
                                        >
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="mb-10">
                    <h2
                        class="text-xl font-bold text-gray-900 dark:text-white mb-6"
                    >
                        Todos os Serviços
                    </h2>
                    <div
                        class="bg-surface-light dark:bg-surface-dark rounded-xl border border-border-light dark:border-border-dark divide-y divide-border-light dark:divide-border-dark"
                    >
                        <div
                            class="p-6 flex flex-col md:flex-row gap-6 items-start md:items-center hover:bg-gray-50 dark:hover:bg-gray-800/50 transition-colors"
                        >
                            <div
                                class="w-20 h-20 rounded-lg bg-gray-200 dark:bg-gray-700 overflow-hidden flex-shrink-0"
                            >
                                <img
                                    alt="Extensão de Cílios"
                                    class="w-full h-full object-cover"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuD1Qmbp2I2YWtuLnsQ8XNJtrJx7L3SmM7c2yfHjEZ20D_-Pud6zgxa4Cwj5bWWhKlYkS8y1TnW7ud5Q8zp3n9fWIerM3phnbQswzp-rlD2kC-_RgHEsnKRRLg1FpfuSkebkpfklZar2VFcwYbMN5ZoU0ctit4w_W8pM4CgNSXCMuIaBp7OLCoF4jfo55JUXfRk60pTCjg9ifmvixML78UNNoZI5fJcBjljydq9oBGLyWwyFd0WAH7QX-zr23kJFgbmXOXCLfbALcE8"
                                />
                            </div>
                            <div class="flex-1">
                                <div class="flex items-center gap-2 mb-1">
                                    <h3
                                        class="text-lg font-bold text-gray-900 dark:text-white"
                                    >
                                        Extensão de Cílios Volume Russo
                                    </h3>
                                    <span
                                        class="px-2 py-0.5 bg-green-100 dark:bg-green-900 text-green-700 dark:text-green-300 text-xs font-semibold rounded-full"
                                        >Novo</span
                                    >
                                </div>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 mb-2"
                                >
                                    Técnica avançada que aplica múltiplos fios
                                    finos em cada cílio natural.
                                </p>
                                <div
                                    class="flex items-center gap-4 text-xs text-gray-500"
                                >
                                    <span class="flex items-center gap-1"
                                        ><span
                                            class="material-symbols-outlined text-sm"
                                            >schedule</span
                                        > 120 min</span
                                    >
                                    <span class="flex items-center gap-1"
                                        ><span
                                            class="material-symbols-outlined text-sm"
                                            >person</span
                                        > Prof. Ana &amp; Carla</span
                                    >
                                </div>
                            </div>
                            <div
                                class="flex flex-col items-end gap-2 w-full md:w-auto"
                            >
                                <div
                                    class="text-xl font-bold text-gray-900 dark:text-white"
                                >
                                    R$ 180,00
                                </div>
                                <button
                                    class="w-full md:w-auto px-4 py-2 bg-primary hover:bg-orange-600 text-white text-sm font-medium rounded-lg transition-colors"
                                >
                                    Agendar
                                </button>
                            </div>
                        </div>
                        <div
                            class="p-6 flex flex-col md:flex-row gap-6 items-start md:items-center hover:bg-gray-50 dark:hover:bg-gray-800/50 transition-colors"
                        >
                            <div
                                class="w-20 h-20 rounded-lg bg-gray-200 dark:bg-gray-700 overflow-hidden flex-shrink-0"
                            >
                                <img
                                    alt="Coloração"
                                    class="w-full h-full object-cover"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuDD4JE3Zsj1WIdbX1jDDcjRuqycv-VKnhim-Dq9dPe246Ik3QX7-X-MWrOZ1th892OtjSyA5NqBvXG1Yoyz3IVWkRqTHaMNdr2h7YMN2FzXSts4RvtbcvEAyUA7yWOa2xS9xkbMCNl9MidBCSlUJ1VZynWQqKFSR10tPN03Tdo1h10YLPJEWFkQtbQOo66SQuonay45vrnU2sxcH9cJaxOuKBGXs9Of7eIql4V0oH9cztTbowEjAxwHiGuuMs-9qNtvHovidxAjSrk"
                                />
                            </div>
                            <div class="flex-1">
                                <div class="flex items-center gap-2 mb-1">
                                    <h3
                                        class="text-lg font-bold text-gray-900 dark:text-white"
                                    >
                                        Coloração Completa
                                    </h3>
                                </div>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 mb-2"
                                >
                                    Coloração profissional com produtos
                                    Schwarzkopf ou L'Oréal.
                                </p>
                                <div
                                    class="flex items-center gap-4 text-xs text-gray-500"
                                >
                                    <span class="flex items-center gap-1"
                                        ><span
                                            class="material-icons-outlined text-sm"
                                            >schedule</span
                                        > 90 min</span
                                    >
                                    <span class="flex items-center gap-1"
                                        ><span
                                            class="material-icons-outlined text-sm"
                                            >person</span
                                        > Prof. Marcos</span
                                    >
                                </div>
                            </div>
                            <div
                                class="flex flex-col items-end gap-2 w-full md:w-auto"
                            >
                                <div
                                    class="text-xl font-bold text-gray-900 dark:text-white"
                                >
                                    R$ 220,00
                                </div>
                                <button
                                    class="w-full md:w-auto px-4 py-2 bg-primary hover:bg-orange-600 text-white text-sm font-medium rounded-lg transition-colors"
                                >
                                    Agendar
                                </button>
                            </div>
                        </div>
                        <div
                            class="p-6 flex flex-col md:flex-row gap-6 items-start md:items-center hover:bg-gray-50 dark:hover:bg-gray-800/50 transition-colors"
                        >
                            <div
                                class="w-20 h-20 rounded-lg bg-gray-200 dark:bg-gray-700 overflow-hidden flex-shrink-0"
                            >
                                <img
                                    alt="Spa dos Pés"
                                    class="w-full h-full object-cover"
                                    src="https://lh3.googleusercontent.com/aida-public/AB6AXuCYI4Dq9bnevoQIZuj2PJd5sQ4PJBe3_TNfOF5NgnNANz2FtSZXRf9Nhw2_QOQM8xiLcdqWsMUIKFTFQRDUxx1WXBK4t5PNeUowqoBkP67l9oOJO8Btj5mXwXj1pvcsPWd7QaPPJXm2WFEqVZQTLc2ZLAOmyjMSqb4msyJMQwyKfIkWE3GsqUnRegwkkRZG2rY4MUqb-HEsu9ZcZEJ5-xsGqq7zns6kgo4kFTKpd0WpZ3eeNGtUjDAdKw_UqqMDjmC5OZPw6sFoI54"
                                />
                            </div>
                            <div class="flex-1">
                                <div class="flex items-center gap-2 mb-1">
                                    <h3
                                        class="text-lg font-bold text-gray-900 dark:text-white"
                                    >
                                        Spa dos Pés + Pedicure
                                    </h3>
                                </div>
                                <p
                                    class="text-sm text-gray-500 dark:text-gray-400 mb-2"
                                >
                                    Esfoliação, hidratação profunda e massagem
                                    relaxante nos pés.
                                </p>
                                <div
                                    class="flex items-center gap-4 text-xs text-gray-500"
                                >
                                    <span class="flex items-center gap-1"
                                        ><span
                                            class="material-icons-outlined text-sm"
                                            >schedule</span
                                        > 60 min</span
                                    >
                                    <span class="flex items-center gap-1"
                                        ><span
                                            class="material-icons-outlined text-sm"
                                            >person</span
                                        > Prof. Júlia</span
                                    >
                                </div>
                            </div>
                            <div
                                class="flex flex-col items-end gap-2 w-full md:w-auto"
                            >
                                <div
                                    class="text-xl font-bold text-gray-900 dark:text-white"
                                >
                                    R$ 95,00
                                </div>
                                <button
                                    class="w-full md:w-auto px-4 py-2 bg-primary hover:bg-orange-600 text-white text-sm font-medium rounded-lg transition-colors"
                                >
                                    Agendar
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
                <div
                    class="mt-12 border-t border-border-light dark:border-border-dark pt-8 pb-4 text-center text-sm text-gray-500"
                >
                    <p>© 2023 BellaVita. Todos os direitos reservados.</p>
                    <div class="flex justify-center gap-4 mt-2">
                        <a class="hover:text-primary" href="/">Termos</a>
                        <a class="hover:text-primary" href="/">Privacidade</a>
                        <a class="hover:text-primary" href="/">Ajuda</a>
                    </div>
                </div>
            </div>
        </main>
    </div>
</div>
