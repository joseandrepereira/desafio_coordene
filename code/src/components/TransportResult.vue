<template>
    <div>
        <p><strong>Estas são as melhores alternativas de frete que encontramos para você.</strong></p>
        <p>{{resultCheap}}</p>
    </div>
</template>

<script>

export default {
    name: 'TransportResult',
    props: ['resultCheap', 'resultFast' ]

}
</script>

<style  scoped>
    *{
        background-color: #666;
    }
    p {
        padding: 0 10px;
    border-bottom: 2px solid #000;
    width: 300px;
    }
</style>