<template>
    <div>
        <h1>My Page</h1>
        <p>{{ mail }}</p>
        <button @click="logout">sign out</button>
        <button @click="userDelete">delete user</button>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import { onAuthStateChanged, signOut, deleteUser, User } from "firebase/auth";
import { auth } from '../firebase/config';
import router from "@/router";

export default defineComponent ({
    name: "SignIn",
    data():{
        mail: string | null
        user: User | null
    } {
        return {
            mail: null,
            user: null,
        }
    },
    methods: {
        logout() {
            signOut(auth).then(() => {
                router.push("/")
            });
        },
        userDelete() {
            if (!window.confirm("削除しますか？")) {
                return;
            }
            if (!this.user) {
                throw new Error("user is null");
            }
            deleteUser(this.user).then(() => {
                router.push("/");
            });
        }
    },
    created () {
        onAuthStateChanged(auth, (u) => {
            if (u) {
                this.user = u;
                this.mail = u.email;
            } else {
                router.push("/signin");
            }
        });
    },
});
</script>
