<template>
    <div class="container col-xl-10 col-xxl-8 px-4 py-5 ">
        <div class="row align-items-center g-lg-5 py-5">
            <div class="col-md-10 mx-auto col-lg-5">
                <h3 class="text-center py-3"><b>Login Page</b></h3>
                <Form @submit="handleSubmit" :validation-schema="schema" v-slot="{ errors }"
                    class="p-4 p-md-5 border rounded-3 bg-light">
                    <div class="form-floating mb-3">
                        <Field name="nomorHp" type="text" class="form-control" v-model="users.nomor_hp" id="floatingField"
                            placeholder="name@example.com" />
                        <label for="floatingField">Nomor hp</label>
                        <span class="text-danger">{{ errors.nomorHp }}</span>
                    </div>
                    <div class="form-group">

                        <div class="form-floating mb-3">
                            <Field name="password" :type="
                                showPassword ? 'text' : 'password'
                            " class="form-control" v-model="users.password" id="floatingPassword"
                                placeholder="Password" />
                            <label for="floatingPassword">Password</label>
                            <span class="text-danger">{{ errors.password }}</span>
                            <div class="input-group-append text-end">
                                <span @click="hidePassword" class="text-primary">
                                    <i :class="
                                        showPassword
                                            ? 'fa-solid fa-eye-low-vision fa-md'
                                            : 'fa-solid fa-eye fa-md'
                                    "></i>
                                </span>
                            </div>
                        </div>
                    </div>
                    <div class="checkbox mb-3">
                        <label>
                            <Field name="checkbox" type="checkbox" value="remember-me"> Remember me
                            </Field>
                        </label>
                    </div>
                    <button class="w-100 btn btn-lg btn-primary" type="submit">
                        <span v-if="isLoading"><i>sebentar, kami sedang memproses data kamu</i></span>
                        <span v-else>
                            Login
                        </span>
                    </button>
                    <hr class="my-4">
                </Form>
            </div>
        </div>
    </div>
</template>

<script>
import iziToast from 'izitoast'
import Cookies from "js-cookie";
import * as validate from 'yup'
import { Field, Form } from 'vee-validate'
export default {
    data() {
        return {
            users: {
                nomor_hp: '',
                password: '',
            },
            error: [],
            showPassword: false,
            isLoading: false
        }
    },
    components: {
        Field,
        Form,
    },
    computed: {
        schema() {
            return validate.object({
                nomorHp: validate.string().required(),
                password: validate.string().required()
            })
        }
    },
    methods: {
        hidePassword() {
            this.showPassword = !this.showPassword
        },
        handleSubmit() {
            let type = "postData"
            const data = {
                nomor_hp: this.users.nomor_hp,
                password: this.users.password
            }
            let url = [
                "autentikasi/login", data
            ]
            this.$store.dispatch(type, url).then((result) => {
                Cookies.set("token", result.data.token)
                Cookies.set("user", JSON.stringify(result));
                iziToast.success({
                    transitionIn: 'fadeInUp',
                    timeout: 2000,
                    title: 'berhasil',
                    message: "login",
                    position: 'topCenter'
                })
                window.location = '/'
            }).catch((err) => {
                console.log(err);
            })
        },
    },
}
</script>