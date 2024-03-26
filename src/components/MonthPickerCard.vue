<template>
    <div class="hello">
      <v-row no-gutters>
        <v-col cols="12">
          <v-menu
            ref="monthPickerMenu"
            v-model="monthPickerMenu"
            :close-on-content-click="false"
            :return-value.sync="monthSelected"
            transition="scale-transition"
            offset-y
            width="416.75px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-row
                no-gutters
                class="fill-height mb-2"
                style="border: 1px solid #979797"
                v-on="on"
                v-bind="attrs"
              >
                <v-col cols="10">
                  <v-row no-gutters>
                    <v-col
                      cols="12"
                      class="text-start px-2 pt-2 font-weight-bold"
                    >
                      MONTH
                    </v-col>
                    <v-col
                      cols="12"
                      class="text-start px-2 black--text font-weight-bold"
                    >
                      {{ monthSelected }}
                    </v-col>
                  </v-row>
                </v-col>
                <v-col cols="2" class="align-self-center">
                  <v-icon color="primary">{{
                    monthPickerMenu ? "mdi-menu-up" : "mdi-menu-down"
                  }}</v-icon>
                </v-col>
              </v-row>
            </template>
            <v-date-picker
              width="402px"
              v-model="monthSelected"
              :max="maxDate"
              type="month"
              no-title
              scrollable
              @change="setMonthPicker()"
            >
            </v-date-picker>
          </v-menu>
        </v-col>
      </v-row>
    </div>
  </template>
  
  <script>
  import { format } from "date-fns";
  import { EventBus } from "@/lib/EventBus";
  export default {
    name: "HelloWorld",
    props: {
      msg: String,
    },
    data() {
      return {
        monthPickerMenu: false,
        monthSelected: new Date().toISOString().substr(0, 7),
        maxDate: new Date().toISOString().substr(0, 7),
      };
    },
    methods: {
      async setMonthPicker() {
        this.$refs.monthPickerMenu.save(this.monthSelected);
        EventBus.$emit('apply-custom-filter', {column: 'monthName', value: format(new Date(this.monthSelected), "MMMM yyyy")})
        // window.domo.onFiltersUpdate(console.log);
  
      },
    },
  };
  </script>