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
            v-if="preguntas.length"
            :preguntaActual="preguntas[index]"
            :siguiente ="siguiente"
            :incrementar="incrementar"/>
          </v-col>
        </v-row>
      </v-container>
        <Footer/>
    </v-main>
  </v-app>
</template>

<script>
import Header from './components/Header';
import Cuestionario from './components/Cuestionario'
import Footer from './components/Footer'

export default {
  name: 'App',

  components: {
    Header,
    Cuestionario,
    Footer
  },

  data() {
    return {
      preguntas: [],
      index: 0,
      numCorrectas: 0,
      numTotales: 0
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
