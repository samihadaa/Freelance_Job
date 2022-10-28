<template>
    <div class="container" id="app">
        <div id="logo">
            <h1 class="logo">Enregistrement</h1>
            <div class="CTA">
                <h1>Stage</h1>
            </div>
        </div>
        <div class="leftbox">
            <nav>
                <a id="profile" class="active"><i class="fa fa-user"></i></a>
                <a id="privacy"><i class="fa fa-tasks"></i></a>
            </nav>
        </div>
        <div class="rightbox">
            <div class="profile">
                <h1>Ajouter un Etudiant</h1>

                <h2>Nom complet</h2>

                <input class="form-control" placeholder="example: fullname" v-model.trim="v$.Name.$model"
                    :class="{ 'is-invalid': v$.Name.$error, 'is-valid': !v$.Name.$invalid }" type="text">
                <span class="invalid-feedback text-danger pt-0"
                    v-if="v$.Name.$error">{{ v$.Name.$errors[0].$message }}</span>

                <h2>Email</h2>
                <div>
                    <input class="form-control" placeholder="example@example.com" v-model.trim="v$.Email.$model"
                        :class="{ 'is-invalid': v$.Email.$error, 'is-valid': !v$.Email.$invalid }" type="text">
                    <span class="invalid-feedback text-danger"
                        v-if="v$.Email.$error">{{ v$.Email.$errors[0].$message }}</span>
                </div>
                <h2>Date de demarrage</h2>
                <input class="form-control" placeholder="" v-model.trim="v$.Date.$model"
                    :class="{ 'is-invalid': v$.Date.$error, 'is-valid': !v$.Date.$invalid }" type="Date">
                <span class="invalid-feedback text-danger" v-if="v$.Date.$error">{{ v$.Date.$errors[0].$message }}</span>

                <h2>
                    <button @click="submitForm" class='btn btn-success btn-submit'>Ajouter</button>
                </h2>
            </div>

            <div class="privacy noshow">
                <h1>Liste des Etudiants</h1>
                <p></p>
            </div>
        </div>
    </div>

</template>

<script>
import { useVuelidate } from '@vuelidate/core'
import { required, minLength, email, } from '@vuelidate/validators'
import { reactive, computed } from 'vue'
export default {
    setup() {
        const state = reactive({
            Name: '',
            Email: '',
            Date: null,
        })
        const MustnotBeYahoo = (value) => !value.includes('yahoo.fr')/* "Yahoo.fr" is not accepted*/
        const MustnotBeMarc = (value) => !value.includes('marc') /* the name "marc" is not accepted*/
        const MustBefuturDate = (value) => value > new Date().toISOString() /* Date must be in the futur*/
        const rules = computed(() => {
            return {
                Name: {
                    required,
                    minLength: minLength(3),
                    MustnotBeMarc,
                },
                Email: {
                    required,
                    email,
                    MustnotBeYahoo,
                },
                Date: {
                    required,
                    MustBefuturDate,
                }
            }
        })
        const v$ = useVuelidate(rules, state)
        return {
            state,
            v$,
        }
    },

    methods: {
        submitForm() {
            this.v$.$validate()
            if (!this.v$.$error) {
                alert('Form successfully submitted')
            }
        }
    }
}
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css?family=Nunito:400,900|Montserrat|Roboto");

body {
    background: linear-gradient(to right, #3FB6A8, #7ED386);
}

span {
    color: red;
}

.container {
    background: #FFFFFF;
    width: 540px;
    min-height: 457px;
    ;
    margin: 0 auto;
    position: relative;
    margin-top: 10%;
    box-shadow: 2px 5px 20px rgba(119, 119, 119, 0.5);
}

.logo {
    float: right;
    margin-right: 12px;
    margin-top: 12px;
    font-family: "Nunito Sans", sans-serif;
    color: #3DBB3D;
    font-weight: 900;
    font-size: 1.5em;
    letter-spacing: 1px;
}

.CTA {
    width: 80px;
    height: 40px;
    right: -20px;
    bottom: 0;
    margin-bottom: 90px;
    position: absolute;
    z-index: 1;
    background: #7ED386;
    font-size: 1em;
    transform: rotate(-90deg);
    transition: all 0.5s ease-in-out;
    cursor: pointer;
}

.CTA h1 {
    color: #FFFFFF;
    margin-top: 10px;
    margin-left: 9px;
}

.CTA:hover {
    background: #3FB6A8;
    transform: scale(1.1);
}

.leftbox {
    float: left;
    top: -5%;
    left: 5%;
    position: absolute;
    width: 15%;
    height: 110%;
    background: #7ED386;
    box-shadow: 3px 3px 10px rgba(119, 119, 119, 0.5);
}

nav a {
    list-style: none;
    padding: 35px;
    color: #FFFFFF;
    font-size: 1.1em;
    display: block;
    transition: all 0.3s ease-in-out;
}

nav a:hover {
    color: #3FB6A8;
    transform: scale(1.2);
    cursor: pointer;
}

nav a:first-child {
    margin-top: 7px;
}

.active {
    color: #3FB6A8;
}

.rightbox {
    float: right;
    width: 60%;
    height: 100%;
}

.profile,
.payment,
.subscription,
.privacy,
.settings {
    transition: opacity 0.5s ease-in;
    position: absolute;
    width: 70%;
}

h1 {
    font-family: "Montserrat", sans-serif;
    color: #7ED386;
    font-size: 1em;
    margin-top: 40px;
    margin-bottom: 35px;
}

h2 {
    color: #777777;
    font-family: "Roboto", sans-serif;
    width: 80%;
    text-transform: uppercase;
    font-size: 8px;
    letter-spacing: 1px;
    margin-left: 2px;
}

p {
    border-width: 1px;
    border-style: solid;
    border-image: linear-gradient(to right, #3FB6A8, rgba(126, 211, 134, 0.5)) 1 0%;
    border-top: 0;
    width: 80%;
    font-family: "Montserrat", sans-serif;
    font-size: 0.7em;
    padding: 7px 0;
    color: #070707;
}

span {
    font-size: 0.5em;
    color: #777777;
}


.btn {
    float: right;
    font-family: "Roboto", sans-serif;
    text-transform: uppercase;
    font-size: 10px;
    border: none;
    color: #3FB6A8;
    margin-top: 24px;
    padding: 13px;
    border: 1px solid #7ed386;
    text-decoration: none;

}

.btn:hover {
    color: white;
    font-weight: 900;
    background-color: #7ed386;
}

input {
    /* border: 1px solid #dddddd;
  font-family: "Roboto", sans-serif;
  padding: 2px;
  margin: 0;*/

    border-width: 1px;
    border-style: solid;
    border-image: linear-gradient(to right, #3FB6A8, rgba(126, 211, 134, 0.5)) 1 0%;
    border-top: 0;
    width: 80%;
    font-family: "Montserrat", sans-serif;
    font-size: 0.7em;
    padding: 7px 0;
    color: #070707;
    margin-bottom: 14px;




}

.privacy h2 {
    margin-top: 25px;
}

.settings h2 {
    margin-top: 25px;
}

.noshow {
    opacity: 0;
}

footer {
    position: absolute;
    width: 20%;
    bottom: 0;
    right: -20px;
    text-align: right;
    font-size: 0.8em;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-family: "Roboto", sans-serif;
}

footer p {
    border: none;
    padding: 0;
}

footer a {
    color: #ffffff;
    text-decoration: none;
}

footer a:hover {
    color: #7d7d7d;
}
</style>