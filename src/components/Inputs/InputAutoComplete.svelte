<script lang='ts'>
    import AutoComplete from 'simple-svelte-autocomplete';

    export let name: string = '';
    export let label: string = '';
    export let classes: string = '';
    export let itemSelecionado;
    export let searchFunction: Function;
    export let getSelecionado: Function;
    export let mensagemErro: string = '';
    export let deveMostrarErro: boolean = false;
</script>

<label class='block text-sm font-medium text-gray-900 dark:text-white' for={name}>{label}</label>
<AutoComplete bind:selectedItem={itemSelecionado}
              class='{classes} bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500'
              className='w-full mb-2'
              delay='300'
              id={name}
              labelFunction='{(item) => `${item.nome}`}'
              minCharactersToSearch='2'
              searchFunction={(value) => searchFunction(value)}
              showLoadingIndicator={true}
              showClear={true}
              noResultsText='Nenhum resultado encontrado'
              valueFunction={getSelecionado}>
    <div slot='loading' let:loadingText>
        <span>Buscando resultados...</span>
    </div>
</AutoComplete>
{#if deveMostrarErro}
    <div {name} class='text-sm text-red-600 dark:text-red-500'>
        {mensagemErro}
    </div>
{/if}
