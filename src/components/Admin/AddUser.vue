<template>
    <v-app>
       <v-container>
         <h1 class="blue--text text-center">ADD USERS</h1>
         <v-divider></v-divider>
         <p class="green--text font-weight-bold text-h5">User Details</p>
    <v-row justify="center">
       <v-expansion-panels accordion>
         <v-expansion-panel class="py-5">
           <v-expansion-panel-header class="blue py-2">Item</v-expansion-panel-header>
           <v-expansion-panel-content>
               <v-form>
                 <v-container>
                   <v-row>
                    <v-col cols="12" sm="6" md="3" lg="3">
                       <v-text-field  :rules="emailRules" label="E-mail" v-model="id" outlined></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="3" lg="3">
                       <v-text-field :rules="passwordRules" label="password" :type="'password'" v-model="pass" outlined></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="3" lg="3">
                         <v-select label="User" :items="['Biller', 'Manager', 'System Admin', 'Inventry',]" 
                         v-model="role" outlined></v-select>
                    </v-col>
                    <v-col cols="12" sm="6" md="3" lg="3">
                         <v-btn @click="change" class="blue white--text" x-large outlined>ADD</v-btn>
                    </v-col>
                   </v-row>
                 </v-container>
               </v-form>
           </v-expansion-panel-content>
         </v-expansion-panel>
       </v-expansion-panels>
    </v-row>
    <v-snackbar v-model="snackbar" class="" >{{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn :color="colour" text v-bind="attrs" @click="snackbar = false">Close</v-btn></template>
        </v-snackbar>
   </v-container>

   </v-app>
   </template>

   <script>
   export default {
    data(){
       return{
           id:'',
           pass:'',
           role:'',
           message:'',
           log:false,
           emailRules: [ v => !!v || 'E-mail is required',
       v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
     ],
     passwordRules: [
       v => !!v || 'Password is required'
     ],
           text:'',
           colour:'green',
           snackbar:false,
       }
    },
    methods:{
       change() {
        if(this.id != '' && this.pass != '' && this.role !=''){
         this.log = false;
         this.$store.state.Login.forEach((val) => {
           if(val.userid==this.id && val.pass==this.pass && val.role==this.role ){
              this.snackbar=true; 
              this.text="User Already Exist";
               this.log=true;
               }
           });
         if (!this.log) {
           this.login();
         }
              this.id='';
               this.pass='';
               this.role='';
        }
        else{
                this.snackbar=true; 
              this.text="Please provide all  details";
        }
       },
       login(){
        this.$store.state.Login.push({userid:this.id, pass:this.pass, role:this.role})
                 this.snackbar=true; 
                this.text="User Added succesfully"; 
       },
    } 
   } 
   </script>