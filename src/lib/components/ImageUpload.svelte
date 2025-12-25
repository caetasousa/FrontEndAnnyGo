<script lang="ts">
    import { createEventDispatcher } from "svelte";
    import {
        validateImageFile,
        createImagePreview,
    } from "$lib/utils/imageUpload";

    /** Dispatcher para eventos do componente */
    const dispatch = createEventDispatcher<{
        select: { file: File; preview: string };
        clear: void;
    }>();

    /** URL de preview da imagem (passada pelo componente pai) */
    export let previewUrl: string | null = null;

    /** Mensagem de erro (gerenciada internamente) */
    let error: string = "";

    /**
     * Manipula a seleção de arquivo de imagem
     */
    async function handleFileSelect(event: Event) {
        const input = event.target as HTMLInputElement;
        const file = input.files?.[0];

        if (!file) return;

        // Valida o arquivo
        const validation = validateImageFile(file);
        if (!validation.valid) {
            error = validation.error || "";
            return;
        }

        // Limpa erro e cria preview
        error = "";

        try {
            const preview = await createImagePreview(file);
            dispatch("select", { file, preview });
        } catch (err) {
            error = "Erro ao criar preview da imagem.";
            console.error(err);
        }
    }

    /**
     * Limpa a imagem selecionada
     */
    function handleClear() {
        dispatch("clear");
    }
</script>

{#if previewUrl}
    <!-- Preview da Imagem -->
    <!-- Preview da Imagem -->
    <div class="relative group">
        <img
            src={previewUrl}
            alt="Preview da imagem"
            class="w-full h-48 object-cover rounded-lg"
        />

        <!-- Overlay com botão de alterar -->
        <div
            class="absolute inset-0 bg-black/40 opacity-0 group-hover:opacity-100 transition-opacity duration-200 flex items-center justify-center rounded-lg"
        >
            <label
                for="image-change"
                class="cursor-pointer px-4 py-2 bg-white/20 backdrop-blur-sm border border-white/50 text-white rounded-md hover:bg-white/30 transition-colors font-medium flex items-center"
            >
                <span class="material-icons text-sm mr-2">edit</span>
                Alterar Imagem
                <input
                    id="image-change"
                    type="file"
                    accept="image/*"
                    class="sr-only"
                    on:change={handleFileSelect}
                />
            </label>
        </div>

        <button
            type="button"
            on:click={handleClear}
            class="absolute top-2 right-2 h-8 w-8 flex items-center justify-center bg-red-500 hover:bg-red-600 text-white rounded-full transition-colors shadow-sm z-10"
            title="Remover imagem"
        >
            <span class="material-icons text-lg">close</span>
        </button>
    </div>

    <div
        class="mt-3 p-3 bg-blue-50 dark:bg-blue-900/20 border border-blue-200 dark:border-blue-800 rounded-md"
    >
        <div class="flex items-center text-blue-700 dark:text-blue-400">
            <span class="material-icons text-sm mr-2">info</span>
            <span class="text-sm"
                >A imagem será enviada ao salvar o serviço</span
            >
        </div>
    </div>
{:else}
    <!-- Área de Upload -->
    <label
        for="image-upload"
        class="mt-2 flex justify-center rounded-lg border border-dashed border-gray-300 dark:border-gray-600 px-6 py-10 hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors cursor-pointer group"
    >
        <div class="text-center">
            <span
                class="material-icons mx-auto h-12 w-12 text-gray-300 dark:text-gray-500 group-hover:text-brand-orange transition-colors text-5xl"
                >image</span
            >
            <div
                class="mt-4 flex text-sm leading-6 text-gray-600 dark:text-gray-400 justify-center"
            >
                <span
                    class="relative cursor-pointer rounded-md font-semibold text-brand-orange hover:text-brand-orange-hover focus-within:outline-none focus-within:ring-2 focus-within:ring-brand-orange focus-within:ring-offset-2"
                >
                    <span>Enviar arquivo</span>
                    <input
                        id="image-upload"
                        type="file"
                        accept="image/*"
                        class="sr-only"
                        on:change={handleFileSelect}
                    />
                </span>
            </div>
            <p class="text-xs leading-5 text-gray-500 dark:text-gray-500">
                PNG, JPG até 5MB
            </p>
        </div>
    </label>
{/if}

{#if error}
    <div
        class="mt-3 p-3 bg-red-50 dark:bg-red-900/20 border border-red-200 dark:border-red-800 rounded-md"
    >
        <p class="text-sm text-red-700 dark:text-red-400">{error}</p>
    </div>
{/if}
