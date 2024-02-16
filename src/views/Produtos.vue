<script>
    import axios from 'axios';
    import Admin from './Admin.vue';
    import DataTable from 'primevue/datatable';
    import Column from 'primevue/column';
    import Button from 'primevue/button';
    import 'primeicons/primeicons.css';
    import Dialog from 'primevue/dialog';
    import InputText from 'primevue/inputtext';
    import FileUpload from 'primevue/fileupload';
    export default{
        components:{
            Admin,
            DataTable,
            Column,
            Button,
            Dialog,
            InputText,
            FileUpload
        },
        data(){
            return {
                dialogo_Visivel: false,
                cadastrarDialogo_Visivel: false,
                produtos: [],
                formulario: {
                    codigo: '',  
                    produto: '',
                    quantidade: '',
                    preco: '',
                    codigo_fornecedor: ''
                }
            }
        },
        async mounted(){
            try {
                const urlApi='http://18.230.134.155:3333/api/produtos';
                const resposta=await axios.get(urlApi);
                console.log("Dados da resposta: ", resposta.data);
                this.produtos=resposta.data; 
            } catch (error) {
                console.error("Erro ao carregar produtos");
            }
        },
        methods:{
            conversorBufferParaURL(buffer){
                try {
                    if (!buffer){
                        return '';
                    } else {
                        const bytes = new Uint8Array(buffer.data);
                        const binario = bytes.reduce((acc,byte) => acc + String.fromCharCode(byte),'');
                        const base64String = btoa(binario);
                        return `data:${buffer.type};base64,${base64String}`;
                    }
                } catch (error) {
                    console.error('Erro ao converter o buffer para URL', error);
                    return '';
                }
            },
            editar(slotProps){
                this.dialogo_Visivel=true;
                this.formulario.codigo=slotProps.data.codigo;
                this.formulario.produto=slotProps.data.produto;
                this.formulario.quantidade=slotProps.data.quantidade;
                this.formulario.preco=slotProps.data.preco;
                this.formulario.codigo_fornecedor=slotProps.data.codigo_fornecedor;

            },
            cadastrar(){
                this.formulario.codigo='';
                this.formulario.produto='';
                this.formulario.quantidade='';
                this.formulario.preco='';
                this.formulario.codigo_fornecedor='';
                this.cadastrarDialogo_Visivel=true;
            },
            async excluir(slotProps){
       
                if(slotProps.data.codigo){
                    const idProduto=slotProps.data.codigo;
                    try {
                        const resposta=await axios.delete(`http://18.230.134.155:3333/api/produto/${idProduto}`);
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
                    produto: this.formulario.produto,
                    quantidade: this.formulario.quantidade,
                    preco: this.formulario.preco,
                    codigo_fornecedor: this.formulario.codigo_fornecedor
                }
                try {
                    const resposta= await axios.put(`http://18.230.134.155:3333/api/produto /${this.formulario.codigo}`, dadosAtualizarEdicao);
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
        <div class="p-d-flex p-jc-center ml-6 mt-5">
             <h2>Produtos</h2>
        </div>
        <div class="p-d-flex p-jc-center ml-6 mb-2">
             <Button label="Cadastrar" severity="info" rounded @click="cadastrar()"/>
        </div>
        <DataTable :value="produtos" tableStyle="min-width: 50rem">
            <Column field="codigo" header="Código"></Column>
            <Column field="produto" header="Produto"></Column>
            <Column field="imagem" header="Imagem">
                <template #body="slotProps">
                    <img :src="conversorBufferParaURL(slotProps.data.imagem)" style="width: 100px; object-fit:cover">
                </template>
            </Column>
            <Column field="quantidade" header="Quantidade"></Column>
            <Column field="preco" header="Preço"></Column>
            <Column field="codigo_fornecedor" header="Fornecedor"></Column>
            <Column  header="Ações">
                <template #body="slotProps">
                    <Button icon="pi pi-pencil" rounded @click="editar(slotProps)"/>
                    <Button icon="pi pi-trash" severity="danger" rounded @click="excluir(slotProps)"/>
                </template>
            </Column>
        </DataTable>
        <!-- Editar -->
        <Dialog v-model="dialogo_Visivel" :visible="dialogo_Visivel" modal header="Editar Produto" :style="{ width: '50rem' }" :breakpoints="{ '1199px': '75vw', '575px': '90vw' }">
            <form action="">
                <div class="p-field p-fluid" >
                    <label for="produto">Produto</label>
                    <InputText type="text" v-model="formulario.produto" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="imagem">Imagem</label>
                    <FileUpload mode="basic" name="demo[]" url="/api/upload" accept="image/*" :maxFileSize="1000000" @upload="onUpload"/>
                </div>
                <div class="p-field p-fluid" >
                    <label for="quantidade">Quantidade</label>
                    <InputText type="text" v-model="formulario.quantidade" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="preco">Preço</label>
                    <InputText type="text" v-model="formulario.preco" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="codigo_fornecedor">Fornecedor</label>
                    <InputText type="text" v-model="formulario.codigo_fornecedor" />
                </div>
                <div class="p-field" >
                    <Button label="Editar" type="submit" severity="secondary" class="m-2" @click="salvar_Edicao"/>
                    <Button label="Cancelar" class="m-2" @click="fechar_Dialogo"/>
                </div>
            </form>
        </Dialog>
        <!-- Cadastrar -->
        <Dialog v-model="cadastrarDialogo_Visivel" :visible="cadastrarDialogo_Visivel" modal header="Cadastrar Produto" :style="{ width: '50rem' }" :breakpoints="{ '1199px': '75vw', '575px': '90vw' }">
            <form action="">
                <div class="p-field p-fluid" >
                    <label for="produto">Produto</label>
                    <InputText type="text" v-model="formulario.produto" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="imagem">Imagem</label>
                    <FileUpload mode="basic" name="demo[]" url="/api/upload" accept="image/*" :maxFileSize="1000000" @upload="onUpload" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="quantidade">Quantidade</label>
                    <InputText type="text" v-model="formulario.quantidade" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="preco">Preço</label>
                    <InputText type="text" v-model="formulario.preco" />
                </div>
                <div class="p-field p-fluid" >
                    <label for="codigo_fornecedor">Fornecedor</label>
                    <InputText type="text" v-model="formulario.codigo_fornecedor" />
                </div>
                <div class="p-field" >
                    <Button label="Editar" type="submit" severity="secondary" class="m-2" @click="salvar_Edicao"/>
                    <Button label="Cancelar" class="m-2" @click="fechar_DialogoCadastrar"/>
                </div>
            </form>
        </Dialog>
    </Admin>
</template>

<style>

</style>