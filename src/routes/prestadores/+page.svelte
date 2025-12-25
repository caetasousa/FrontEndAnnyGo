<script lang="ts">
  import Sidebar from "$lib/components/Sidebar.svelte";
  import ThemeToggle from "$lib/components/ThemeToggle.svelte";
  import { onMount } from "svelte";

  interface Provider {
    id: number;
    nome: string;
    especialidade: string;
    foto: string;
    rating: number;
    reviews: number;
    status: "disponivel" | "ocupado" | "ausente";
    biografia: string;
    ativo: boolean;
  }

  let providers: Provider[] = [
    {
      id: 1,
      nome: "Ana Maria Costa",
      especialidade: "Cabeleireira Especialista",
      foto: "https://images.unsplash.com/photo-1544005313-94ddf0286df2?auto=format&fit=crop&q=80&w=200",
      rating: 4.9,
      reviews: 128,
      status: "disponivel",
      biografia: "Especialista em cortes modernos e coloração.",
      ativo: true
    },
    {
      id: 2,
      nome: "Ricardo Santos",
      especialidade: "Barbeiro & Visagista",
      foto: "https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?auto=format&fit=crop&q=80&w=200",
      rating: 4.8,
      reviews: 95,
      status: "disponivel",
      biografia: "Mestre em barbas clássicas e degradês modernos.",
      ativo: true
    },
    {
      id: 3,
      nome: "Juliana Silva",
      especialidade: "Manicure & Nail Art",
      foto: "https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&q=80&w=200",
      rating: 5.0,
      reviews: 210,
      status: "ocupado",
      biografia: "Apaixonada por detalhes e tendências em unhas.",
      ativo: true
    },
    {
      id: 4,
      nome: "Marcos Oliveira",
      especialidade: "Massoterapeuta",
      foto: "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?auto=format&fit=crop&q=80&w=200",
      rating: 4.7,
      reviews: 64,
      status: "disponivel",
      biografia: "Especialista em massagens relaxantes e drenagem.",
      ativo: false
    },
    {
      id: 5,
      nome: "Beatriz Mello",
      especialidade: "Esteticista Facial",
      foto: "https://images.unsplash.com/photo-1554151228-14d9def656e4?auto=format&fit=crop&q=80&w=200",
      rating: 4.9,
      reviews: 87,
      status: "ausente",
      biografia: "Cuidando da saúde e beleza da sua pele.",
      ativo: true
    },
    {
      id: 6,
      nome: "Fernando Lima",
      especialidade: "Colorista",
      foto: "https://images.unsplash.com/photo-1492562080023-ab3db95bfbce?auto=format&fit=crop&q=80&w=200",
      rating: 4.6,
      reviews: 42,
      status: "disponivel",
      biografia: "Técnicas avançadas de mechas e balayage.",
      ativo: false
    }
  ];

  let searchQuery = "";
  let statusFilter: "todos" | "ativos" | "inativos" = "todos";

  $: filteredProviders = providers.filter(p => {
    const matchesSearch = p.nome.toLowerCase().includes(searchQuery.toLowerCase()) ||
                         p.especialidade.toLowerCase().includes(searchQuery.toLowerCase());
    
    const matchesStatus = statusFilter === "todos" || 
                         (statusFilter === "ativos" && p.ativo) ||
                         (statusFilter === "inativos" && !p.ativo);
    
    return matchesSearch && matchesStatus;
  });
</script>

<div class="font-body bg-background-light dark:bg-background-dark text-text-light dark:text-text-dark antialiased h-screen flex overflow-hidden transition-colors duration-200">
  <Sidebar />

  <main class="flex-1 flex flex-col h-full overflow-hidden relative">
    <header class="h-16 bg-surface-light dark:bg-surface-dark border-b border-border-light dark:border-border-dark flex items-center justify-between px-6 z-10">
      <div class="flex items-center flex-1 max-w-2xl">
        <div class="relative w-full">
          <span class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
            <span class="material-symbols-outlined text-gray-400">search</span>
          </span>
          <input 
            type="text" 
            bind:value={searchQuery}
            class="block w-full pl-10 pr-3 py-2 border border-border-light dark:border-border-dark rounded-md leading-5 bg-gray-50 dark:bg-gray-800 text-gray-900 dark:text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-1 focus:ring-primary focus:border-primary sm:text-sm transition-all" 
            placeholder="Buscar por nome ou especialidade..."
          />
        </div>
      </div>
      <div class="flex items-center space-x-4 ml-4">
        <button class="p-2 rounded-full text-gray-400 hover:text-gray-500 dark:hover:text-gray-300 focus:outline-none transition-colors">
          <span class="material-symbols-outlined">notifications</span>
        </button>
        <ThemeToggle />
      </div>
    </header>

    <div class="flex-1 overflow-y-auto p-6 md:p-8">
      <div class="max-w-6xl mx-auto space-y-8">
        <div>
          <nav class="flex text-sm text-gray-500 dark:text-gray-400 mb-2">
            <a href="/" class="hover:text-primary transition-colors">Início</a>
            <span class="mx-2">/</span>
            <span class="text-gray-900 dark:text-white font-medium">Equipe de Profissionais</span>
          </nav>
          <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
            <div>
              <h1 class="text-3xl font-bold text-gray-900 dark:text-white">Nossos Profissionais</h1>
              <p class="text-gray-500 dark:text-gray-400 mt-1">Conheça os especialistas prontos para cuidar de você.</p>
            </div>
            
            <div class="flex flex-col sm:flex-row items-center gap-3">
              <div class="flex items-center space-x-1 bg-gray-100 dark:bg-gray-800 p-1 rounded-xl">
                <button 
                  on:click={() => statusFilter = 'todos'}
                  class="px-4 py-1.5 text-sm font-semibold rounded-lg transition-all {statusFilter === 'todos' ? 'bg-white dark:bg-gray-700 shadow-md text-brand-orange' : 'text-gray-500 hover:text-gray-700 dark:hover:text-gray-300'}"
                >
                  Todos
                </button>
                <button 
                  on:click={() => statusFilter = 'ativos'}
                  class="px-4 py-1.5 text-sm font-semibold rounded-lg transition-all {statusFilter === 'ativos' ? 'bg-white dark:bg-gray-700 shadow-md text-brand-orange' : 'text-gray-500 hover:text-gray-700 dark:hover:text-gray-300'}"
                >
                  Ativos
                </button>
                <button 
                  on:click={() => statusFilter = 'inativos'}
                  class="px-4 py-1.5 text-sm font-semibold rounded-lg transition-all {statusFilter === 'inativos' ? 'bg-white dark:bg-gray-700 shadow-md text-brand-orange' : 'text-gray-500 hover:text-gray-700 dark:hover:text-gray-300'}"
                >
                  Inativos
                </button>
              </div>
              
              <a href="/prestadores/cadastro" class="w-full sm:w-auto px-6 py-2.5 bg-brand-orange hover:bg-brand-orange-hover text-white rounded-xl font-bold shadow-lg shadow-brand-orange/20 transition-all active:scale-95 flex items-center justify-center">
                <span class="material-symbols-outlined text-xl mr-2">person_add</span>
                Novo Prestador
              </a>
            </div>
          </div>
        </div>

        {#if filteredProviders.length === 0}
          <div class="bg-surface-light dark:bg-surface-dark rounded-xl border-2 border-dashed border-border-light dark:border-border-dark p-12 text-center">
            <span class="material-symbols-outlined text-5xl text-gray-300 mb-4">person_search</span>
            <p class="text-gray-500">Nenhum profissional encontrado para "{searchQuery}" {statusFilter !== 'todos' ? `(${statusFilter})` : ''}</p>
          </div>
        {:else}
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            {#each filteredProviders as provider}
              <div class="bg-surface-light dark:bg-surface-dark rounded-xl border border-border-light dark:border-border-dark overflow-hidden hover:shadow-xl transition-all duration-300 group flex flex-col">
                <div class="p-6 flex flex-col items-center text-center">
                  <div class="relative mb-4">
                    <img src={provider.foto} alt={provider.nome} class="h-24 w-24 rounded-full object-cover border-4 border-white dark:border-gray-700 shadow-sm" />
                    <span class="absolute bottom-1 right-1 w-5 h-5 rounded-full border-2 border-white dark:border-gray-800 {provider.status === 'disponivel' ? 'bg-green-500' : provider.status === 'ocupado' ? 'bg-amber-500' : 'bg-gray-400'}" title={provider.status}></span>
                  </div>
                  <h3 class="text-lg font-bold text-gray-900 dark:text-white group-hover:text-brand-orange transition-colors">{provider.nome}</h3>
                  <p class="text-sm text-brand-orange font-medium mt-1">{provider.especialidade}</p>
                  
                  <div class="flex items-center mt-3 text-sm text-amber-500">
                    <span class="material-icons text-sm mr-1">star</span>
                    <span class="font-bold">{provider.rating}</span>
                    <span class="text-gray-400 ml-1">({provider.reviews} avaliações)</span>
                  </div>

                  <p class="text-sm text-gray-500 dark:text-gray-400 mt-4 line-clamp-2 italic">
                    "{provider.biografia}"
                  </p>
                </div>
                
                <div class="mt-auto px-6 py-4 bg-gray-50 dark:bg-gray-800/50 border-t border-border-light dark:border-border-dark flex items-center justify-between">
                  <div class="flex items-center gap-2">
                    <span class="flex h-2 w-2 rounded-full {provider.ativo ? 'bg-green-500' : 'bg-red-500'}"></span>
                    <span class="text-xs text-gray-400 uppercase tracking-wider">{provider.ativo ? 'Ativo' : 'Inativo'} • {provider.status}</span>
                  </div>
                  <a href="/prestadores/{provider.id}" class="px-4 py-2 bg-brand-orange/10 text-brand-orange hover:bg-brand-orange hover:text-white rounded-md text-sm font-semibold transition-all">
                    Ver Perfil
                  </a>
                </div>
              </div>
            {/each}
          </div>
        {/if}

        <footer class="mt-12 text-center text-sm text-gray-500 dark:text-gray-400 pb-6">
          <p>© 2023 BellaVita Salon. Todos os direitos reservados.</p>
        </footer>
      </div>
    </div>
  </main>
</div>
