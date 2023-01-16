<script lang='ts'>
    import { ErrorMessage, Field } from 'svelte-forms-lib';

    export let name: string;
    export let label: string;
    export let type: string = 'text';
    export let classes: string = '';
</script>

<label for={name} class='block mb-2 text-sm font-medium text-gray-900 dark:text-white'>{label}</label>
<Field
    {type}
    {name}
    id={name}
    class={`bg-white border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 ${classes}`} />
<ErrorMessage {name} class='mt-2 text-sm text-red-600 dark:text-red-500' />
