<template>
    <v-app>
      <v-container>
        <h1 class="green--text text-center">LOGIN HISTORY</h1>
        <v-divider></v-divider>
        <p class="green--text font-weight-bold text-h5">Login Details</p>
        <v-container fluid>
        <v-card >
      <v-card-title class="ml-8 green--text text-h5 font-weight-medium">
       History
        <v-spacer></v-spacer>
        <v-text-field class="red--text"
          v-model="search" 
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table  class="text-caption font-weight-medium px-8"
        :headers="headers"  :items="list" :search="search" >
    
        <template v-slot:item.Type="{ item }">
      <v-chip :color="getColor(item.Type)" dark>
        {{ item.Type }}
      </v-chip>
    </template>
    </v-data-table>
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
          { text: 'User info', align: 'start',sortable: false, value: 'userid',},
          { text: 'Type', value: 'Type', align: 'center' },
          { text: 'Date & Time', value: 'Date' , align: 'center'},],
        id:'',
        pass:'',
        role:'',
        list:[],
    }
  },
  methods:{
    getColor(Type) {
      console.log(Type);
        if (Type == 'login'){return 'green'}
        else if (Type == 'logout'){ return 'red'}
      },
  },
  mounted(){
    this.list = this.$store.state.log_history
  },
  };
</script>