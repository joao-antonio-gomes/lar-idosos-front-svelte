<script>
    import '../app.postcss';
    import Sidebar from '../components/Sidebar/Sidebar.svelte';
    import { title } from '../store.js';
    import Toast from '../components/Toast/Toast.svelte';
</script>

<main>
    <Sidebar />
    <div class="ml-20 md:ml-60 px-32 pt-16">
        <div class="pb-6">
            <h1
                class="mb-4 text-3xl font-extrabold leading-none tracking-tight text-gray-900 md:text-4xl lg:text-5xl dark:text-white">
                {$title}
            </h1>
            <hr class="h-px my-2 bg-gray-200 border-0 dark:bg-gray-700" />
        </div>
        <div class="w-full">
            <slot />
        </div>
    </div>
</main>

<Toast />
