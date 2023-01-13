<script lang="ts">
    import {api} from "../../service/api.js";
    import {format} from "date-fns";
    import {maskBr} from "js-brasil/index.js";
    import {title} from "../../store.js";
    import Table from "../../components/Table/Table.svelte";
    import ModalDelete from "../../components/Modal/ModalDelete.svelte";
    import {onMount} from "svelte";
    import {Modal} from "flowbite";
    import {notifications} from '../../service/notification.js'

    $title = "Pacientes";

    const tableStructure = [
        {
            label: "Nome",
            value: "nome",
            isOrdinal: true,
            dataCallback: (value) => value
        },
        {
            label: "CPF",
            value: "cpf",
            isOrdinal: false,
            dataCallback: (value) => maskBr.cpf(value)
        },
        {
            label: "Data Nascimento",
            value: "data_nascimento",
            isOrdinal: true,
            dataCallback: (value) => format(new Date(value), 'dd/MM/yyyy')
        },
        {
            label: "Admissão",
            value: "created_at",
            isOrdinal: true,
            dataCallback: (value) => format(new Date(value), 'dd/MM/yyyy')
        }
    ]
    const loadingPhrase = "Carregando pacientes...";
    const tableActions = [
        {
            label: "Editar",
            callback: (paciente) => console.log('editado ' + paciente.id)
        },
        {
            label: "Excluir",
            callback: (paciente) => openModalDeletePaciente(paciente)
        }
    ]

    async function fetchPacientes() {
        const res = await api.get("/pacientes");
        return res.data;
    }

    let isModalDelecaoPacienteOpen = false;
    let pacienteDelecao = {};
    let modalDelete;

    function openModalDeletePaciente(paciente) {
        isModalDelecaoPacienteOpen = true;
        pacienteDelecao = paciente;
        modalDelete.show()
    }

    function handleDeletePaciente(paciente) {
        notifications.info(`Deletando paciente Deletando paciente Deletando paciente Deletando paciente ${paciente.nome}`, 100000)
    }

    onMount(() => {
        modalDelete = new Modal(document.getElementById("modal-delete"));
    })
</script>

<Table fetchApiData={fetchPacientes} tableActions={tableActions} tableStructure={tableStructure}
       loadingPhrase={loadingPhrase}/>

<ModalDelete callbackDelecao={() => handleDeletePaciente(pacienteDelecao)}
             textoConfirmacao="Você tem certeza que deseja deletar o paciente {pacienteDelecao.nome}?"/>

<div class="mt-6 flex justify-end">
    <a href="/pacientes/cadastro"
       class="text-white bg-blue-main hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">
        Novo
    </a>
</div>

