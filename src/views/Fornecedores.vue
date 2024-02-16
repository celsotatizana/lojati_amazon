<script>
    import axios from 'axios';
    import Admin from './Admin.vue';
    import DataTable from 'primevue/datatable';
    import Column from 'primevue/column';
    import Button from 'primevue/button';
    import 'primeicons/primeicons.css';
    import Dialog from 'primevue/dialog';
    import InputText from 'primevue/inputtext';
    export default{
        components:{
            Admin,
            DataTable,
            Column,
            Button,
            Dialog,
            InputText
        },
        data(){
            return {
                dialogo_Visivel: false,
                cadastrarDialogo_Visivel: false,
                fornecedores: [],
                formulario: {
                    codigo: '',
                    fornecedor: '',
                    email: '',
                    telefone: ''
                }
            }
        },
        async mounted(){
            try {
                const urlApi='http://18.230.134.155:3333/api/fornecedores';
                const resposta=await axios.get(urlApi);
                console.log("Dados da resposta: ", resposta.data);
                this.fornecedores=resposta.data; 
            } catch (error) {
                console.error("Erro ao carregar fornecedores");
            }
        },
        methods:{
            editar(slotProps){
                this.dialogo_Visivel=true;
                
                console.log('Editar');
                console.log('Codigo:',slotProps.data.codigo);
                console.log('Fornecedor:',slotProps.data.fornecedor);
                console.log('Email:',slotProps.data.email);
                console.log('Telefone:',slotProps.data.telefone);
                
                this.formulario.codigo=slotProps.data.codigo;
                this.formulario.fornecedor=slotProps.data.fornecedor;
                this.formulario.email=slotProps.data.email;
                this.formulario.telefone=slotProps.data.telefone;

            },
            cadastrar(){
                this.cadastrarDialogo_Visivel=true;
            },
            async excluir(slotProps){
                console.log('Excluir');
                console.log('Codigo:',slotProps.data.codigo);
                console.log('Fornecedor:',slotProps.data.fornecedor);
                console.log('Email:',slotProps.data.email);
                console.log('Telefone:',slotProps.data.telefone);

                if(slotProps.data.codigo){
                    const idFornecedor=slotProps.data.codigo;
                    try {
                        const resposta=await axios.delete(`http://18.230.134.155:3333/api/fornecedor/${idFornecedor}`);
                        console.log('Exclusão bem sucedida: ', resposta.data);
                    } catch (error) {
                        console.error('Erroa ao excluir', error);
                    }
                    location.reload(true);
                } else{
                    console.warn('Nenhum fornecedor selecionado para exclusão ');
                }
            },
            fechar_Dialogo(){
                this.dialogo_Visivel=false;
            },
            fechar_DialogoCadastrar(){
                this.cadastrarDialogo_Visivel=false;
            },
            async salvar_Edicao(){
                const dadosAtualizarEdicao={
                    fornecedor: this.formulario.fornecedor,
                    email: this.formulario.email,
                    telefone: this.formulario.telefone
                }
                try {
                    const resposta= await axios.put(`http://18.230.134.155:3333/api/fornecedor/${this.formulario.codigo}`, dadosAtualizarEdicao);
                    console.log('Atualização bem sucedida:',resposta.data);
                } catch (error) {
                    console.error('Erro ao atualizar ',error);
                }    
                //location.reload(true);
            },
            async salvar_Cadastro(){
                const dadosSalvarCadastro={
                    fornecedor: this.formulario.fornecedor,
                    email: this.formulario.email,
                    telefone: this.formulario.telefone
                }
                if (dadosSalvarCadastro.fornecedor=='' || dadosSalvarCadastro.email=='' || dadosSalvarCadastro.telefone==''){
                    alert('Preencha todos os campos ')
                } else{
                    try {
                        const resposta= await axios.post(`http://18.230.134.155:3333/api/novoFornecedor`, dadosSalvarCadastro);
                        console.log('Cadastro bem sucedido:',resposta.data);
                        location.reload(true);
                    } catch (error) {
                        console.error('Erro ao cadastrar ',error);
                    }    
  
                }
             
            }
        }

    }
</script>

<template>
    <Admin>
        <div class="p-d-flex p-jc-center ml-2 mt-5">
             <h2>Fornecedores</h2>
        </div>
        <div class="p-d-flex m-2">
            <Button label="Cadastrar" severity="info" rounded @click="cadastrar" />
        </div>

        <DataTable :value="fornecedores" tableStyle="min-width: 50rem">
            <Column field="codigo" header="Código"></Column>
            <Column field="fornecedor" header="Fornecedor"></Column>
            <Column field="email" header="Email"></Column>
            <Column field="telefone" header="Telefone"></Column>
            <Column  header="Ações">
                <template #body="slotProps">
                    <Button icon="pi pi-pencil" rounded @click="editar(slotProps)"/>
                    <Button icon="pi pi-trash" severity="danger" rounded @click="excluir(slotProps)"/>
                </template>
            </Column>
        </DataTable>
        <!-- Editar -->
        <Dialog v-model="dialogo_Visivel" :visible="dialogo_Visivel" modal header="Editar Fornecedor" :style="{ width: '50rem' }" :breakpoints="{ '1199px': '75vw', '575px': '90vw' }">
            <form action="">
                <div class="p-field p-fluid" >
                    <label for="fornecedor">Fornecedor</label>
                    <InputText type="text" v-model="formulario.fornecedor" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="email">Email</label>
                    <InputText type="text" v-model="formulario.email" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="telefone">Telefone</label>
                    <InputText type="text" v-model="formulario.telefone" />
                </div>
                <div class="p-field" >
                    <Button label="Editar" type="submit" severity="secondary" class="m-2" @click="salvar_Edicao"/>
                    <Button label="Cancelar" class="m-2" @click="fechar_Dialogo"/>
                </div>
            </form>
        </Dialog>
        <!-- Cadastrar -->
        <Dialog v-model="cadastrarDialogo_Visivel" :visible="cadastrarDialogo_Visivel" modal header="Cadastrar Fornecedor" :style="{ width: '50rem' }" :breakpoints="{ '1199px': '75vw', '575px': '90vw' }">
            <form action="">
                <div class="p-field p-fluid" >
                    <label for="fornecedor">Fornecedor</label>
                    <InputText type="text" v-model="formulario.fornecedor" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="email">Email</label>
                    <InputText type="text" v-model="formulario.email" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="telefone">Telefone</label>
                    <InputText type="text" v-model="formulario.telefone" />
                </div>
                <div class="p-field" >
                    <Button label="Cadastrar"  severity="secondary" class="m-2" @click="salvar_Cadastro"/>
                    <Button label="Cancelar" class="m-2" @click="fechar_DialogoCadastrar"/>
                </div>
            </form>
        </Dialog>
    </Admin>
</template>

<style>

</style>