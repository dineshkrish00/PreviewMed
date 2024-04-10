<template>
    <v-app>
      <v-container>
        <h1 class="green--text text-center">STOCK VIEW</h1>
        <v-divider></v-divider>
        <v-container fluid>
        <v-card >
      <v-card-title class="ml-8 green--text text-h5 font-weight-medium">
        Stock View Page
        <v-spacer></v-spacer>
        <v-text-field class="red--text" v-model="search" append-icon="mdi-magnify" label="Search"
        single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table  class="text-h5 font-weight-medium px-8"
        :headers="headers"
        :items="list"
        :search="search"
      ></v-data-table>
    </v-card>
    </v-container>
  </v-container>
  </v-app>
  </template>

<script>
export default {
  data(){
    return{
        search: "",
        headers: [
          { text: 'Medicine Name', align: 'start',sortable: false, value: 'MedicineName',},
          { text: 'Brand', value: 'brand' },
          { text: 'Quantity', value: 'quantity' },
          { text: 'Unitprice', value: 'unitprice' },],
        list:[],
    }
  },
  methods:{
  },
  mounted(){
    let a = this.$store.state.MedicineMaster;
    let b = this.$store.state.Stock;
    let c = 0;

    for(let i of b){
        for(let j of a){
            if(i.MedicineName==j.MedicineName){
                b[c]['brand'] = j.Brand;
            }
        }
        c++;
    }
    this.list = b;
  }
  };
</script>