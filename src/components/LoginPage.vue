<template>
<!-- login form inputs -->
<div class="login-container">
    <h2>Login</h2>
    <form @submit.prevent="login">
        <input v-model="username" type="text" placeholder="Enter Username" required>
        <input v-model="password" type="password" placeholder="Enter Password" required>
        <button type="submit">Login</button>

        <!-- error message for failed login -->
        <p v-if="error" class="error">{{error}}</p>
    </form>
</div>

</template>

<script>


export default {
    name:"LoginPage",
  data(){
    return{
        username: "",
        password: "",
        error:""
    }
  },
  methods:{
    login(){
        // login in authentication
        if(this.username === "admin" && this.password === "admin1234"){
            const userData = {
                username: this.username
            };
            this.$emit('login-success', userData);
        }else{
            this.error = "invalid username or password"
        }
    }
  }
}

</script>

<style scoped>

.login-container{
    display: flex;
    flex-direction: column;
    position: absolute;
    border-radius: 20px;
    background-color: grey;
    width:50vw;
    height:30vw;
    top: 25%;
    left: 25%;
    align-items: center;
    border: 2px solid blueviolet;
}

h2{
    margin-top:2vw;
    font-size:4vw ;
}

input{
    position: relative;
    border-radius: 4px;
    margin:5px;
    border:none;
    width:20vw;
    height:2.5vw;
    font-size: 1.2vw;
    padding-left: 5px;
}

button{
    position: absolute;
    bottom: 2vw;
    left: 40%;
    width: 10vw;
    height: 3vw;
    border-radius: 10px;
    border:none;
    background-color: rgb(100, 100, 250);
}

button:hover{
    background-color: rgb(68, 68, 206);
    color:white;
    cursor: pointer;
}

.error{
    color:red;
    font-size: 1.5vw;
    margin-top: 1vw;
}

</style>