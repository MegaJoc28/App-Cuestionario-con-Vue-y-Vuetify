<template>
  <v-simple-table>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">Número de pregunta</th>
          <th class="text-left">Respuesta seleccionada</th>
          <th class="text-left">Respuesta correcta</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(respuesta, index) in sumario" :key="index">
          <td class="text-left"> {{ index+1 }}</td>
          <td class="text-left">{{ respuesta.selected }}</td>
          <td class="text-left">{{ respuesta.correct }}</td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>

<script>
export default {
  props: ["sumario"],

  }
</script>
