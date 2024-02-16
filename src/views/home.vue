<script setup>
    import Button from 'primevue/button';
</script>

<template>
    <div class="grid grid-nogutter surface-section text-800">
        <div class="col-12 md:col-6 p-6 text-center md:text-left flex align-items-center ">
            <section>
                <span class="block text-6xl font-bold mb-1">Loja Eletrônica </span>
                <div class="text-6xl text-primary font-bold mb-3">Equipamentos de Informática</div>
                <p class="mt-0 mb-4 text-700 line-height-3">Os melhores fornecedores do mercado. Venha conhecer nossa loja física</p>
                
                <Button class="mx-1" label="Saiba mais" v-tooltip="'Conheça nossa linha de produtos'" rounded />
                <Button class="mx-1" label="Promoções" severity="secondary" rounded />
            </section>
        </div>
        <div class="col-12 md:col-6 overflow-hidden">
            <img src="../imagens/drone.jpg" alt="Image" class="md:ml-auto block md:h-full" style="clip-path: polygon(8% 0, 100% 0%, 100% 100%, 0 100%)" />
        </div>
    </div>

    <div class="surface-ground px-4 py-8 md:px-6 lg:px-8">
        <div class="text-900 font-bold text-6xl mb-4 text-center">Departamentos</div>
        <div class="text-700 text-xl mb-6 text-center line-height-3">Os melhores produtos do mercado</div>

        <div class="grid">
            <div class="col-12 lg:col-4">
                <div class="p-3 h-full">
                    <div class="shadow-2 p-3 h-full flex flex-column surface-card" style="border-radius: 6px">
                        <div class="text-900 font-medium text-xl mb-2">Informática</div>
                        <hr class="my-3 mx-0 border-top-1 border-none surface-border" />
                        <img src="../imagens/infor.jpg" alt="">
                        <hr class="mb-3 mx-0 border-top-1 border-none surface-border mt-auto" />
                        <Button label="Info" severity="info" raised />
                    </div>
                </div>
            </div>

            <div class="col-12 lg:col-4">
                <div class="p-3 h-full">
                    <div class="shadow-2 p-3 h-full flex flex-column surface-card" style="border-radius: 6px">
                        <div class="text-900 font-medium text-xl mb-2">Games</div>
                        <hr class="my-3 mx-0 border-top-1 border-none surface-border" />
                        <img src="../imagens/game.jpg" alt="">
                        <hr class="mb-3 mx-0 border-top-1 border-none surface-border" />
                        <Button label="Info" severity="info" raised />
                    </div>
                </div>
            </div>

            <div class="col-12 lg:col-4">
                <div class="p-3 h-full">
                    <div class="shadow-2 p-3 flex flex-column surface-card" style="border-radius: 6px">
                        <div class="text-900 font-medium text-xl mb-2">Telefonia</div>
                        <hr class="my-3 mx-0 border-top-1 border-none surface-border" />
                        <img src="../imagens/tel.jpg" alt="">
                        <hr class="mb-3 mx-0 border-top-1 border-none surface-border" />
                        <Button label="Info" severity="info" raised />
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="surface-section px-4 py-8 md:px-6 lg:px-8">
    <div class="text-700 text-center">
        <div class="text-blue-600 font-bold mb-3"><i class="pi pi-discord"></i>&nbsp;Contatos</div>
        <div class="text-900 font-bold text-5xl mb-3">Entre em contato</div>
        <div class="text-700 text-2xl mb-5">Estamos à sua disposição .</div>
        <Button label="WhatsApp" icon="pi pi-discord" class="font-bold px-5 py-3 p-button-raised p-button-rounded white-space-nowrap"></Button>
    </div>
</div>

</template>

<style scoped>

</style>

