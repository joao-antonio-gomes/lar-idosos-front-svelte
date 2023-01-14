<script>
    import { title } from "../../../store";
    import { Form, Field, ErrorMessage, Select } from "svelte-forms-lib";
    import * as yup from 'yup';
    import { api } from "../../../service/api";
    import { validate_cpf } from "js-brasil/dist/src/validate.js";
    import { goto } from "$app/navigation";
    import InputGeneric from "../../../components/Inputs/InputGeneric.svelte";
    import InputSelect from "../../../components/Inputs/InputSelect.svelte";
    import InputError from "../../../components/Inputs/InputError.svelte";
    import { notifications } from "../../../service/notification.js";

    $title = "Pacientes - Novo";

    const sexoOptions = [
        {
            label: "Masculino",
            value: "masculino"
        },
        {
            label: "Feminino",
            value: "feminino"
        }
    ];

    const estadoCivilOptions = [
        {
            label: "Solteiro",
            value: "solteiro"
        },
        {
            label: "Casado",
            value: "casado"
        },
        {
            label: "Divorciado",
            value: "divorciado"
        },
        {
            label: "Viúvo",
            value: "viuvo"
        }
    ];

    const formProps = {
        initialValues: {
            nome: '',
            cpf: '',
            data_nascimento: undefined,
            sexo: '',
            estado_civil: ''
        },
        validationSchema: yup.object().shape({
            nome: yup.string().required("Nome é obrigatório"),
            cpf: yup.string().required("CPF é obrigatório").test(
                'cpfIsValid', "CPF inválido", (value) => validate_cpf(value)
            ),
            data_nascimento: yup.date().required("Data de nascimento é obrigatório").nullable(),
            sexo: yup.string().required("Sexo é obrigatório"),
            estado_civil: yup.string().required("Estado civil é obrigatório"),
        }),
        onSubmit: paciente => {
            paciente.cpf = paciente.cpf.replace(/\D+/g, '');
            api.post("/pacientes", paciente)
                .then(() => {
                    goto('/pacientes');
                    notifications.success(`Paciente ${paciente.nome} cadastrado com sucesso`);
                });

        }
    };

</script>

<div>
    <Form class="content" {...formProps}>
        <div class="mt-4">
            <InputGeneric name="nome" type="text" label="Nome"/>

        </div>

        <div class="mt-4">
            <InputGeneric name="cpf" type="text" label="CPF"/>
        </div>
        <div class="mt-4">
            <InputGeneric name="data_nascimento" type="date" label="Data de Nascimento"/>
        </div>
        <div class="mt-4">
            <InputSelect options={sexoOptions}
                         name="sexo" label="Sexo"/>
        </div>
        <div class="mt-4">
            <InputSelect options={estadoCivilOptions}
                         name="estado_civil" label="Estado Civil"/>
        </div>
        <div class="flex justify-end mt-4">
            <button type="submit"
                    class="text-white bg-blue-main hover:bg-blue-500 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">
                Enviar
            </button>
        </div>
    </Form>
</div>
