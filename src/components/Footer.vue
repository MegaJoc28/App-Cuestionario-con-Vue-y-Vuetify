<template>
  <v-footer dark padless  class="footer">
    <v-card class="flex" flat tile>
      <v-card-title class="teal lighten-2">
        <strong class="subheading">Revisa mis proyectos o contáctame!</strong>

        <v-spacer></v-spacer>

        <v-btn v-for="(icon, index) in icons" :key="index" :href="icon.link" target="_blank" class="mx-4" dark icon>
            <v-icon size="24px">
            {{ icon.icon }}
            </v-icon>
        </v-btn>
      </v-card-title>

      <v-card-text class="py-2 white--text text-center">
        {{ new Date().getFullYear() }} — <strong>José Zapata</strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
  export default {
    data: () => ({
      icons: [
        {icon: 'mdi-github', link: 'https://github.com/MegaJoc28?tab=repositories'},
        {icon: 'mdi-linkedin', link: 'https://www.linkedin.com/in/j-zapata/'}
      ],
    }),
  }
  
  

</script>

<style scoped>
 .footer {
     position: absolute;
     bottom: 0;
     width: 100%;
 }
</style>