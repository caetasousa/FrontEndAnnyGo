<script lang="ts">
    import { page } from "$app/stores";

    interface NavItem {
        label: string;
        href: string;
        icon: string;
        category?: string;
    }

    export let items: NavItem[] = [
        { label: "Início", href: "/", icon: "home" },
        {
            label: "Agenda do Prestador",
            href: "/prestadores/agenda",
            icon: "calendar_month",
        },
        {
            label: "Listagem de Prestadores",
            href: "/prestadores/editar",
            icon: "group",
            category: "Prestadores",
        },
        {
            label: "Cadastrar Prestador",
            href: "/prestadores/cadastro",
            icon: "person_add",
        },
        {
            label: "Listagem de Serviços",
            href: "/catalogo/listagem",
            icon: "list_alt",
            category: "Serviços",
        },
        { label: "Cadastrar Serviço", href: "/catalogo", icon: "add_box" },
    ];

    // Helper to check if a link is active
    $: isActive = (href: string) => {
        const path = $page.url.pathname;
        if (href === "/") return path === "/";

        // Exact match
        if (path === href) return true;

        // Subpath match (e.g., viewing a profile under /prestadores)
        // But only if there isn't a more specific nav item in the list
        if (path.startsWith(href + "/")) {
            const hasMoreSpecificMatch = items.some(
                (item) =>
                    item.href !== href &&
                    path.startsWith(item.href) &&
                    item.href.length > href.length,
            );
            return !hasMoreSpecificMatch;
        }

        return false;
    };
</script>

<aside
    class="w-64 bg-[hsl(var(--bs-card))] flex-col hidden md:flex h-full flex-shrink-0"
>
    <div class="h-16 flex items-center px-6">
        <span class="material-icons text-brand-orange text-3xl mr-2">spa</span>
        <span class="font-bold text-xl tracking-tight text-brand-orange"
            >BellaVita</span
        >
    </div>

    <nav class="flex-1 overflow-y-auto py-4 px-3 space-y-1">
        {#each items as item}
            {#if item.category}
                <div
                    class="pt-4 pb-2 px-3 text-xs font-semibold text-gray-500 uppercase tracking-wider"
                >
                    {item.category}
                </div>
            {/if}
            <a
                class="group flex items-center px-3 py-2 text-sm font-medium rounded-md transition-all duration-200 {isActive(
                    item.href,
                )
                    ? 'bg-brand-orange/10 text-brand-orange'
                    : 'text-gray-700 dark:text-gray-300 hover:bg-brand-orange/10 hover:text-brand-orange'}"
                href={item.href}
            >
                <span
                    class="material-symbols-outlined mr-3 {isActive(item.href)
                        ? 'text-brand-orange'
                        : 'text-gray-400 group-hover:text-brand-orange transition-colors'}"
                >
                    {item.icon}
                </span>
                {item.label}
            </a>
        {/each}
    </nav>

    <div
        class="border-t border-border-light dark:border-border-dark p-4 space-y-1"
    >
        <h3
            class="px-3 text-xs font-semibold text-gray-500 uppercase tracking-wider mb-2"
        >
            Conta
        </h3>
        <a
            class="group flex items-center px-3 py-2 text-sm font-medium rounded-md transition-all duration-200 {isActive(
                '/meus-dados',
            )
                ? 'bg-brand-orange/10 text-brand-orange'
                : 'text-gray-700 dark:text-gray-300 hover:bg-brand-orange/10 hover:text-brand-orange'}"
            href="/meus-dados"
        >
            <span
                class="material-symbols-outlined mr-3 {isActive('/meus-dados')
                    ? 'text-brand-orange'
                    : 'text-gray-400 group-hover:text-brand-orange transition-colors'}"
                >person</span
            >
            Meus Dados
        </a>
        <a
            class="group flex items-center px-3 py-2 text-sm font-medium rounded-md transition-all duration-200 {isActive(
                '/perfil',
            )
                ? 'bg-brand-orange/10 text-brand-orange'
                : 'text-gray-700 dark:text-gray-300 hover:bg-brand-orange/10 hover:text-brand-orange'}"
            href="/perfil"
        >
            <span
                class="material-symbols-outlined mr-3 {isActive('/perfil')
                    ? 'text-brand-orange'
                    : 'text-gray-400 group-hover:text-brand-orange transition-colors'}"
                >settings</span
            >
            Configurações
        </a>
    </div>

    <div class="border-t border-border-light dark:border-border-dark p-4">
        <div class="flex items-center">
            <img
                alt="User avatar"
                class="h-8 w-8 rounded-full object-cover"
                src="https://lh3.googleusercontent.com/aida-public/AB6AXuBb0ATcXG4FyKxC1twW6jW1I3PLqBWNywkqLLdNpJ4x01o_EjHRWFY69U_fzxAa2pSXWVWHjFYGBYs34htCvjGS2fz_k_hAX2eGfmgYOb6GfalpvTABqMextdJ2-EFSN-vph8xY2l31Bz-0d4r5jlelA6Dogcle1FRJXYR6PJ9hIT22fwfdjLphhZvByuIIkVULiwZzXITJYwFF1zIyMBG-KPYgE022HBqLcyuU-jIHNafhWecl7NofaMfp_E_HIxlFcgEecoHAQbI"
            />
            <div class="ml-3">
                <p class="text-sm font-medium text-gray-700 dark:text-gray-200">
                    Maria Silva
                </p>
                <p class="text-xs text-gray-500 dark:text-gray-400">Admin</p>
            </div>
        </div>
    </div>
</aside>
