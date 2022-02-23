<template>
  <p><b>Correctas: {{ numCorrectas }} - Respuestas Totales: {{ numTotales }}</b></p>
</template>

<script>
export default {
    props: [
        'numCorrectas',
        'numTotales'
    ]
}
</script>