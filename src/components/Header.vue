<template>
  <v-tabs>
    <v-tab><b>Respuestas Correctas: {{ numCorrectas }}</b></v-tab>
    <v-tab><b>Respuestas Totales: {{ numTotales }}</b></v-tab>
  </v-tabs>
</template>

<script>
export default {
    props: [
        'numCorrectas',
        'numTotales'
    ]
}
</script>