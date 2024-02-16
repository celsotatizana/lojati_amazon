<script>
    import axios from 'axios';
    import InputText from 'primevue/inputtext';
    import Password from 'primevue/password';
    import Checkbox from 'primevue/checkbox';
    import Button from 'primevue/button';
    import Toast from 'primevue/toast';
    export default{
        components:{
            InputText,
            Password,
            Checkbox,
            Button,
            Toast
        },
        data(){
            return{
                usuarios:'',
                senha:'',
                lembrarsenha:false,
            }
        },
        methods:{
            async login(){
                try {
                    const response=await axios.post('http://18.230.134.155:3333/api/login',{
                        usuario: this.usuario,
                        senha: this.senha,
                    });
                    localStorage.setItem('usuario','logou');
                    this.$router.push('/admin');
                } catch (error) {
                    this.$refs.toast.add({
                        severity: 'error',
                        summary: 'Erro',
                        detail: 'Usuário ou Senha Inválido',
                        life: 4000,
                    });
                }
            }
        },
    }
</script>

<template>
    <div class="login_form">
        <div class="card">
            <h1>Login</h1>
            <div class="p_field">
                <InputText placeholder="Usuário" v-model="usuario"/>
            </div>
            <div class="p_field">
                <Password placeholder="Senha" v-model="senha"/>
            </div>
            <div class="p_field">
                <Checkbox id="cd1" v-model="lembrarsenha"/>
                <label id="label" for="cd1">Lembrar senha</label>
            </div>
            <Button label="Logar" @click="login" />
            <Toast ref="toast" />
        </div>
    </div>
</template>

<style>
    .login_form{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    .card{
        padding: 2rem;
        border-radius: 5px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.3 );
    }
    .p_field{
        margin-bottom: 5px;
    }
    label{
        margin-left: 5px;
    }
</style>
