<template>
  <div id="app">
    <!-- login in to be the first to pop up -->
    <LoginPage v-if="!isLoggedIn" @login-success="onLoginSuccess"/>
     
    <div v-else>

  <div class="main-content">
    <!-- main application view after login -->
    <Header @logout="logout"/>
    <SideBar/>
  </div>

  
    <!-- <Footer/> -->
     <Footer/>
    </div>
  </div>
</template>

<script>
import LoginPage from './components/LoginPage.vue';
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';
import SideBar from './components/SideBar.vue';

export default {
  name: 'App',
  components: {
    LoginPage,
    Footer,
    Header,
    SideBar,
  },
  data(){
    return{
      isLoggedIn: false,
      user: {},
    }
  },
  methods:{
    // function for succesfull login
    onLoginSuccess(userData){
      this.isLoggedIn = true,
      this.user = userData;
    },
    // function for loging out
    logout(){ 
      this.isLoggedIn = false;
      this.user ={}
    }
  }
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 0;
  display:flex;
  flex-direction: column;
}

.main-content{
  flex: 1;
  padding:2vw;
}

</style>
