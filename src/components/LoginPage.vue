<template>
<div class="container" style="text-align: center;">
    
    <div class="loginContainer">
    <h1 class="mb-3">Mongoal Vue</h1>
    <div class="row">
        <div class="col-md-6">
            <img style="height: 250px; padding-left: 5px; padding-right: 5px;" src="@/assets/goals.png" alt="A person infront of their goals">
        </div>
    <div class="col-md-6">
        <form v-on:submit.prevent="emitLogin()">
            <div class="form-group">
                <label>Username </label>
                <input v-model="usernameInput" class="form-control" type="text" name="username" placeholder="Username">
            </div>
            <div class="form-group">
                <label>Password </label>
                <input v-model="passwordInput" class="form-control" type="password" name="password" placeholder="Password">
            </div>
            <button class="btn btn-success">Login</button>
        </form>
    </div>
    </div>
    </div>
</div>


</template>

<script>
export default {
    name: 'LoginPage',
    data(){
        return {
            usernameInput: '',
            passwordInput: ''
        }
    },
    methods: {
        emitLogin(){
            this.$emit('login-user', this.usernameInput, this.passwordInput);    
        }
    }
    
}
</script>

<style>
    .loginContainer {
        margin-top: 20%;
        padding: 20px;    
        height: 33%;
        background-color: #FFF;
        border-radius: 10px;
    }
</style>
