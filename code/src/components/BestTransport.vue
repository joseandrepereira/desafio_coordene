<template>
  <div>
    <div class="title">
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand class="ml-2">
          <img src="../assets/logo.png" alt="logo.png" height="45" width="45" />
          <b>{{ appName }}</b>
        </b-navbar-brand>
      </b-navbar>
    </div>

    <div class="body">
      <TransportForm :shippingData="shippingData" />
      <p>\\\\\\\\\\ RESULTADOS ////////////</p>
    </div>
  </div>
</template>

<script>
import { BNavbar, BNavbarBrand } from "bootstrap-vue";
import TransportForm from "./TransportForm.vue";

export default {
  name: "BestTransport",

  components: {
    BNavbar,
    BNavbarBrand,
    TransportForm,
  },
  data() {
    const appName = "";
    const shippingData = [];

    return {
      appName,
      shippingData
    };
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    this.appName = "MELHOR FRETE";
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina

    //Método para filtrar as repetições das cidades
    transfer(data){
      let count = 0;
      for (let index = 0; index < data.length; index++) {
        if(!this.shippingData.includes(data[index].city)){ 
               this.shippingData[count] = data[index].city;
               count ++;
        }   
      }
      this.shippingData.sort();
    },
    async getDatas(){
      const req = await fetch("http://localhost:3000/transport");
      const data = await req.json();

      this.transfer(data);

       console.log(this.shippingData);
    }
  },
  mounted(){
    this.getDatas()
  }
};
</script>

<style scoped>
.title .navbar {
  background-color: #93c47d !important;
  border-bottom: 2px solid #000;
  padding: 15px 30px;
}

.title .navbar-brand {
  background-color: rgba(0, 0, 0, 0.519);

  margin-left: 20px;

  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.title .navbar-brand b {
  margin-left: 15px;
  color: black;
  font-size: larger;
}

.body {
  display: flex;
  justify-content:center;
  align-items: center;
  flex-direction: column;
}
</style>
