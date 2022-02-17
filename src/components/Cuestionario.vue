<template>
  <div class="caja-de-preguntas">
    <v-card class="mx-auto" max-width="500">
      <v-list>
        <h1> {{ preguntaActual.question }} </h1>
        <v-list-item-group v-for="(respuesta, index) in respuestasAleatorias" :key="index" @click.prevent="seleccionarRespuesta(index)" :class="claseRespuesta(index)">
          {{respuesta}}
        </v-list-item-group>
      </v-list>

      <v-btn depressed color="primary" @click="submitRespuesta" :disabled="indexSeleccionado === null || respondido">
      Responder
      </v-btn>

      <v-btn depressed color="success" @click ="siguiente" :disabled="indexSeleccionado === null">
      Siguiente pregunta
      </v-btn>

    </v-card>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        preguntaActual: Object,
        siguiente: Function,
        incrementar: Function
    },
    data: function(){
        return{
            indexSeleccionado: null,
            indexCorrecto : null,
            respuestasAleatorias: [],
            respondido: false
        }
    },
    computed:{
        respuestas(){
            let respuestas = [...this.preguntaActual.incorrect_answers]
            respuestas.push(this.preguntaActual.correct_answer)
            return respuestas
        }
    },
    watch: {
        preguntaActual: {
            immediate: true,
            handler() {
                this.indexSeleccionado = null
                this.respondido = false
                this.randomizarRespuestas()
            }
        }
    },
    methods: {
      seleccionarRespuesta(index){
        this.indexSeleccionado = index
      },
      submitRespuesta(){
      let esCorrecto = false
        if(this.indexSeleccionado === this.indexCorrecto){
          esCorrecto = true
        }
          this.respondido = true
          this.incrementar(esCorrecto)
      },
      randomizarRespuestas(){
        let respuestas = [...this.preguntaActual.incorrect_answers, this.preguntaActual.correct_answer]
        this.respuestasAleatorias = _.shuffle(respuestas)
        this.indexCorrecto = this.respuestasAleatorias.indexOf(this.preguntaActual.correct_answer)
      },
      claseRespuesta(index){
        let claseRespuesta = ''
        if(!this.respondido && this.indexSeleccionado === index){
          claseRespuesta = 'seleccionada'
        } else if (this.respondido && this.correct_answer === index){
          claseRespuesta = 'correcta'
        }else if (this.respondido && this.indexSeleccionado === index && this.indexCorrecto !== index){
          claseRespuesta = 'incorrecto'
        }
        return claseRespuesta
      }
    }
}
</script>

<style scoped>
  .seleccionada{
    background-color: lightblue;
  }

  .correcta {
    background-color: green;
  }

  .incorrecto {
    background-color: red;
  }
</style>
