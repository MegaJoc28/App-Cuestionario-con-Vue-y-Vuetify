<template>
  <v-app id="app">
    <v-main>
      <Header
      :numCorrectas="numCorrectas"
      :numTotales="numTotales"/>
      <v-container>
        <v-row>
          <v-col sm="6" offset="3">
          <Cuestionario
            v-if="preguntas.length && !mostrarSumario"
            :preguntaActual="preguntas[index]"
            :siguiente ="siguiente"
            :incrementar="incrementar"
            @submitPregunta ="submitPregunta"
            :ultimaPregunta ="index===preguntas.length-1"
            @terminar="terminar"/>
          <Resultados
          v-if="mostrarSumario"
          :sumario="sumario"
          />
          </v-col>
        </v-row>
      </v-container>
        <Footer/>
    </v-main>
  </v-app>
</template>

<script>
import Header from './components/Header'
import Cuestionario from './components/Cuestionario'
import Footer from './components/Footer'
import Resultados from './components/Resultados'

export default {
  name: 'App',

  components: {
    Header,
    Cuestionario,
    Footer,
    Resultados
  },

  data() {
    return {
      preguntas: [],
      index: 0,
      numCorrectas: 0,
      numTotales: 0,
      sumario: [],
      mostrarSumario: false
      }
    },
    methods: {
      siguiente() {
        this.index++
      },
      incrementar(esCorrecto){
        if(esCorrecto){
          this.numCorrectas++
        }
        this.numTotales++
      },
      submitPregunta(respuesta){
        this.sumario.push(respuesta)
      },
      terminar(){
        this.mostrarSumario = true
      }
    },

  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&type=multiple',{
      method: 'get'
    })
    .then((response) => {
      return response.json()
      })
        .then((jsonData) => {
          this.preguntas = jsonData.results
      })
  }
}
</script>

<style>
#app {
  font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin-top: 60px;
  text-align: center;
}  
</style>
