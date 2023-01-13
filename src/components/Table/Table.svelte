<script lang="ts">
    export let fetchApiData;
    export let loadingPhrase = "";
    export let tableStructure = []; //label, value, isOrdinal
    export let tableActions = []; //label e link
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
                                <svg xmlns="http://www.w3.org/2000/svg" class="w-3 h-3 ml-1" aria-hidden="true"
                                     fill="currentColor" viewBox="0 0 320 512">
                                    <path d="M27.66 224h264.7c24.6 0 36.89-29.78 19.54-47.12l-132.3-136.8c-5.406-5.406-12.47-8.107-19.53-8.107c-7.055 0-14.09 2.701-19.45 8.107L8.119 176.9C-9.229 194.2 3.055 224 27.66 224zM292.3 288H27.66c-24.6 0-36.89 29.77-19.54 47.12l132.5 136.8C145.9 477.3 152.1 480 160 480c7.053 0 14.12-2.703 19.53-8.109l132.3-136.8C329.2 317.8 316.9 288 292.3 288z"/>
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
        {#await fetchApiData()}
            <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">
                {loadingPhrase}
            </tr>
        {:then items}
            {#each items as item}
                <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">
                    {#each tableStructure as row, index}
                        {#if index === 0}
                            <th scope="row"
                                class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                                {row.dataCallback(item[row.value])}
                            </th>
                        {:else}
                            <td class="px-6 py-4">
                                {row.dataCallback(item[row.value])}
                            </td>
                        {/if}
                    {/each}
                    <td class="px-6 py-4 text-right flex justify-between lg:w-48">
                        {#each tableActions as action}
                            <button type="button"
                                    on:click={action.callback(item)}
                                    class="px-2 py-1 mr-2 font-medium text-center text-blue-main bg-white border border-blue-main rounded-lg hover:bg-blue-main hover:text-white focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                                {action.label}
                            </button>
                        {/each}
                    </td>
                </tr>
            {/each}
        {:catch error}
            Erro!
        {/await}
        </tbody>
    </table>
</div>
