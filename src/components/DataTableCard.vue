<template>
    <v-container>
      <v-row class="text-center">
        <v-col cols="12">
          <v-data-table :headers="headers" :items="getItems" :loading="loading"></v-data-table>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import { EventBus } from "@/lib/EventBus"
  export default {
    name: "HelloWorld",
  
    data: () => ({
      loading: false,
      reportData: [],
      headers: [
        { text: "Month Name", value: "monthName" },
        { text: "Booking Id", value: "bid" },
        { text: "Booking State Label", value: "bookingStateLabel" },
        { text: "Entry time", value: "entryTime" },
        { text: "Exit time", value: "exitTime" },
      ],
      appliedFilter: [],
    }),
    computed:{
      getItems(){
        return this.reportData;
      },
    },
    mounted() {
      this.loading = false;
      this.fetchData();
      EventBus.$on('apply-custom-filter',  (filterObj) => {
        let index = this.appliedFilter.findIndex((a) => a.column == filterObj.column);
        console.log(this.appliedFilter, filterObj);
        index != -1 ? this.appliedFilter[index] = filterObj : this.appliedFilter.push(filterObj);
      this.fetchData();
    })
    },
    methods: {
      async fetchData() {
        this.loading = true;
  
       this.reportData =  await window.domo
          .get(`/data/v2/durationReportData?limit=100&filter=${this.getFilterURL()}zcode==99999`)
          // .then(function (durationReportData) {
          //   // this.reportData = durationReportData;
          //   // console.log("durationReportData", durationReportData);
          // });
        this.loading = false;
      },
      getFilterURL(){
        let url = "";
        this.appliedFilter.forEach((filter) => {
            url += filter.column + "==" + filter.value + ",";
        })
        console.log(url)
        return url;
      },
    },
  };
  </script>
  