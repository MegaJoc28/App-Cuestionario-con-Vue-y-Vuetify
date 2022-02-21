<template>
    <h1>Hola</h1>
</template>

<script>
export default {
    props: [
        'sumario'
    ],

    mounted(){
        console.log(this.sumario)
    }
}
</script>