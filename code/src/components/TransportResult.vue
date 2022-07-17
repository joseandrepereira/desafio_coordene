<template>
  <div>
    <p id="title">
      <strong
        >Estas são as melhores alternativas de frete que encontramos para
        você.</strong
      >
    </p>
    <div class="result" id="resultCheap">
      <p v-show="resultCheap.name"> Frete mais barato: <strong>Transportadora {{ cName }} - R$ {{ cCost }} - {{ cTime }}h</strong>
      </p>
    </div>
    <div class="result" id="resultFast">
      <p v-show="resultCheap.name">
        Frete mais rápido:
        <strong>
          Transportador {{ fName }} - R$ {{ fCost }} - {{ fTime }}h</strong>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "TransportResult",
  props: ["resultCheap", "resultFast", "weight"],
  data() {
    const fName = "";
    const fCost = null;
    const fTime = null;
    const cName = "";
    const cCost = null;
    const cTime = null;

    return {
      fName,
      fCost,
      fTime,
      cName,
      cCost,
      cTime,
    };
  },
  created() {},
  methods: {
    setStates() {
      this.fName = this.resultFast.name;
      this.fTime = this.resultFast.lead_time;
      this.fCost = this.calculateValue(
        this.weight,
        this.resultFast.cost_transport_light,
        this.resultFast.cost_transport_heavy
      );
      this.cName = this.resultCheap.name;
      this.cTime = this.resultCheap.lead_time;
      this.cCost = this.calculateValue(
        this.weight,
        this.resultCheap.cost_transport_light,
        this.resultCheap.cost_transport_heavy
      );
    },
    //Método para calcular o valor do frete
    calculateValue(weight, costLight, costHeavy) {
      if (weight > 100) {
        return weight * costHeavy;
      } else {
        return weight * costLight;
      }
    },
  },
  mounted() {
    this.setStates();
  },
  updated() {
    this.setStates();
  },
};
</script>

<style  scoped>
#title {
  padding: 0 10px;
  border-bottom: 2px solid #000;
  width: 400px;
}

.result{
    display: flex;
    text-align: center;
    justify-content: center;
    border: 2px dashed #93c47d;
    border-radius: 10px;
    padding: 3px;
    margin-bottom: 10px;
}
</style>