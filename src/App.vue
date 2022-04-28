<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <h1>E-MANAFA Report</h1>
      <v-spacer></v-spacer>
      <v-btn
        href="https://greensoftwarelab.github.io/E-MANAFA/"
        target="_blank"
        text
      >
        <span class="mr-2">E-MANAFA</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>
    <v-main>
      <ResultsFilePicker @filechange="loadJSONFile"/>
      <MainResultsCard :results="getGlobal" />
      <methods-card :called_methods="getInvokedMethods" />
    </v-main>
  </v-app>
</template>

<script>
import ResultsFilePicker from './components/ResultsFilePicker.vue';
import MainResultsCard from './components/MainResultsCard';
import MethodsCard from './components/MethodsCard.vue';

export default {
  name: 'App',

  components: {
    MainResultsCard,
    MethodsCard,
    ResultsFilePicker
  },
  data(){
    return {
      results: {
        'global': {
          },
      },
      json_file: ''
    }
  },
  methods:{
    loadJSONFile(filepath){
      this.json_file = filepath
      this.readJSONFILE(filepath)
    },
    async readJSONFILE(file){
      let text = await (new Response(file)).text()
      this.results = JSON.parse(text)
    },
  },
  computed:{
    getGlobal(){
      return this.results.global
    },
    getInvokedMethods(){
      console.log(this.results)
      return this.results.invoked_methods
    }
  }
};
</script>
