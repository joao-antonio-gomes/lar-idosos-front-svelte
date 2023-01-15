<script lang="ts">
    import { api } from '../../service/api.js';
    import { format } from 'date-fns';
    import { maskBr } from 'js-brasil/index.js';
    import { title } from '../../store.js';
    import Table from '../../components/Table/Table.svelte';
    import ModalDelete from '../../components/Modal/ModalDelete.svelte';
    import { onMount } from 'svelte';
    import { Modal } from 'flowbite';
    import { notifications } from '../../service/notification.js';
    import type { TableStructure } from '../../interface/TableStructure';
    import type { TableActions } from '../../interface/TableActions';
    import type { FetchApiData } from '../../interface/FetchApiData';

    interface Paciente {
        id: number;
        nome: string;
        cpf: string;
        estado_civil: string;
        sexo: string;
        data_nascimento: Date;
        created_at: Date;
    }

    interface PacientePaginado extends FetchApiData {
        data: Paciente[];
    }

    $title = 'Pacientes';

    let modalDelete;
    onMount(() => {
        modalDelete = new Modal(document.getElementById('modal-delete'), {
            backdrop: 'static'
        });
    });

    let pacienteDelecao: Paciente;

    function openModalDeletePaciente(paciente) {
        pacienteDelecao = paciente;
        modalDelete.show();
    }

    function handleDeletePaciente(paciente) {
        api.delete(`/pacientes/${paciente.id}`)
            .then((res) => {
                if (res.status !== 204)
                    notifications.danger(
                        `Houve um erro ao excluir o paciente ${paciente.nome}, atualize a página e tente novamente ou entre em contato com o suporte.`
                    );

                notifications.success(`Paciente ${paciente.nome} excluído com sucesso.`, 1500);
                modalDelete.hide();
                pacientesFetch = fetchPacientes();
            })
            .catch((res) => {
                notifications.danger(
                    `Houve um erro ao excluir o paciente ${paciente.nome}, atualize a página e tente novamente ou entre em contato com o suporte.`
                );
            });
    }

    const tableStructure: TableStructure[] = [
        {
            label: 'Nome',
            value: 'nome',
            isOrdinal: true,
            callback: (value) => value
        },
        {
            label: 'CPF',
            value: 'cpf',
            isOrdinal: false,
            callback: (value) => maskBr.cpf(value)
        },
        {
            label: 'Data Nascimento',
            value: 'data_nascimento',
            isOrdinal: true,
            callback: (value) => format(new Date(value), 'dd/MM/yyyy')
        },
        {
            label: 'Admissão',
            value: 'created_at',
            isOrdinal: true,
            callback: (value) => format(new Date(value), 'dd/MM/yyyy')
        }
    ];
    const loadingPhrase = 'Carregando pacientes...';
    const tableActions: TableActions[] = [
        {
            label: 'Editar',
            callback: (paciente) => console.log('editado ' + paciente.id)
        },
        {
            label: 'Excluir',
            callback: (paciente) => openModalDeletePaciente(paciente)
        }
    ];

    let promiseResolved = false;
    let pacientesFetch: Promise<any> | PacientePaginado = fetchPacientes();

    async function fetchPacientes(page = 1, per_page = 10) {
        promiseResolved = false;
        return api.get(`/pacientes?page=${page}&per_page=${per_page}`).then((res) => {
            pacientesFetch = res.data;
            promiseResolved = true;
            return res.data;
        });
    }

    async function handleChangePage(page = 1, per_page = 10) {
        pacientesFetch = fetchPacientes(page, per_page);
    }
</script>

<Table
    fetchApiData={pacientesFetch}
    {handleChangePage}
    {tableActions}
    {tableStructure}
    {loadingPhrase}
    {promiseResolved} />

{#if pacienteDelecao}
    <ModalDelete
        callbackDelecao={() => handleDeletePaciente(pacienteDelecao)}
        callbackCloseModal={() => modalDelete.hide()}
        textoConfirmacao="Você tem certeza que deseja deletar o paciente {pacienteDelecao.nome}?" />
{/if}

<div class="mt-6 flex justify-end">
    <a
        href="/pacientes/cadastro"
        class="text-white bg-blue-main hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">
        Novo
    </a>
</div>
