<template>
    <div>
        <v-col class="d-flex justify-center align-center">
            <v-card class="pa-4">
                <div class="text-center">
                    <v-avatar size="100" color="indigo lighten-4">
                        <v-icon size="40" color="indigo">mdi-account</v-icon>
                    </v-avatar>
                </div>
                <v-form @submit.prevent="submitHandler" ref="form">
                    <v-card-text>
                        <v-text-field v-model="dataUser.userName" :rules="userNameRules" type="UserName" label="UserName"
                            placeholder="UserName" prepend-inner-icon="mdi-account" required />
                        <v-text-field v-model="dataUser.password" :rules="passwordRules"
                            :type="passwordShow ? 'text' : 'password'" label="Password" placeholder="Password"
                            prepend-inner-icon="mdi-key" :append-icon="passwordShow ? 'mdi-eye' : 'mdi-eye-off'"
                            @click:append="passwordShow = !passwordShow" required />

                    </v-card-text>
                    <v-card-actions class="justify-center">
                        <v-btn :loading="loading" type="submit" color="indigo" @click="goToRegisterPage">
                            <span class="white--text px-8">Register</span>
                        </v-btn>
                    </v-card-actions>
                    <v-card-actions class="justify-center">
                        <v-btn :loading="loading" type="submit" color="indigo">
                            <span class="white--text px-8">Login</span>
                        </v-btn>
                    </v-card-actions>
                </v-form>
            </v-card>
        </v-col>
    </div>
</template>

<script>
export default {
    data: () => {
        return {
            dataUser: {
                userName: '',
                password: '',

            },
            passwordShow: false,
            userNameRules: [
                v => !!v || 'UserName is required',
            ],
            passwordRules: [
                v => !!v || 'Password is required',
                v => (v && v.length >= 6) || 'Password must be 6  characters or more!',
            ],
        }
    },
    methods: {
        submitHandler() {
            if (this.$refs.form.validate()) {
                this.axios.post('http://localhost:3000/api/login', this.dataUser).then((res) => {
                    console.log(res)
                }).catch((err) => {
                    console.log('err :', err)
                })
                console.log('email', this.email)
                console.log('password', this.password)
            }
        },
        goToRegisterPage() {
            this.$router.push("/register");
        },
    }

}
</script>

<style></style>