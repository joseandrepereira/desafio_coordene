<template>
  <div>
    <p><strong>Como é o frete que você precisa?</strong></p>
    <div>
      <form id="transport-form">
        <div class="input-container">
          <label for="destino">Destino</label>
          <select name="destino" id="destino" v-model="destino">
            <option value="">Selecione aqui o destino do frete</option>
            <option  value="">Selecione aqui o destino do frete</option>
          </select>
        </div>
        <div class="input-container">
          <label for="peso">Peso</label>
          <input type="text" id="peso" name="name" v-model="peso" placeholder="Insira aqui o peso da carga em Kg" />
        </div>
        <div class="input-container">
            <input type="submit" class="submit-btn" value="Analisar">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "TransportForm",
  props: ['shippingData'],
  data(){
    return {
        destinations: [],
    }
  },
  mounted(){
  //  this.destinations = this.shippingData;
   // console.log(this.shippingData);
  }
};
</script>

<style scoped>

    p {
    padding: 0 15px;
    border-bottom: 2px solid #000;
    width: 300px;
    }

    #transport-form {
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
        padding: 0 20px;

    }

    .submit-btn {
       background-color: #93c47d;
       border-radius: 10px;
       width: 130px;
       font-weight: bold;
       padding: 5px 10px;
       margin: 0 auto;
       cursor: pointer;
       transition: .5s;
    }

    .submit-btn:hover{
        background-color: #244517;
        color: #fafafa;
        border-color: #000000;
    }

    label {
        font-weight:400;
    }

    input, select {
        padding: 5px 10px;
        width: 350px;
    }

</style>