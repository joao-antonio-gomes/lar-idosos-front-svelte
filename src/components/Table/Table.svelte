<script lang="ts">
    import type { FetchApiData } from '../../interface/FetchApiData';
    import type { TableStructure } from '../../interface/TableStructure';
    import type { TableActions } from '../../interface/TableActions';

    export let fetchApiData: Promise<any> | FetchApiData;
    export let loadingPhrase: string = '';
    export let tableStructure: TableStructure[] = []; //label, value, isOrdinal
    export let tableActions: TableActions[] = []; //label e link
    export let promiseResolved: boolean = false;
    export let handleChangePage: Function;
</script>

<div class="overflow-x-auto shadow-md sm:rounded-lg w-full">
    <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                {#each tableStructure as coluna}
                    <th scope="col" class="px-6 py-3">
                        <div class="flex items-center">
                            {coluna.label}
                            {#if coluna.isOrdinal}
                                <a href="#">
                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="w-3 h-3 ml-1"
                                        aria-hidden="true"
                                        fill="currentColor"
                                        viewBox="0 0 320 512">
                                        <path
                                            d="M27.66 224h264.7c24.6 0 36.89-29.78 19.54-47.12l-132.3-136.8c-5.406-5.406-12.47-8.107-19.53-8.107c-7.055 0-14.09 2.701-19.45 8.107L8.119 176.9C-9.229 194.2 3.055 224 27.66 224zM292.3 288H27.66c-24.6 0-36.89 29.77-19.54 47.12l132.5 136.8C145.9 477.3 152.1 480 160 480c7.053 0 14.12-2.703 19.53-8.109l132.3-136.8C329.2 317.8 316.9 288 292.3 288z" />
                                    </svg>
                                </a>
                            {/if}
                        </div>
                    </th>
                {/each}
                <th scope="col" class="px-6 py-3">
                    <span class="sr-only">Edit</span>
                </th>
            </tr>
        </thead>
        <tbody>
            {#await fetchApiData}
                <tr
                    class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">
                    {loadingPhrase}
                </tr>
            {:then fetchedData}
                {#each fetchedData.data as item}
                    <tr
                        class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">
                        {#each tableStructure as row, index}
                            {#if index === 0}
                                <th
                                    scope="row"
                                    class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                                    {row.callback(item[row.value])}
                                </th>
                            {:else}
                                <td class="px-6 py-4">
                                    {row.callback(item[row.value])}
                                </td>
                            {/if}
                        {/each}
                        <td class="px-6 py-4 text-right flex justify-between lg:w-48">
                            {#each tableActions as action}
                                <button
                                    type="button"
                                    on:click={action.callback(item)}
                                    class="px-2 py-1 mr-2 font-medium text-center text-blue-main bg-white border border-blue-main rounded-lg hover:bg-blue-main hover:text-white focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                                    {action.label}
                                </button>
                            {/each}
                        </td>
                    </tr>
                {/each}
            {:catch error}
                Erro ao carregar dados.
            {/await}
        </tbody>
    </table>
    {#if promiseResolved}
        <nav class="flex items-center justify-between p-4" aria-label="Table navigation">
            <span class="text-sm font-normal text-gray-500 dark:text-gray-400"
                >Showing <span class="font-semibold text-gray-900 dark:text-white">1-10</span> of
                <span class="font-semibold text-gray-900 dark:text-white">{fetchApiData.count}</span></span>
            <ul class="inline-flex items-center -space-x-px">
                <li>
                    <a
                        href="#"
                        on:click={() => handleChangePage(Number(fetchApiData.page) - 1, 10)}
                        class="{fetchApiData.page === 1
                            ? 'bg-gray-200 pointer-events-none'
                            : 'bg-white'} disabled block px-3 py-2 ml-0 leading-tight text-gray-500 border border-gray-300 rounded-l-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">
                        <span class="sr-only">Previous</span>
                        <svg
                            class="w-5 h-5"
                            aria-hidden="true"
                            fill="currentColor"
                            viewBox="0 0 20 20"
                            xmlns="http://www.w3.org/2000/svg">
                            <path
                                fill-rule="evenodd"
                                d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                                clip-rule="evenodd" />
                        </svg>
                    </a>
                </li>
                {#each Array(Math.ceil(fetchApiData.count / fetchApiData.per_page)) as page, index}
                    <li>
                        <a
                            href="#"
                            on:click={() => handleChangePage(index + 1, 10)}
                            class={fetchApiData.page === index + 1
                                ? 'z-10 px-3 py-2 leading-tight text-blue-600 border border-blue-300 bg-blue-50 hover:bg-blue-100 hover:text-blue-700 dark:border-gray-700 dark:bg-gray-700 dark:text-white'
                                : 'px-3 py-2 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white'}>
                            {index + 1}</a>
                    </li>
                {/each}
                <li>
                    <a
                        href="#"
                        on:click={() => handleChangePage(Number(fetchApiData.page) + 1, 10)}
                        class="{fetchApiData.page === Math.ceil(fetchApiData.count / fetchApiData.per_page)
                            ? 'bg-gray-200 pointer-events-none'
                            : 'bg-white'} block px-3 py-2 leading-tight text-gray-500 border border-gray-300 rounded-r-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">
                        <span class="sr-only">Next</span>
                        <svg
                            class="w-5 h-5"
                            aria-hidden="true"
                            fill="currentColor"
                            viewBox="0 0 20 20"
                            xmlns="http://www.w3.org/2000/svg">
                            <path
                                fill-rule="evenodd"
                                d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                                clip-rule="evenodd" />
                        </svg>
                    </a>
                </li>
            </ul>
        </nav>
    {/if}
</div>

<style>
    a {
        font-family: Inter, ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica Neue, Arial, Noto Sans,
            sans-serif, Apple Color Emoji, Segoe UI Emoji, Segoe UI Symbol, Noto Color Emojier;
        line-height: 1.5;
    }
</style>
