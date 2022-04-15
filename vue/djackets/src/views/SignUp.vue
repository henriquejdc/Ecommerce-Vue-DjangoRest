<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Sign Up</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Username</label>
                        <div class="control">
                            <input type="text" class="input" v-model="username">
                        </div>
                    </div>

                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password">
                        </div>
                    </div>

                    <div class="field">
                        <label>Repeat password</label>
                        <div class="control">
                            <input type="password" class="input" v-model="password2">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark">Sign up</button>
                        </div>
                    </div>

                    <hr>

                    Or <router-link to="/log-in">click here</router-link> to log in!
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            password: '',
            password2: '',
            errors: []
        }
    },
    methods: {
        submitForm() {
        this.errors = []
        let formData = {}

        if (this.username === '') {
            this.erros.push('The username is missing')
        } 

        if (this.password === '') {
            this.erros.push('The password is too short')
        } 

        if (this.password !== this.password2) {
            this.erros.push('The passwords doesnt\'t match')
        } 

        if (!this.errors.length) {
            console.log(this.errors.length)
            formData = {
                username: this.username,
                password: this.password
            }
            console.log(formData)
        }
        console.log(formData)

        axios
            .post("/api/v1/users/", formData)
            .then(response => {
                toast({
                    message: 'Account created, please log in!',
                    type: 'is-success',
                    dismissible: true,
                    pauseOnHover: true,
                    duration: 2000,
                    position: 'bottom-right',
                })

                this.$router.push('/log-in')
            })
            .catch(error => {                
                if (error.response) {
                    for (const property in error.response.data) {
                        this.errors.push(`${property}: ${error.response.data[property]}`)
                    }
                    console.log(JSON.stringify(error.response.data))
                } else if (error.message) {
                    this.errors.push('Something went wrong. Please try again')
                    
                    console.log(JSON.stringify(error))
                }
            })
        
        }
    }
}
</script>