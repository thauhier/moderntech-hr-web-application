<template>
<!-- footer -->
<footer>
    <!-- footer info -->
    <ul class="footer-list1"> 
        <li>{{footerInfo.companyName}}</li>
        <li>{{footerInfo.companyContact}} </li>
        <li>{{footerInfo.companyEmail}}</li>
    </ul>
    <ul class="footer-list2">
        <li>{{footerInfo.companyAdress.street}}<br>
            {{footerInfo.companyAdress.suburb}}<br>
            {{footerInfo.companyAdress.city}}<br>
            {{ footerInfo.companyAdress.postalCode }}
        </li>
    </ul> 
    
    <ul>
        <li>Developer Info:</li>
        <li>{{footerInfo.help}}</li>
        <li>{{footerInfo.copyright}}</li>
    </ul>
</footer>

</template>

<script>
export default {
    name: "Footer",
    // data for footer info pulled from an object
    data(){
        return{
            footerInfo:{
                companyName: "ModernTech",
                companyContact: "076 848 3378",
                companyEmail: "ModernTech@gmail.com",
                companyAdress:{                     
                    street: "2313 bloemfon Street",    /*the adress inside another object */
                    suburb: "The Hague Delft", 
                    city: "Downtown", 
                    postalCode:"7133" 
                },
                help: "mtkellerman@gmail.com",
                copyright: "© 2025",
            }

        }

    }
}

</script>

<style scoped>
footer{
    left: 0;
    bottom: 0;
    width: 100%;
    background-color: #333;
    color: white;
    text-align: center;
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 9.3vh;
}

ul{
    list-style-type: none;
    margin: 0;
    padding: 10px;
    overflow: hidden;
    background-color: #333;
    font-size: 0.8vw;
}

p{
    margin: 10px;
    padding: 0;
    overflow: hidden;
    background-color: #333;
    
}

</style>