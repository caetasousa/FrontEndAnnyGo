<script lang="ts">
    import ThemeToggle from "$lib/components/ThemeToggle.svelte";
</script>

<!-- Dashboard Navbar - NO SEARCH BAR -->
<header
    class="h-16 bg-[hsl(var(--bs-card))] border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10 flex-shrink-0"
>
    <div class="flex items-center flex-1">
        <h2 class="text-xl font-semibold text-gray-900 dark:text-white">
            Dashboard
        </h2>
    </div>
    <div class="flex items-center space-x-4">
        <button
            class="p-2 rounded-full text-gray-400 hover:text-gray-500 dark:hover:text-gray-300 focus:outline-none transition-colors"
        >
            <span class="material-icons">notifications</span>
        </button>
        <ThemeToggle />
    </div>
</header>
