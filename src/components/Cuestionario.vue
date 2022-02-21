<template>
  <div class="caja-de-preguntas">
    <v-card class="mx-auto" max-width="500">
      <v-card-title>
        {{ preguntaActual.question }}
      </v-card-title>
      <v-list>
        <v-list-item-group>
          <v-list-item
            v-for="(respuesta, index) in respuestasAleatorias"
            :key="index"
            @click.prevent="seleccionarRespuesta(index)"
            :class="categorizarRespuesta(index)"
          >
            {{ respuesta }}
          </v-list-item>
        </v-list-item-group>
      </v-list>

      <v-card-actions>
        <div class="card-footer">
          <template v-if="!(ultimaPregunta && respondido)">
            <v-btn
              depressed
              color="primary"
              @click="submitRespuesta"
              :disabled="indexSeleccionado === null || respondido"
            >
              Responder
            </v-btn>

            <v-btn
              depressed
              color="success"
              @click="siguiente"
              :disabled="indexSeleccionado === null || noRespondido"
            >
              Siguiente pregunta
            </v-btn>
          </template>
          <v-btn v-else @click="$emit('terminar')"> Mostrar resultados </v-btn>
        </div>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    preguntaActual: Object,
    siguiente: Function,
    incrementar: Function,
    ultimaPregunta: Boolean,
  },
  data: function () {
    return {
      indexSeleccionado: null,
      indexCorrecto: null,
      respuestasAleatorias: [],
      respondido: false,
      noRespondido: true,
    };
  },
  computed: {
    respuestas() {
      let respuestas = [...this.preguntaActual.incorrect_answers];
      respuestas.push(this.preguntaActual.correct_answer);
      return respuestas;
    },
  },
  watch: {
    preguntaActual: {
      immediate: true,
      handler() {
        this.indexSeleccionado = null;
        this.respondido = false;
        this.noRespondido = true;
        this.randomizarRespuestas();
      },
    },
  },
  methods: {
    seleccionarRespuesta(index) {
      this.indexSeleccionado = index;
    },
    submitRespuesta() {
      let esCorrecto = false;
      if (this.indexSeleccionado === this.indexCorrecto) {
        esCorrecto = true;
      }
      this.respondido = true;
      this.noRespondido = false;
      this.incrementar(esCorrecto);
      this.$emit("submitPregunta", {
        correct: this.respuestasAleatorias[this.indexCorrecto],
        selected: this.respuestasAleatorias[this.indexSeleccionado],
      });
    },
    randomizarRespuestas() {
      let respuestas = [
        ...this.preguntaActual.incorrect_answers,
        this.preguntaActual.correct_answer,
      ];
      this.respuestasAleatorias = _.shuffle(respuestas);
      this.indexCorrecto = this.respuestasAleatorias.indexOf(
        this.preguntaActual.correct_answer
      );
    },
    categorizarRespuesta(index) {
      let claseRespuesta = "";
      if (!this.respondido && this.indexSeleccionado === index) {
        claseRespuesta = "seleccionada";
      } else if (this.respondido && this.indexCorrecto === index) {
        claseRespuesta = "correcto";
      } else if (
        this.respondido &&
        this.indexSeleccionado === index &&
        this.indexCorrecto !== index
      ) {
        claseRespuesta = "incorrecto";
      }
      return claseRespuesta;
    },
  },
};
</script>

<style scoped>
.card-footer {
  text-align: center;
  display: flex;
  justify-content: center;
}

.seleccionada {
  background-color: lightblue;
}

.correcto {
  background-color: green;
}

.incorrecto {
  background-color: red;
}
</style>
