<script>
    export let value;
    export let errors;
    export let name;
    export let label;
    export let type = "text";
    export let classes = "";

    function typeAction(node) {
        node.type = type;
    }
</script>

<label for={name} class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">{label}</label>
<input id={name} use:typeAction
       class="bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 ${classes}"
       bind:value={value}/>
{#if errors}
    <small class="mt-2 text-sm text-red-600 dark:text-red-500">{errors}</small>
{/if}
