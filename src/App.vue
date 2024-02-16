<script setup>
    import Menubar from 'primevue/menubar';
      import { useRouter } from 'vue-router';
      import router from './router';
      const rota = useRouter();
    const menuItens=[
      {
        label:'Home',
        command: () => navegarPara('/')
      },
      {
        label:'Loja',
        command: () => navegarPara('/loja')
      },
      {
        label:'Sobre',
        to:'/contatos'
      },
      {
        label:'Administrador',
        command: () => navegarPara('/admin')
      },
      {
        label:'Programador',
        to:'/cadastro'
      },
    ];
    const navegarPara = (to) => {
      rota.push(to);
    }
 
</script>

<template>
    <Menubar :model="menuItens" />
    <RouterView/>
</template>

<style scoped>

</style>
