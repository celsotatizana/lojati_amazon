<script setup>
    import {ref, onMounted} from 'vue';
    import axios from 'axios';
    const produtos=ref([]);
    const convesorBufferParaURL=(buffer)=>{
        try {
            if (!buffer){
                return '';
            } else{
                const bytes=new Uint8Array(buffer.data);
                const binario=bytes.reduce((acc,byte) => acc + String.fromCharCode(byte),'');
                const base64String=btoa(binario);
                return `data:${buffer.type};base64, ${base64String}`;
            }
        } catch (error) {
            console.error('Erro ao converter de buffer para URL', error);
            return '';
        }
    }
    onMounted(async()=>{
        try {
            const response= await axios.get('http://18.230.134.155:3333/api/produtos');
            console.log(response.data);
            produtos.value=response.data.map(produtos=>({
                ...produtos,
                imagem: convesorBufferParaURL(produtos.imagem), 
            }))
        } catch (error) {
            console.error('Erro ao acessar banco produtos',error);
        }
    });
    /*const produtos=[
        {
            id: 1,
            nome: 'Produto A',
            preco: '10,00',
            quantidade: 100,
            imagem: 'src/imagens/drone.jpg'
        },
        {
            id: 2,
            nome: 'Produto B',
            preco: '20,00',
            quantidade: 200,
            imagem: 'src/imagens/notebook.jpg'
         }
    ]*/
</script>

<template>
    <div class="surface-ground px-4 py-5 md:px-6 lg:px-8">
        <div class="grid">
            <div class="col-12 md:col-6 lg:col-3" v-for="produto in produtos" :key="produtos.codigo">
                <div class="surface-card shadow-2 p-3 border-round">
                    <div class="flex justify-content-between mb-3">
                        <div>
                            <span class="block text-500 font-medium mb-3">{{ produto.produto }}</span>
                            <div class="text-900 font-medium text-xl">{{produto.preco}}</div>
                        </div>
                    </div>
                    <div style="width:100px">
                        <img :src="produto.imagem" style="width:100px; object-fit:cover">
                    </div>
                    <span class="text-green-500 font-medium">Quantidade </span>
                    <span class="text-500">{{ produto.quantidade }}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>

</style>