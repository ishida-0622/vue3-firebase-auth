<template>
    <div>
        <h1>Home</h1>
        <router-link to="/signin">sign in page</router-link><br />
        <router-link to="/signup">sign up page</router-link><br />
        <p>
            送信されたメールアドレスは保存され、制作者が閲覧できる状態になります。<br />
            悪用するつもりはありませんが、ダミーのものを使用することを推奨します。<br />
            例 : dummy001@example.com
        </p>
    </div>
    <router-view />
</template>
