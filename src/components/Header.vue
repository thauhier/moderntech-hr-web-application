<template>
<!-- header -->
<header>
    <!-- header info -->
     <h1>MODERN<span>TECH</span></h1>

    <SideBar v-if="$emit('toggle-sidebar')" :menu="menu" />

     <!-- logout button -->
     <button class="logout" @click="$emit('logout')">Logout</button>
</header>

</template>

<script>

import SideBar from './SideBar.vue';
import menu from '../assets/menu.png';

export default {
    name: 'Header',
    components: {
        SideBar,
    },
    emits: ['logout', 'toggle-sidebar'],
    data() {
        return {
            menu,
            sidebarVisible: false,
        };
    },
    methods: {
        active(){
            this.$emit('toggle-sidebar');
        }
    }

}

</script>

<style scoped>
header{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 3vw;
    background-color: #333;
    color: white;
    font-size: 2vw;
    margin: 0;
    margin-top: 0;
    padding: 0;
    position: fixed;
    top: 0;
    left:0;
    overflow:hidden;
}

span{
    color: blueviolet;
}


.logout{
    position: absolute;
    background-color: crimson;
    border: none;
    color: white;
    font-weight: bold;
    padding: 0.2vw 0.5vw;
    border-radius: 1vw;
    cursor: pointer;
    font-size: 1.5vw;
    top: 0.3vw;
    right: 1vw;
    margin-left: 53vw;
    height: 2.3vw;
    width: 6.5vw;
    transition-duration: .25s;
}

.logout:active{
    transform: scale(0.95);
}

.logout:hover{
    background-color: red;
}


</style>