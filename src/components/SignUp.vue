<template>
    <h1>Sign Up</h1>
    <form @submit.prevent="signup()">
        e-mail : <input type="email" name="mail" id="mail" v-model="mail" required /><br>
        password : <input type="password" name="pass" id="pass" v-model="pass" minlength="6" required /><br>
        <input type="submit" value="Sign Up"><br>
        <router-link to="/signin">sign in page</router-link>
    </form>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import {
    createUserWithEmailAndPassword,
    onAuthStateChanged,
    AuthErrorCodes
} from "firebase/auth";
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
        signup() {
            createUserWithEmailAndPassword(auth, this.mail, this.pass).then(() => {
                router.push("/mypage");
            }).catch((e) => {
                switch (e.code) {
                    case AuthErrorCodes.EMAIL_EXISTS:
                        alert("そのメールアドレスは使用されています");
                        break;
                    case AuthErrorCodes.WEAK_PASSWORD:
                        alert("パスワードは6文字以上で入力してください");
                        break;
                    case AuthErrorCodes.INVALID_EMAIL:
                        alert("メールアドレスの形式が正しくありません");
                        break;
                    default:
                        alert(e);
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
