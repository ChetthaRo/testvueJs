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
                        <v-text-field v-model="dataUser.username" :rules="usernameRules" type="username" label="username"
                            placeholder="username" prepend-inner-icon="mdi-account" required />
                        <v-text-field v-model="dataUser.password" :rules="passwordRules"
                            :type="passwordShow ? 'text' : 'password'" label="Password" placeholder="Password"
                            prepend-inner-icon="mdi-key" :append-icon="passwordShow ? 'mdi-eye' : 'mdi-eye-off'"
                            @click:append="passwordShow = !passwordShow" required />
                        <v-text-field v-model="dataUser.email" :rules="emailRules" type="email" label="Email"
                            placeholder="Email" prepend-inner-icon="mdi-account" required />

                    </v-card-text>
                    <v-card-actions class="justify-center">
                        <v-btn :loading="loading" type="submit" color="indigo">
                            <span class="white--text px-8">Register</span>
                        </v-btn>
                    </v-card-actions>

                </v-form>
            </v-card>
        </v-col>
    </div>
</template>

<script>
import Swal from 'sweetalert2'
export default {

    data: () => {
        return {
            dataUser: { username: '', password: '', email: '', },

            usernameRules: [
                v => !!v || 'E-mail is required',
            ],
            passwordShow: false,

            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
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
                console.log(this.dataUser)
                this.axios.post('http://localhost:3000/api/register', this.dataUser).then((res) => {
                    console.log("res: ", res)
                    Swal.fire({
                        title: "Register Success !",
                        text: "Pleas contact Admin",
                        icon: "success"
                    }).then((result) => {
                        if (result.isConfirmed) {
                            this.$router.push("/login");
                        }
                    })
                }).catch((err) => {
                    console.log('err :', err)
                    Swal.fire({
                        title: "Error !",
                        text: "Register Fail",
                        icon: "error"
                    })
                })
            } else return Swal.fire({
                title: "Error !",
                text: "กรุณากรอกข้อมูลให้ครบถ้วน",
                icon: "error"
            })
        }
    }
}
</script>

<style></style>