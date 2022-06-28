<template>
    <h1>Sign In</h1>
    <form @submit.prevent="login()">
        e-mail : <input type="email" name="mail" id="mail" v-model="mail" /><br>
        password : <input type="password" name="pass" id="pass" v-model="pass" /><br>
        <input type="submit" value="Sign In">
    </form>
    <router-link to="/signup">sign up page</router-link>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { signInWithEmailAndPassword, onAuthStateChanged, AuthErrorCodes } from "firebase/auth";
import { auth } from "../firebase/config";
import router from "@/router";

export default defineComponent({
    name: "SignIn",
    data() {
        return {
            mail: "",
            pass: "",
        };
    },
    methods: {
        login() {
            signInWithEmailAndPassword(auth, this.mail, this.pass).then(() => {
                router.push("/mypage");
            }).catch((e) => {
                const errorCode = e.code;
                if (errorCode === AuthErrorCodes.USER_DELETED || errorCode === AuthErrorCodes.INVALID_PASSWORD) {
                    alert("メールアドレスもしくはパスワードが間違っています");
                } else {
                    alert(`Error\n${errorCode}`)
                }
            });
        },
    },
    created() {
        onAuthStateChanged(auth, (u) => {
            if (u) {
                router.push("/mypage");
            }
        });
    },
});
</script>
