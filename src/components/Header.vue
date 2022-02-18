<template>
  <v-tabs>
    <v-tab disabled>Respuestas Correctas: {{ numCorrectas }}</v-tab>
    <v-tab disabled>Respuestas Totales: {{ numTotales }}</v-tab>
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