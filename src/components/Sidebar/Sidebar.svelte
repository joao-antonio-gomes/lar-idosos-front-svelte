<script>
    import SidebarSingleItem from "./SidebarSingleItem.svelte";
    import SidebarMultiItem from "./SidebarMultiItem.svelte";
    import {faHome, faPersonCane} from "@fortawesome/free-solid-svg-icons";

    let isMenuOpen = true;

    const pacientesLinks = [
        {
            itemName: 'Listar',
            itemLink: '/pacientes'
        },
        {
            itemName: 'Novo',
            itemLink: '/pacientes/cadastro'
        },
    ]
</script>

<div id="drawer-navigation"
     class="bg-blue-main fixed z-40 h-full p-4 overflow-y-auto dark:bg-gray-800 transition-all duration-300 {isMenuOpen ? 'w-60' : 'w-20 md:w-60'}"
     tabindex="-1"
     aria-labelledby="drawer-navigation-label">
    {#if isMenuOpen}
        <button type="button" data-drawer-hide="drawer-navigation" aria-controls="drawer-navigation"
                on:click={()=> {isMenuOpen = false}}
                class="text-white hover:text-blue-main hover:bg-white rounded-lg text-sm p-1.5 absolute top-2.5 right-2.5 inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white md:hidden">
            <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"
                 xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd"
                      d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                      clip-rule="evenodd"></path>
            </svg>
            <span class="sr-only">Fechar menu</span>
        </button>
    {:else}
        <button type="button" data-drawer-show="drawer-navigation" aria-controls="drawer-navigation"
                on:click={()=> {isMenuOpen = true}}
                class="text-white hover:text-blue-main hover:bg-white rounded-lg text-sm p-1.5 absolute top-2.5 right-7 inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                 stroke="currentColor"
                 class="w-5 h-5">
                <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"/>
            </svg>
            <span class="sr-only">Abrir menu</span>
        </button>
    {/if}
    <div class="py-4 mt-2 overflow-y-auto">
        <ul class="space-y-2">
            <SidebarSingleItem itemName="Início" itemLink="/" icon={faHome} isMenuOpen={isMenuOpen}/>
            <SidebarSingleItem itemName="Pacientes" itemLink="/pacientes" icon={faPersonCane} isMenuOpen={isMenuOpen}/>
            <!--            <SidebarMultiItem categoryName="Pacientes" icon={faPersonCane} isMenuOpen={isMenuOpen}-->
            <!--                              links={pacientesLinks} dropdownName="pacientes"/>-->
        </ul>
    </div>
</div>
