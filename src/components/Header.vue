<template>
  <p>Respuestas Correctas: {{ numCorrectas }} - Respuestas Totales: {{ numTotales }}</p>
</template>

<script>
export default {
    props: [
        'numCorrectas',
        'numTotales'
    ]
}
</script>