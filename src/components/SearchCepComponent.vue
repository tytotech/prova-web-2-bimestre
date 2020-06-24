<template>
    <div>
        <h1 v-text="title"></h1>
        <form @submit.prevent="onSubmit">
            <input type="text" placeholder="Informe o CEP" v-model="cep">
            <button type="submit">Buscar CEP</button>
        </form>

        <div v-if="preloader">
            <img src="../assets/preloader.gif" alt="Carregando...">
        </div>

        <div v-if="error != ''">
            {{error}}
        </div>

        <div v-show = "address.bairro" >
            <p><b>Logradouro: </b> {{ address.logradouro }}</p>
            <p><b>Bairro: </b> {{ address.bairro }}</p>
            <p><b>Cidade: </b> {{ address.localidade }}</p>
            <p><b>Estado: </b> {{ address.uf }}</p>
            <p><b>CEP: </b> {{ address.cep }}</p>
        </div>
    </div>
</template>
<script>
    export default {
        data () {
            return  {
                title: "Buscar CEP com VIACEP",
                cep: '',
                address: {},
                preloader: false,
                error: ''
            }
        },
        methods: {
            onSubmit () {

                this.reset()
                
                this.preloader = true
                this.$http.get('https://viacep.com.br/ws/'+ this.cep +'/json/')
                .then(response => {
                    this.address = response.body
                }, error => {
                    console.log(error)
                    this.error = 'CEP Invalido'
                })
                .finally(() => {
                    this.preloader = false
                })
            },
            reset() {
                this.error = '',
                this.address = {}
            }
        }
    }
</script>
<style>

</style>