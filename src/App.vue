<template>
  <Navbar :logo="logo_src" :alt="alt_name" />
  <router-view/>
  <Footer />
</template>

<script>

import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'

export default {
  components:{
    Navbar,
    Footer
  },
  data(){
    return {
      logo_src: "/img/logo.png",
      app_name: "Make Your Burguer"
    }
  }
    
  
}
</script>

<style>
*{
  font-family: Helvetica;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.main-container{
  margin-bottom: 50px;
  min-height: 650px;

}

h1{
  margin-top: 30px;
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}

</style>
