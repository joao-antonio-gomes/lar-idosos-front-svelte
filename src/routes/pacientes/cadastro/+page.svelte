<script lang='ts'>

    import { title } from '../../../store';
    import { Form } from 'svelte-forms-lib';
    import * as yup from 'yup';
    import { api } from '../../../service/api';
    import { goto } from '$app/navigation';
    import { validate_cpf } from 'js-brasil/dist/src/validate.js';
    import InputGeneric from '../../../components/Inputs/InputGeneric.svelte';
    import InputSelect from '../../../components/Inputs/InputSelect.svelte';
    import { notifications } from '../../../service/notification.js';
    import type { Responsavel } from '../../../interface/Responsavel';
    import InputAutoComplete from '../../../components/Inputs/InputAutoComplete.svelte';

    $title = 'Pacientes - Novo';

    const sexoOptions = [
        {
            label: 'Masculino',
            value: 'masculino'
        },
        {
            label: 'Feminino',
            value: 'feminino'
        }
    ];

    const estadoCivilOptions = [
        {
            label: 'Solteiro',
            value: 'solteiro'
        },
        {
            label: 'Casado',
            value: 'casado'
        },
        {
            label: 'Divorciado',
            value: 'divorciado'
        },
        {
            label: 'Viúvo',
            value: 'viuvo'
        }
    ];

    const formProps = {
        initialValues: {
            nome: '',
            cpf: '',
            data_nascimento: undefined,
            sexo: '',
            estado_civil: '',
            responsavel: undefined
        },
        validationSchema: yup.object().shape({
            nome: yup.string().required('Nome é obrigatório'),
            cpf: yup
                .string()
                .required('CPF é obrigatório')
                .test('cpfIsValid', 'CPF inválido', (value) => validate_cpf(value)),
            data_nascimento: yup.date('Data de nascimento inválida').required('Data de nascimento é obrigatório').nullable(),
            sexo: yup.string().required('Sexo é obrigatório'),
            estado_civil: yup.string().required('Estado civil é obrigatório')
        }),
        onSubmit: (paciente) => {
            paciente.cpf = paciente.cpf.replace(/\D+/g, '');

            if (responsavelSelecionado)
                paciente.usuarios = [responsavelSelecionado.id];

            api.post('/pacientes', paciente).then(() => {
                goto('/pacientes');
                notifications.success(`Paciente ${paciente.nome} cadastrado com sucesso`);
            });
        }
    };


    async function getResponsaveis(nome: string) {
        return api.get(`/responsaveis/auto-complete?q[nome_cont]=${nome}&q[nivel_permissao_eq]=3`)
            .then(res => {
                if (res.status !== 200) {
                    notifications.danger('Houve um problema ao carregar os dados dos responsáveis, atualize a página e tente novamente');
                }

                return res.data;
            })
            .catch(erro => {
                notifications.danger('Houve um problema ao carregar os dados dos responsáveis, atualize a página e tente novamente');
            });
    }

    let responsavelSelecionado: Responsavel;
    let responsavelSelecionadoId;

    function setResponsavelSelecionado(responsavel: Responsavel) {
        if (!responsavel)
            return;

        responsavelSelecionado = responsavel;
        responsavelSelecionadoId = responsavel.id;
    }

    let component;
</script>

<div>
    <Form {...formProps} class='content' bind:this={component}>
        <div class='mt-4'>
            <InputGeneric label='Nome*' name='nome' type='text' />
        </div>

        <div class='mt-4'>
            <InputGeneric label='CPF*' name='cpf' type='text' />
        </div>
        <div class='mt-4'>
            <InputGeneric label='Data de Nascimento*' name='data_nascimento' type='date' />
        </div>
        <div class='mt-4'>
            <InputSelect label='Sexo*' name='sexo' options={sexoOptions} />
        </div>
        <div class='mt-4'>
            <InputSelect label='Estado Civil*' name='estado_civil' options={estadoCivilOptions} />
        </div>
        <div class='mt-4'>
            <InputAutoComplete name='responsavel' label='Responsável' itemSelecionado={responsavelSelecionado}
                               searchFunction={getResponsaveis} campoParaApresentar='nome'
                               getSelecionado={setResponsavelSelecionado} />
        </div>
        <div class='flex justify-end mt-4'>
            <button
                class='text-white bg-blue-main hover:bg-blue-500 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800'
                type='submit'>
                Enviar
            </button>
        </div>
    </Form>
</div>
