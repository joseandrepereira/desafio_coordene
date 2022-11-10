<template>
  <div id="allElements">
    <div class="title">
      <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-brand class="ml-2">
          <img src="../assets/logo.png" alt="logo.png" height="45" width="45" />
          <b>{{ appName }}</b>
        </b-navbar-brand>
      </b-navbar>
    </div>

    <div class="body">
      <TransportForm
        :shippingData="shippingData"
        v-on:formSubmit="formSubmit"
      />
      <TransportResult
        :resultCheap="cheapResultt" 
        :resultFast="fastResultt" 
        :weight="weight"
      />
    </div>
  </div>
</template>

<script>
import { BNavbar, BNavbarBrand } from "bootstrap-vue";
import TransportForm from "./TransportForm.vue";
import TransportResult from "./TransportResult.vue";

export default {
  name: "BestTransport",

  components: {
    BNavbar,
    BNavbarBrand,
    TransportForm,
    TransportResult,
  },
  data() {
    const appName = "";
    const shippingData = [];
    const weight = null;
    const destiny = null;
    const datas = [];
    const cheapResultt = {
      name: "",
    };
    const fastResultt = {
      name: "",
    };

    return {
      appName,
      shippingData,
      weight,
      destiny,
      datas,
      cheapResultt,
      fastResultt
    };
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    this.appName = "MELHOR FRETE";
    this.getDatas();
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina

    //Método para filtrar as repetições das cidades
    filterRepetition(data) {
      let count = 0;
      for (let index = 0; index < data.length; index++) {
        if (!this.shippingData.includes(data[index].city)) {
          this.shippingData[count] = data[index].city;
          count++;
        }
      }
      this.shippingData.sort();
    },
    //Método para filtrar os dados referentes ao resultado recebido do form
    filterResult(data, city) {
      let listDestiny = [];
      for (let index = 0; index < data.length; index++) {
        if (data[index].city == city) {
          listDestiny.push(data[index]);
          listDestiny[listDestiny.length-1].cost_transport_light = listDestiny[listDestiny.length-1].cost_transport_light.replace("R$ ", '');
          listDestiny[listDestiny.length-1].cost_transport_heavy = listDestiny[listDestiny.length-1].cost_transport_heavy.replace("R$ ", '');
          listDestiny[listDestiny.length-1].lead_time = listDestiny[listDestiny.length-1].lead_time.replace("h", '');
        }
      }
      return listDestiny;
    },
    //Método para dar o GET na API
    async getDatas() {
      const req = await fetch("http://localhost:3000/transport");
      const data = await req.json();

      this.datas = data;
      this.filterRepetition(this.datas);
    },
    //Método que recebe os dados do form por meio do @emit
    formSubmit(data) {
      this.weight = data.data.weight;
      this.destiny = data.data.destiny;

      this.fastResult();
      this.cheapResult(this.weight);
    },
    //Método para calcular o frete mais barato
    cheapResult(weigth) {
      let listFilter = [];
      listFilter = this.filterResult(this.datas, this.destiny);
      let resultCheap = listFilter[0];

      if (weigth > 100) {
        for (let index = 0; index < listFilter.length; index++) {
          if (listFilter[index].cost_transport_heavy < resultCheap.cost_transport_heavy) {
            resultCheap = listFilter[index];
          }
        }
      } else {
        for (let index = 0; index < listFilter.length; index++) {
          if ( listFilter[index].cost_transport_light < resultCheap.cost_transport_light) {
            resultCheap = listFilter[index];
          }
        }
      }
      this.cheapResultt = resultCheap;
    },
    //Método para calcular o frete mais rápido
    fastResult(){
      let listFilterr = [];
      listFilterr = this.filterResult(this.datas, this.destiny);
      let resultFast = listFilterr[0];

      for (let index = 0; index < listFilterr.length; index++) {
        //console.log(listFilterr[index].lead_time);
        if ( Number(resultFast.lead_time) > Number(listFilterr[index].lead_time) ) {
            resultFast = listFilterr[index];          
        }
      }
      this.fastResultt = resultFast;
    }
  },
};
</script>

<style scoped>
.navbar {
  background-color: #93c47d !important;
  border-bottom: 2px solid #000;
  border-left:2px solid #000;
  border-right: 2px solid #000;
  padding: 5px 30px;
  width: 500px;
  height: 100px;
  margin-top: 0px;
}.navbar-brand {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.navbar-brand b {
  margin-left: 15px;
  color: black;
  font-size: larger;
}

#allElements{
  width: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: auto;
  border: 2px solid #000;
}
</style>
