<script lang="ts">
    import { onMount } from "svelte";
    import ThemeToggle from "./ThemeToggle.svelte";

    let isMobileMenuOpen = false;

    function toggleMobileMenu() {
        isMobileMenuOpen = !isMobileMenuOpen;
    }
</script>

<nav
    class="sticky top-0 z-50 bg-white/90 dark:bg-gray-900/90 backdrop-blur shadow-sm border-b border-gray-200 dark:border-gray-800"
>
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-20">
            <!-- Logo -->
            <div class="flex items-center space-x-3">
                <div class="text-primary">
                    <span class="material-icons text-3xl">spa</span>
                </div>
<span class="font-bold text-xl tracking-tight">BellaVita</span>
            </div>

            <!-- Desktop Menu -->
            <div class="hidden md:flex space-x-8 items-center">
                <a
                    class="text-gray-600 dark:text-gray-300 hover:text-primary dark:hover:text-primary font-medium transition"
                    href="/">Início</a
                >
                <a
                    class="text-gray-600 dark:text-gray-300 hover:text-primary dark:hover:text-primary font-medium transition"
                    href="/#services">Serviços</a
                >
                <a
                    class="text-gray-600 dark:text-gray-300 hover:text-primary dark:hover:text-primary font-medium transition"
                    href="/#specialists">Especialistas</a
                >
                <a
                    class="text-gray-600 dark:text-gray-300 hover:text-primary dark:hover:text-primary font-medium transition"
                    href="/#testimonials">Depoimentos</a
                >
            </div>

            <!-- Actions -->
            <div class="flex items-center space-x-4">
                <ThemeToggle />
                <a
                    class="hidden md:inline-flex items-center justify-center px-6 py-2.5 border border-transparent text-sm font-semibold rounded-full shadow-sm text-white bg-primary hover:bg-orange-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary transition-all transform hover:scale-105"
                    href="/login"
                >
                    Agendar Agora
                </a>

                <!-- Mobile Menu Button -->
                <button
                    on:click={toggleMobileMenu}
                    class="md:hidden text-gray-500 hover:text-primary p-2"
                >
                    <span class="material-icons"
                        >{isMobileMenuOpen ? "close" : "menu"}</span
                    >
                </button>
            </div>
        </div>
    </div>

    <!-- Mobile Menu -->
    {#if isMobileMenuOpen}
        <div
            class="md:hidden bg-white dark:bg-gray-900 border-t border-gray-200 dark:border-gray-800"
        >
            <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3">
                <a
                    class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 dark:text-gray-200 hover:text-primary hover:bg-gray-50 dark:hover:bg-gray-800"
                    href="/">Início</a
                >
                <a
                    class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 dark:text-gray-200 hover:text-primary hover:bg-gray-50 dark:hover:bg-gray-800"
                    href="/#services">Serviços</a
                >
                <a
                    class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 dark:text-gray-200 hover:text-primary hover:bg-gray-50 dark:hover:bg-gray-800"
                    href="/#specialists">Especialistas</a
                >
                <a
                    class="block px-3 py-2 rounded-md text-base font-medium text-gray-700 dark:text-gray-200 hover:text-primary hover:bg-gray-50 dark:hover:bg-gray-800"
                    href="/#testimonials">Depoimentos</a
                >
                <a
                    class="block w-full text-center mt-4 px-6 py-3 border border-transparent text-base font-semibold rounded-full shadow-sm text-white bg-primary hover:bg-orange-600"
                    href="/#book"
                >
                    Agendar Agora
                </a>
            </div>
        </div>
    {/if}
</nav>
