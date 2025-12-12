<template >
     <!-- button to toggle sidebar -->
    <button class="menu" @click="toggleSidebar" ><img :src="menu" alt="Menu Toggle" :class="{active: !isHidden}"></button>
    
     <div class="sideBar" :class="{hidden: isHidden}">
        <!-- list for the sidebar navigation with feature to navigate between pages-->
        <ul>
            <li><button class="btnList" @click="currentPage = 'HomePage'">Home</button></li>
            <li><button class="btnList" @click="currentPage = 'EmployeeList'">Employee List</button></li>
            <li><button class="btnList" @click="currentPage = 'Salary'">Salary</button></li>
            <li><button class="btnList" @click="currentPage = 'Attendance'">Attendance</button></li>
        </ul>
    </div>

    <!-- conditional rendering of components based on currentPage -->
    <component :is="currentPage === 'HomePage' ? 'Index' :
                    currentPage === 'EmployeeList' ? 'EmployeeList' :
                    currentPage === 'Salary' ? 'SalaryPage' :
                    currentPage === 'Attendance' ? 'AttendancePage' : 'Index'">
                    </component>
    
</template>

<script>
// import image from assets
import menu from '../assets/menu.png';

// import component pages
import Index from './Index.vue';
import EmployeeList from './EmployeeList.vue';
import SalaryPage from './SalaryPage.vue';
import AttendancePage from './AttendancePage.vue';




export default {
    name: 'SideBar',
    components: {
        Index,
        EmployeeList,
        SalaryPage,
        AttendancePage,
    },
    data() {
        return {
            menu,
            isHidden: true,
            currentPage: 'Home',
        };
    },
    methods: {
        // function to toggle sidebar visibility
        toggleSidebar() {
            this.isHidden = !this.isHidden;
        }
    },
    mounted() {
        // emit the event to parent when sidebar is toggled
        this.$emit('toggle-sidebar', this.toggleSidebar);
    }
}
</script>

<style scoped>
.sideBar{
    position: fixed;
    top: 3vw;
    left: 0;
    width: 11vw;
    height: 96vh;
    background-color: #444;
    color: white;
    padding-top: 2vw;
    transform: translateX(0);
    transition: transform 0.3s ease-in-out;
}

.sideBar.hidden{
    transform: translateX(-100%);
}

ul{
    list-style-type: none;
    padding: 0;
}

.btnList{
    display: block;
    width: 100%;
    width: 11vw;
    height: 2vw;
    margin-top: 1vw;
    border: 1px solid white;
    background-color: #666;
    color: white;
    font-size: 1.5vw;
    cursor: pointer;
}

.btnList:hover{
    background-color: #333;
}

.menu{
    position: fixed;
    background-color: transparent;
    border: none;
    cursor: pointer;
    top: 0.1vw;
    left: 0.1vw;
    width: 3.1vw;
    height: 3vw;
    margin: 0;  
    padding: 0;  
}

.menu img{
    width: 3.1vw;
    height: 3vw;
    transition: transform 0.3s ease-in-out;
}

.menu img.active{
    transform: rotate(90deg);
}

.menu:hover{
    background-color: rgba(255, 255, 255, 0.2);
    width: 3.1vw;
    height: 3.1vw;
}

</style>