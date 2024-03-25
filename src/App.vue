<template>
  <v-app>
    <HelloWorld/>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
  mounted(){
    window.domo.get('/data/v2/durationReportData?limit=100')
    .then(function(durationReportData){
      console.log("durationReportData", durationReportData);
    });

  },
};
</script>
