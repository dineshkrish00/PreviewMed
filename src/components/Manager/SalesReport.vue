<template>
  <div>
     <v-card fluid class="mt-16 mx-16 ">
       <v-card-title class="text-h5 green--text font-weight-bold">Sales Report</v-card-title>
       <v-card-text>
         <v-row justify="space-around">
           <v-col cols="2">
             <v-text-field v-model="start" outlined label="Start date" type="date"></v-text-field>
           </v-col>
           <v-col cols="2">
             <v-text-field v-model="end" outlined label="End date" type="date"></v-text-field>
           </v-col>
           <v-col cols="2">
             <v-btn @click="change" class="green white--text">SEARCH</v-btn>
           </v-col>
         </v-row>
       </v-card-text>
     </v-card>
     <v-main>
       <v-data-table :headers="headers" :items="filteredItems" :search="search" class="elevation-1 my-12 mx-15">
         <template v-slot:top>
           <v-toolbar flat>
             <v-text-field v-model="search" label="Search" class="mx-4"></v-text-field>
           </v-toolbar>
         </template>
       </v-data-table>
     </v-main>
     <v-snackbar v-model="snackbar" class="">{{ text }}
       <template v-slot:action="{ attrs }">
         <v-btn :color="colour" text v-bind="attrs" @click="snackbar = false">Close</v-btn></template>
       </v-snackbar>
  </div>
 </template>
 
 <script>
 export default {
  data() {
     return {
       search: "",
       start: "",
       end: "",
       scheck: false,
       text: "",
       colour: "green",
       snackbar: false,
       startdate:'',
       enddate:'',
       headers: [
         { text: "Billno", value: "Billno" },
         { text: "Bill Date", value: "Billdate" },
         { text: "Medicine Name", value: "MedicineName" },
         { text: "Qty", value: "quantity" },
         { text: "Amount", value: "amount" },
       ],
       filteredItems: [],
       tempItems: [],
     };
  },
  methods: {
     change() {
       if (this.start != "" && this.end != "") {
         this.tablepassing();
       } else {
         this.snackbar = true;
         this.text = "First Select the date";
       }
     },
     tablepassing() {
       let a = this.$store.state.billMaster;
       let b = this.$store.state.billDetails;
       let c = 0;
       
       for (let i of b) {
         for (let j of a) {
           if (i.Billno == j.Billno) {
            console.log(j.Billdate);
             b[c]['Billdate'] = j.Billdate;
           }
         }
         c++;
       }
       this.tempItems = b;
      // b.forEach((ele) => 
      // for (let ele of b){
      //  this.tempItems.push({Billno:ele.Billno, MedicineName:ele.MedcineName, Billdate:ele.Billdate, quantity:ele.quantity, unitprice:ele.unitprice, amount:ele.amount}) 
      // }

      console.log(this.tempItems)
    console.log("temp"+this.tempItems)
       this.startdate = this.convertDate(this.start);
       this.enddate = this.convertDate(this.end);
       this.filteredItems = this.tempItems.filter((item) => {
         return this.startdate <= item.Billdate && this.enddate >= item.Billdate;
       });
      },

   convertDate(date) {
        var dateParts = date.split('-');
        return dateParts[2] + '/' + dateParts[1] + '/' + dateParts[0];
        },
  },

 };
 </script>