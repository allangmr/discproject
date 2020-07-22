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
                            <textarea class="form-control" type="text" v-model="message" name="confession" placeholder="Escribe tu Confesión..." required=""></textarea>
                            <div class="form-button">
                                <button id="submit" type="submit" class="ibtn">Enviar</button>
                            </div>
                        </form>
                        <strong>Output:</strong>
                        <pre>
                        {{output}}
                        </pre>
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
            console.log('Component mounted.')
        },
        data() {
            return {
              message: '',
              errors: [],
              output: ''

            };
        },
        methods: {
            formSubmit(e) {
                e.preventDefault();
                 if (this.message) {
                    let currentObj = this;
                    this.axios.post('https://us-central1-infinityprojects-c0bc3.cloudfunctions.net/confessionDiscord', {
                        message: this.message
                    })
                    .then(function (response) {
                        currentObj.output = response.data;
                    })
                    .catch(function (error) {
                        currentObj.output = error;
                    });
                    return true;
                }

                this.errors = [];

                if (!this.message) {
                    this.errors.push('El nombre es obligatorio.');
                }
            }
        }
    }
</script>