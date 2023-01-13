<script>
    import SidebarIcon from "./SidebarIcon.svelte";

    export let icon;
    export let itemName;
    export let itemLink;
    export let isMenuOpen;
</script>

<li>
    <a href={itemLink}
       class="flex items-center p-2 text-base font-normal text-white hover:text-blue-main rounded-lg hover:bg-white transition duration-75">
        <SidebarIcon icon={icon}/>
        {#if isMenuOpen}
            <span class="ml-3">{itemName}</span>
        {/if}
    </a>
</li>
