<template>

    <div class="form-body">
        <div class="row">
            <div class="form-holder">
                <div class="form-content" style="padding-top: 150px;">
                    <div class="form-items">
                        <div class="website-logo-inside">
                            <a href="#">
                                <div class="logo">
                                    <img :src="`anoni.png`" :ref="`#`">
                                </div>
                            </a>
                        </div>
                        <h3>Confesiones Boombang</h3>
                        <p>Este es tu momento de desahogarte en el anonimato.</p>
                        <form @submit="formSubmit">
                              <p v-if="errors.length">
                                <b>Por favor, corrija el(los) siguiente(s) error(es):</b>
                                <ul>
                                <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
                                </ul>
                                </p>
                            <textarea class="form-control" type="text" v-model="message" v-on:click="confirmacion = ''"  name="confession" placeholder="Escribe tu Confesión..." v-validate="'required|min:10'"></textarea>
                            <div class="form-button">
                                <button id="submit" type="submit" class="ibtn">Enviar</button>
                            </div>
                        </form>
                        <div class="alert alert-warning alert-dismissible fade show" role="alert" v-show="seen">
                            <strong>Confirmación de Envio</strong>
                            <br>
                            <span v-show="confirmacion">{{confirmacion}}</span>
                            <br>
                            <span>{{ errors.first('confession') }}</span>
                        </div>
                        <div class="other-links">
                            <span>Desarrollado por <b>#DevsTeam</b></span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
     
<script>
    export default {
        mounted() {
            console.log('Component mounted.');
        },
        data() {
            return {
              message: '',
              output: '',
              seen: false,
              confirmacion:''
            };
        },
        methods: {
            formSubmit(e) {
                    e.preventDefault();
                    this.seen= true
                    this.confirmacion = ""
                    if (this.message && this.message != "") {
                        let currentObj = this;
                        this.axios.post('https://us-central1-infinityprojects-c0bc3.cloudfunctions.net/confessionDiscord', {
                            message: this.message.trim()
                        })
                        .then(function (response) {
                            currentObj.confirmacion = "Tu Confesión ya esta dando de que hablar.";

                        })
                        .catch(function (error) {
                            currentObj.confirmacion = "Se produjo un error, al intentar enviar tu confesión. Intentalo más tarde.";
                        });
                        return true;
                    }

                    if (!this.message || this.message == "") {

                        this.confirmacion = "La Confesión no puede ir vacia. Buen Intento Crack!";
                    }

            }
        }
    }
</script>