<script lang="ts">
    import Navbar from "$lib/components/Navbar.svelte";
    import Footer from "$lib/components/Footer.svelte";
</script>

<Navbar />

<div class="relative bg-secondary overflow-hidden">
    <div
        class="absolute inset-0 opacity-20 dark:opacity-10 pointer-events-none"
    >
        <div
            class="absolute top-10 left-10 w-20 h-20 border-4 border-primary rounded-full"
        ></div>
        <div
            class="absolute top-1/2 right-20 w-12 h-12 bg-primary rotate-45 transform"
        ></div>
        <div
            class="absolute bottom-20 left-1/4 text-primary text-9xl font-bold opacity-10"
        >
            +
        </div>
        <div
            class="absolute top-1/4 right-1/4 w-64 h-64 bg-primary rounded-full blur-3xl opacity-20"
        ></div>
    </div>
    <div class="max-w-7xl mx-auto">
        <div
            class="relative z-10 pb-8 sm:pb-16 md:pb-20 lg:max-w-2xl lg:w-full lg:pb-28 xl:pb-32 px-4 sm:px-6 lg:px-8 pt-20"
        >
            <main
                class="mt-10 mx-auto max-w-7xl sm:mt-12 md:mt-16 lg:mt-20 xl:mt-28"
            >
                <div class="sm:text-center lg:text-left">
                    <p
                        class="text-base text-primary font-semibold tracking-wide uppercase"
                    >
                        Beleza &amp; Bem-Estar
                    </p>
                    <h1
                        class="text-4xl tracking-tight font-extrabold text-white sm:text-5xl md:text-6xl"
                    >
                        <span class="block xl:inline">Realce sua</span>
                        <span class="block text-primary xl:inline"
                            >Beleza Natural</span
                        >
                    </h1>
                    <p
                        class="mt-3 text-base text-gray-300 sm:mt-5 sm:text-lg sm:max-w-xl sm:mx-auto md:mt-5 md:text-xl lg:mx-0"
                    >
                        Descubra uma experiência única de cuidado pessoal. Do
                        design de sobrancelhas a tratamentos capilares
                        exclusivos, nossa equipe está pronta para transformar
                        seu dia.
                    </p>
                    <div
                        class="mt-8 sm:mt-10 sm:flex sm:justify-center lg:justify-start gap-4"
                    >
                        <div class="rounded-md shadow">
                            <a
                                class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-secondary bg-white hover:bg-gray-100 md:py-4 md:text-lg md:px-10 transition"
                                href="#services"
                            >
                                Nossos Serviços
                            </a>
                        </div>
                        <div class="mt-3 sm:mt-0 sm:ml-3">
                            <a
                                class="w-full flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-white bg-primary hover:bg-orange-600 md:py-4 md:text-lg md:px-10 transition shadow-lg shadow-orange-500/30"
                                href="/login"
                            >
                                Agendar Visita
                            </a>
                        </div>
                    </div>
                    <div
                        class="mt-8 flex items-center space-x-6 text-gray-400 sm:justify-center lg:justify-start"
                    >
                        <a class="hover:text-primary transition" href="/"
                            ><span class="material-icons">facebook</span></a
                        >
                        <a class="hover:text-primary transition" href="/"
                            ><span class="font-bold text-xl">IG</span></a
                        >
                        <a class="hover:text-primary transition" href="/"
                            ><span class="font-bold text-xl">WA</span></a
                        >
                    </div>
                </div>
            </main>
        </div>
    </div>
    <div class="lg:absolute lg:inset-y-0 lg:right-0 lg:w-1/2 bg-gray-900">
        <img
            alt="Mulher recebendo tratamento de beleza em salão"
            class="h-56 w-full object-cover sm:h-72 md:h-96 lg:w-full lg:h-full opacity-80 hover:opacity-90 transition duration-500"
            src="https://lh3.googleusercontent.com/aida-public/AB6AXuD2ty_jT_I1HeVi_P2vtEXoMn6NKKbwBp3HpGsrDkk4bKJ7YKnUZU6NYqplzt_uB265T0UmyppxjYhOPp0WSVeZfZLwX5AGYEb-TmLvcGtJ1sD2l856C2pCtMkV77itEcKgElSuulonQJDG-8718UmnxpU1sIchvEo64ziimtaJVudADNEsFBkbsWF1JRT5HsZKHD3byT0x-bcvAb9JaFtQF9AwziHdBv9SfhKWnfyaoYfmnh_5bvIE51WZJDO5n3q6ecoA_ykcvEY"
        />
        <div
            class="absolute inset-0 bg-gradient-to-r from-secondary to-transparent lg:via-secondary/50"
        ></div>
    </div>
    <div class="custom-shape-divider-bottom">
        <svg
            data-name="Layer 1"
            preserveAspectRatio="none"
            viewBox="0 0 1200 120"
            xmlns="http://www.w3.org/2000/svg"
        >
            <path
                class="shape-fill"
                d="M321.39,56.44c58-10.79,114.16-30.13,172-41.86,82.39-16.72,168.19-17.73,250.45-.39C823.78,31,906.67,72,985.66,92.83c70.05,18.48,146.53,26.09,214.34,3V0H0V27.35A600.21,600.21,0,0,0,321.39,56.44Z"
            ></path>
        </svg>
    </div>
</div>
<div class="relative -mt-16 z-20 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 mb-20">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div
            class="bg-white dark:bg-surface-dark rounded-xl shadow-xl p-8 transform hover:-translate-y-2 transition duration-300 border-t-4 border-primary"
        >
            <div
                class="w-12 h-12 bg-orange-100 dark:bg-orange-900/30 rounded-lg flex items-center justify-center mb-4 text-primary"
            >
                <span class="material-icons">calendar_today</span>
            </div>
            <h3 class="text-xl font-bold text-gray-900 dark:text-white mb-2">
                Agendamento Online
            </h3>
            <p class="text-gray-600 dark:text-gray-400 text-sm">
                Reserve seu horário em segundos, 24 horas por dia, 7 dias por
                semana.
            </p>
        </div>
        <div
            class="bg-white dark:bg-surface-dark rounded-xl shadow-xl p-8 transform hover:-translate-y-2 transition duration-300 border-t-4 border-primary"
        >
            <div
                class="w-12 h-12 bg-orange-100 dark:bg-orange-900/30 rounded-lg flex items-center justify-center mb-4 text-primary"
            >
                <span class="material-icons">card_giftcard</span>
            </div>
            <h3 class="text-xl font-bold text-gray-900 dark:text-white mb-2">
                Pacotes Especiais
            </h3>
            <p class="text-gray-600 dark:text-gray-400 text-sm">
                Combos exclusivos de manicure e pedicure com descontos
                incríveis.
            </p>
        </div>
        <div
            class="bg-white dark:bg-surface-dark rounded-xl shadow-xl p-8 transform hover:-translate-y-2 transition duration-300 border-t-4 border-primary"
        >
            <div
                class="w-12 h-12 bg-orange-100 dark:bg-orange-900/30 rounded-lg flex items-center justify-center mb-4 text-primary"
            >
                <span class="material-icons">star</span>
            </div>
            <h3 class="text-xl font-bold text-gray-900 dark:text-white mb-2">
                Profissionais Top
            </h3>
            <p class="text-gray-600 dark:text-gray-400 text-sm">
                Equipe treinada e especializada nas últimas tendências de
                beleza.
            </p>
        </div>
    </div>
</div>
<section class="py-16 bg-gray-50 dark:bg-gray-900" id="services">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
            <h2
                class="text-base text-primary font-semibold tracking-wide uppercase"
            >
                O que oferecemos
            </h2>
            <p
                class="mt-2 text-3xl leading-8 font-extrabold tracking-tight text-gray-900 dark:text-white sm:text-4xl"
            >
                Nossos Serviços Premium
            </p>
            <p
                class="mt-4 max-w-2xl text-xl text-gray-500 dark:text-gray-400 mx-auto"
            >
                Cuidamos de cada detalhe para que você se sinta renovada.
            </p>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <div
                class="group relative overflow-hidden rounded-2xl shadow-lg bg-white dark:bg-surface-dark"
            >
                <div class="h-48 overflow-hidden">
                    <img
                        alt="Manicure"
                        class="w-full h-full object-cover transform group-hover:scale-110 transition duration-500"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuBXdlqBvfq4Jf6y1DSxSqBTqJmiKe7omeapwkJ6L_NcTza7eZkd6iqP02rTQTin8XnbiGdO36Z-IDB_kl5LqEOVQfiEkIB2LJRZzcZ4YViJgmdUZXdvOZy9ZXfvD7I3pO_uGTHXoQ_0mtWLpVhZzliykp22Ga6hvLkFilUTWWkxgDlQSzkfRZmZQVMOMs5F0e5ERzKW9HmUrrw4j-rzjhNbrP2IwxBAw4jmfAfIq6V4sNb1LiGFdpx4GNRQfq-s8sCGDzQQcm21MjY"
                    />
                </div>
                <div class="p-6">
                    <div class="flex justify-between items-center mb-4">
                        <span
                            class="px-3 py-1 text-xs font-semibold text-primary bg-orange-100 dark:bg-orange-900/20 rounded-full"
                            >Popular</span
                        >
                        <span class="text-gray-500 dark:text-gray-400 text-sm"
                            >45 min</span
                        >
                    </div>
                    <h3
                        class="text-xl font-bold text-gray-900 dark:text-white mb-2"
                    >
                        Manicure &amp; Pedicure
                    </h3>
                    <p class="text-gray-600 dark:text-gray-400 text-sm mb-4">
                        Tratamento completo para mãos e pés, com esmaltação em
                        gel ou tradicional.
                    </p>
                    <a
                        class="text-primary font-medium hover:text-orange-600 flex items-center"
                        href="/login"
                    >
                        Agendar <span class="material-icons text-sm ml-1"
                            >arrow_forward</span
                        >
                    </a>
                </div>
            </div>
            <div
                class="group relative overflow-hidden rounded-2xl shadow-lg bg-white dark:bg-surface-dark"
            >
                <div class="h-48 overflow-hidden">
                    <img
                        alt="Cabelo"
                        class="w-full h-full object-cover transform group-hover:scale-110 transition duration-500"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuBZabpb8TkADGbEeQkkl5sxJoIKvHyqhwVp4_njUXPo3Vi5parHQThnmlWBG6WeGQ4tPIN6RmojZf_s8VFqrti0mdoyJNKDM-k5AeqoDHcu0Dz1N4JI0OShxrPpnmmIp-ddLU8-XukHeRBrqTXilnGRVykqAuMO-lIggpzJVks7wZsAXf5TwX52_IekR0aoW6_OmDIb9ue31lOeLCTPx1iF63PDRykOyZu5N_11wyO1KUe0aHwrs2-1WSAYrhf1UnLXVrq_YMsKF1o"
                    />
                </div>
                <div class="p-6">
                    <div class="flex justify-between items-center mb-4">
                        <span
                            class="px-3 py-1 text-xs font-semibold text-green-600 bg-green-100 dark:bg-green-900/20 rounded-full"
                            >Novidade</span
                        >
                        <span class="text-gray-500 dark:text-gray-400 text-sm"
                            >60 min</span
                        >
                    </div>
                    <h3
                        class="text-xl font-bold text-gray-900 dark:text-white mb-2"
                    >
                        Corte &amp; Tratamento Capilar
                    </h3>
                    <p class="text-gray-600 dark:text-gray-400 text-sm mb-4">
                        Renove seu visual com cortes modernos e hidratação
                        profunda.
                    </p>
                    <a
                        class="text-primary font-medium hover:text-orange-600 flex items-center"
                        href="/login"
                    >
                        Agendar <span class="material-icons text-sm ml-1"
                            >arrow_forward</span
                        >
                    </a>
                </div>
            </div>
            <div
                class="group relative overflow-hidden rounded-2xl shadow-lg bg-white dark:bg-surface-dark"
            >
                <div class="h-48 overflow-hidden">
                    <img
                        alt="Maquiagem"
                        class="w-full h-full object-cover transform group-hover:scale-110 transition duration-500"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuB617kbDHuvCTdDGxlozdvx7079w6jmYpClXHwNf9Mm2LssV9P--e9WAh5_iNLbcm8dkSOI7drNmoYOeBRgqluuooOcm7KXkF3O35R3f67REty-lVHOCtwuK5t8buW6ZDAQlysJogvq51RJjMUdGFjNPIdjjWxsl4BJEPZP9aNiwXryENZGbFxJd5tQ1brUATTykWKF2idG6eITpzYF6J84zSW19a_yqBlQ_7zzy8Tt9OELNbS5x53VEKOC4iX02Mj6BpFVXukNaT0"
                    />
                </div>
                <div class="p-6">
                    <div class="flex justify-between items-center mb-4">
                        <span
                            class="px-3 py-1 text-xs font-semibold text-purple-600 bg-purple-100 dark:bg-purple-900/20 rounded-full"
                            >Luxo</span
                        >
                        <span class="text-gray-500 dark:text-gray-400 text-sm"
                            >90 min</span
                        >
                    </div>
                    <h3
                        class="text-xl font-bold text-gray-900 dark:text-white mb-2"
                    >
                        Maquiagem Profissional
                    </h3>
                    <p class="text-gray-600 dark:text-gray-400 text-sm mb-4">
                        Para eventos especiais ou dia a dia, realce seus traços
                        com nossas experts.
                    </p>
                    <a
                        class="text-primary font-medium hover:text-orange-600 flex items-center"
                        href="/login"
                    >
                        Agendar <span class="material-icons text-sm ml-1"
                            >arrow_forward</span
                        >
                    </a>
                </div>
            </div>
            <div
                class="group relative overflow-hidden rounded-2xl shadow-lg bg-white dark:bg-surface-dark"
            >
                <div class="h-48 overflow-hidden">
                    <img
                        alt="Cílios"
                        class="w-full h-full object-cover transform group-hover:scale-110 transition duration-500"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuCTK0saWyM1_4K7LtleZ_4U45Y-MM4guJjfhxrhKVUYbUCzX5QAXOLhpmpk2t4Q9KA6paLez2wxQuLN5eDDWj4RiFNiMJczzJalwnBCaHEa0ADhfSwerhKjmAchJvhGnH9_GfO8_DRK_9ze4rUl3Aa26lERwKJK-OiSqfzNNq5aBAp09ftTr5oRGnrWzaemsNaBXmC4AHOEYzn6hS-q5EKTQR5CiDWpwICSa1Hwuhc5XzTKPox27OVPVeGbyGWDoeMW3uZZD_T0Bqo"
                    />
                </div>
                <div class="p-6">
                    <div class="flex justify-between items-center mb-4">
                        <span></span>
                        <span class="text-gray-500 dark:text-gray-400 text-sm"
                            >120 min</span
                        >
                    </div>
                    <h3
                        class="text-xl font-bold text-gray-900 dark:text-white mb-2"
                    >
                        Extensão de Cílios
                    </h3>
                    <p class="text-gray-600 dark:text-gray-400 text-sm mb-4">
                        Fio a fio, volume russo ou híbrido. Olhar marcante
                        garantido.
                    </p>
                    <a
                        class="text-primary font-medium hover:text-orange-600 flex items-center"
                        href="/login"
                    >
                        Agendar <span class="material-icons text-sm ml-1"
                            >arrow_forward</span
                        >
                    </a>
                </div>
            </div>
            <div
                class="group relative overflow-hidden rounded-2xl shadow-lg bg-white dark:bg-surface-dark"
            >
                <div class="h-48 overflow-hidden">
                    <img
                        alt="Sobrancelha"
                        class="w-full h-full object-cover transform group-hover:scale-110 transition duration-500"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuD8pmsfR9W1dnWlSiWO4Fq4ODCuRJ5NKKF3bHpjk1CL0r_gDbw5Ry-fwRZutLVCOQScAJHsZQtXhaRlQf5nc8KDAhF8O9Q5TZOreFMwDF9jk9wjnXzb32mhEaLrZldwHcBAYzd5vaCarL_mkdWW4I0T70dJQsrF2nQf7041UPwezihWf7UMd0_3eFNxS8P8VBIkHpD0lcQqI0p2qDFwWIxuuxmDQCnOcD0zWxd5qUDNPvoigDlbrmT_SkpkZP4VmjGTX4sOkPDRLUg"
                    />
                </div>
                <div class="p-6">
                    <div class="flex justify-between items-center mb-4">
                        <span></span>
                        <span class="text-gray-500 dark:text-gray-400 text-sm"
                            >30 min</span
                        >
                    </div>
                    <h3
                        class="text-xl font-bold text-gray-900 dark:text-white mb-2"
                    >
                        Design de Sobrancelhas
                    </h3>
                    <p class="text-gray-600 dark:text-gray-400 text-sm mb-4">
                        Micropigmentação e design com henna para harmonizar seu
                        rosto.
                    </p>
                    <a
                        class="text-primary font-medium hover:text-orange-600 flex items-center"
                        href="/login"
                    >
                        Agendar <span class="material-icons text-sm ml-1"
                            >arrow_forward</span
                        >
                    </a>
                </div>
            </div>
            <div
                class="group relative overflow-hidden rounded-2xl shadow-lg bg-primary flex flex-col justify-center items-center text-center p-8"
            >
                <div class="absolute inset-0 bg-black/10"></div>
                <div class="relative z-10">
                    <h3 class="text-2xl font-bold text-white mb-4">
                        Não encontrou o que procura?
                    </h3>
                    <p class="text-white/90 mb-6">
                        Temos diversos outros serviços personalizados para você.
                    </p>
                    <a
                        class="inline-block bg-white text-primary font-bold py-3 px-8 rounded-full shadow hover:bg-gray-100 transition"
                        href="/listagem">Ver Menu Completo</a
                    >
                </div>
            </div>
        </div>
    </div>
</section>
<section
    class="py-16 bg-white dark:bg-background-dark overflow-hidden"
    id="testimonials"
>
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="lg:text-center mb-16">
            <h2
                class="text-base text-primary font-semibold tracking-wide uppercase"
            >
                Depoimentos
            </h2>
            <p
                class="mt-2 text-3xl leading-8 font-extrabold tracking-tight text-gray-900 dark:text-white sm:text-4xl"
            >
                O que dizem nossas clientes
            </p>
        </div>
        <div class="flex flex-wrap justify-center gap-8">
            <div
                class="max-w-md w-full bg-gray-50 dark:bg-surface-dark p-8 rounded-2xl relative shadow-sm"
            >
                <span
                    class="material-icons text-4xl text-primary/20 absolute top-4 left-4"
                    >format_quote</span
                >
                <p
                    class="text-gray-600 dark:text-gray-300 italic mb-6 relative z-10"
                >
                    "O melhor salão da cidade! Fiz minhas sobrancelhas e amei o
                    resultado. O ambiente é super aconchegante e as
                    profissionais muito atenciosas."
                </p>
                <div class="flex items-center">
                    <img
                        alt="Foto de perfil de Ana Souza"
                        class="h-10 w-10 rounded-full object-cover"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuCexJEPaNVmFdD_2cySMXqd71FG5pOcZzr5G0PjwBU0MXWumpLu_TFy5RlV9xnGP3MAQFaBdUTZEpCsMX-U-lKVK8AoqenQiSQsatZimI6f56Z4UtcjVYrM0jWYgyhiYrIy_xT3y9TkGYkXd-y7Zb7Y4pfwRF8kLFSixxzNv6rfgtVBUC1n3Pe3rq7eKrilvn2YW5pgLNveRj2kiIDtfecX-ouE6Oa7UqEgjQtQlnjeCqN0fbnWiqbMRKClnFKlwtRvbGjWC42yNd8"
                    />
                    <div class="ml-3">
                        <p
                            class="text-sm font-medium text-gray-900 dark:text-white"
                        >
                            Ana Souza
                        </p>
                        <div class="flex text-yellow-400 text-xs">
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                        </div>
                    </div>
                </div>
            </div>
            <div
                class="max-w-md w-full bg-gray-50 dark:bg-surface-dark p-8 rounded-2xl relative shadow-sm"
            >
                <span
                    class="material-icons text-4xl text-primary/20 absolute top-4 left-4"
                    >format_quote</span
                >
                <p
                    class="text-gray-600 dark:text-gray-300 italic mb-6 relative z-10"
                >
                    "Fiz uma hidratação capilar que salvou meu cabelo. Recomendo
                    muito o pacote de tratamento mensal. Preço justo e qualidade
                    excelente."
                </p>
                <div class="flex items-center">
                    <img
                        alt="Foto de perfil de Carla Dias"
                        class="h-10 w-10 rounded-full object-cover"
                        src="https://lh3.googleusercontent.com/aida-public/AB6AXuCojTLPv-ENJ1kPj3idE4m4GdISNAATV6tFbmtFGuZmLa1PXVVTrlXybE4PvC9Oz7S272x-s8plVMwu-d7KQsMo-zyQLvzPqBLjV3ebghhjBMu61njW5_Ibkki5ADyJZ_zR10-stmaPkJr3Z6nxrV-OA3Ik10Cs4UG180CQVa9pUfHdad2KzbJ86eRVgRnlpCm1BcDAnRSA8o_Nrz5-fPOraee1Y3_Ia4sbt_IBNw2XBP9hgWNdY2K0s7pGj23xsbFe_xQ3swM5_Cg"
                    />
                    <div class="ml-3">
                        <p
                            class="text-sm font-medium text-gray-900 dark:text-white"
                        >
                            Carla Dias
                        </p>
                        <div class="flex text-yellow-400 text-xs">
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]">star</span>
                            <span class="material-icons text-[16px]"
                                >star_half</span
                            >
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</section>
<div class="relative bg-secondary py-16 px-4 overflow-hidden">
    <div class="absolute inset-0 opacity-10">
        <svg
            class="h-full w-full"
            preserveAspectRatio="none"
            viewBox="0 0 100 100"
        >
            <path d="M0 100 C 20 0 50 0 100 100 Z" fill="white"></path>
        </svg>
    </div>
    <div class="relative max-w-4xl mx-auto text-center">
        <span class="block text-primary text-lg font-bold mb-2"
            >Oferta Exclusiva</span
        >
        <h2 class="text-3xl md:text-5xl font-extrabold text-white mb-6">
            Ganhe 20% OFF na sua primeira visita!
        </h2>
        <p class="text-gray-300 text-lg mb-8 max-w-2xl mx-auto">
            Cadastre-se agora e receba um cupom exclusivo para usar em qualquer
            serviço do salão. Válido por 30 dias.
        </p>
        <form class="max-w-md mx-auto flex flex-col sm:flex-row gap-3">
            <input
                class="flex-1 rounded-full border-none px-6 py-4 focus:ring-2 focus:ring-primary text-gray-900 placeholder-gray-500 shadow-sm"
                placeholder="Seu melhor e-mail"
                type="email"
            />
            <button
                class="bg-primary text-white font-bold py-4 px-8 rounded-full shadow-lg hover:bg-orange-600 transition transform hover:-translate-y-1"
                type="button"
            >
                Quero o Desconto
            </button>
        </form>
        <p class="text-xs text-gray-500 mt-4">
            *Ao se cadastrar você concorda com nossos termos.
        </p>
    </div>
</div>

<Footer />

<a
    class="md:hidden fixed bottom-6 right-6 bg-primary text-white p-4 rounded-full shadow-2xl z-50 hover:bg-orange-600 transition flex items-center justify-center"
    href="/login"
>
    <span class="material-icons">calendar_today</span>
</a>

<style>
    .custom-shape-divider-bottom {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        overflow: hidden;
        line-height: 0;
        transform: rotate(180deg);
    }
    .custom-shape-divider-bottom svg {
        position: relative;
        display: block;
        width: calc(100% + 1.3px);
        height: 50px;
    }
    .custom-shape-divider-bottom .shape-fill {
        fill: #ffffff;
    }
    :global(.dark) .custom-shape-divider-bottom .shape-fill {
        fill: #111827;
    }
</style>
