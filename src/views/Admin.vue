<script setup>
    import { useRouter } from 'vue-router';
    const rota = useRouter();
    import TabMenu from 'primevue/tabmenu';
    import Button from 'primevue/button';
    const menuItens=[
        {
            label: 'Fornecedores',
            command: () => navegarPara('/fornecedores')
        },
        {
            label: 'Produtos',
            command: () => navegarPara('/produtos')
        },
    ]
    const navegarPara = (to) => {
      rota.push(to);
    }
    const usuario=localStorage.getItem('usuario');
    if(usuario!='logou'){
      navegarPara('/login');
    }
    console.log(usuario);
    function sair(){
        localStorage.setItem('usuario','');
        location.reload(true);
    }
</script>

<template>
    <div class="p-d-flex p-jc-center ml-6 mt-5">
        <h2>Dashboard Administrativa</h2>
        <Button label="Sair" severity="info" rounded @click="sair" />
    </div>
    <TabMenu :model="menuItens" />
    <div class="admin">
        <slot></slot>
    </div>
</template>

<style>
    .admin{
        margin-top: 20px;
    }
</style>