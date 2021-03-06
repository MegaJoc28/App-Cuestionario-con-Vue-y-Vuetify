<template>
  <div class="caja-de-preguntas">
    <v-card class="mx-auto" max-width="500">
      <div class="card-header">
        <v-card-title style="word-break:normal; font-weight: bold" v-html="preguntaActual.question"></v-card-title>
        <hr>
      </div>
      <v-list>
        <v-list-item-group>
          <v-list-item
            v-for="(respuesta, index) in respuestasAleatorias"
            :key="index"
            @click.prevent="seleccionarRespuesta(index)"
            :class="categorizarRespuesta(index)"
            v-html="respuesta"
          >
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
              :disabled="indexSeleccionado === null || !respondido"
            >
              Siguiente pregunta
            </v-btn>
          </template>
          <v-btn depressed 
          color="info"
          v-else @click="$emit('terminar')"> Mostrar resultados </v-btn>
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
        this.randomizarRespuestas();
      },
    },
  },
  methods: {
    seleccionarRespuesta(index) {
      this.indexSeleccionado = index;
    },
    submitRespuesta() {
      const esCorrecto = this.indexSeleccionado === this.indexCorrecto;
      this.respondido = true;
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
.card-header{
  text-align: center;
  color: black;
}

.card-footer {
  text-align: center;
  display: flex;
  width: 100%;
  justify-content: space-around;
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
