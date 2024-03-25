<template>
  <v-app>
    <v-row no-gutters>
      <v-col cols="6" class="pr-1">
        <month-picker-card></month-picker-card>
      </v-col>
      <v-col cols="6">
        <data-table-card></data-table-card>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import DataTableCard from './components/DataTableCard.vue';
import MonthPickerCard from './components/MonthPickerCard.vue';
export default {
  name: 'App',

  components: {
    MonthPickerCard, DataTableCard
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
