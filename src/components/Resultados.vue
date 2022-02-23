<template>
  <div>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left"><b>Número de pregunta</b></th>
            <th class="text-left"><b>Respuesta seleccionada</b></th>
            <th class="text-left"><b>Respuesta correcta</b></th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(respuesta, index) in sumario"
            :key="index"
            :class="
              respuesta.selected == respuesta.correct
                ? 'respuesta-correcta'
                : 'respuesta-incorrecta'
            "
          >
            <td class="text-left">{{ index + 1 }}</td>
            <td class="text-left" v-html="respuesta.selected"></td>
            <td class="text-left" v-html="respuesta.correct"></td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
    <v-btn depressed color="blue-grey" onclick="location.reload()">Reintentar</v-btn>
  </div>
</template>

<script>
export default {
  props: ["sumario"],
};
</script>

<style scoped>

.v-btn{
  margin-top: 45px;
}

.respuesta-correcta {
  background-color: #26e2a3;
}

.respuesta-incorrecta {
  background-color: #e97b7b;
}
</style>