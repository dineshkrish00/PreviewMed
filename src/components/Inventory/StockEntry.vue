<template>
    <v-app>
      <v-container>
        <h1 class="green--text text-center">STOCK ENTRY</h1>
        <v-divider></v-divider>
        <v-container fluid>
        <v-card >
      <v-card-title class="ml-0 green white--text text-h5 font-weight-medium">
        Stock Entry Page
        <v-spacer></v-spacer>
        <v-btn class="white green--text" @click="Addbtn"> + ADD</v-btn>
        <v-snackbar v-model="snackbar" class="" >{{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn :color="colour" text v-bind="attrs" @click="snackbar = false">Close</v-btn></template>
        </v-snackbar>
        <v-form ref="form" v-model="valid" lazy-validation>
        <v-dialog v-model="demo" max-width="600px">
        <v-card >
            <v-card-title class="green white--text">Add Stock</v-card-title>
            <v-text-field outlined class="pa-2 mx-8" :rules="rules" v-model="newName" label="Medicine Name"></v-text-field>
            <v-text-field outlined class="pa-2 mx-8" :rules="rules" v-model="newBrand" label="Brand"></v-text-field>
            <v-btn block @click="newAdd" :disabled="!valid" class="green white--text">ADD</v-btn>
        </v-card>
        </v-dialog>
      </v-form>
      </v-card-title>
      <v-form ref="form" v-model="valid" lazy-validation>
      <v-row class="my-2 mx-2">
        <v-col cols="10" sm="4" md="2" lg="3">
          <v-select :items="list" v-model="SelectedMedicine" :rules="rules" outlined label="SelectedMedicine"></v-select>
        </v-col>
        <span v-show="val">{{ tempvariable }}</span>
        <v-col cols="10" sm="4" md="2" lg="3">
          <v-text-field v-model="brandlist" :rules="rules" readonly outlined label="Brand"></v-text-field>
        </v-col>
        <v-col cols="10" sm="4" md="2" lg="3">
          <v-text-field v-model="Qty" outlined type="number" :rules="numberRules1" required label="Qty"></v-text-field>
        </v-col>
        <v-col cols="10" sm="4" md="2" lg="3">
          <v-text-field v-model="price" outlined type="number" :rules="numberRules2" required label="price"></v-text-field>
        </v-col>
      </v-row>
      <v-row  class="mx-1">
        <v-btn block class="green font-weight-bold white--text" :disabled="!valid" @click="change">UPDATE MEDICINES TO STOCK</v-btn>
        <v-snackbar v-model="snackbar" class="" >{{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn :color="colour" text v-bind="attrs" @click="snackbar = false">Close</v-btn></template>
        </v-snackbar>
      </v-row>
     </v-form> 
    </v-card>
    </v-container>
  </v-container>
  </v-app>
</template>

<script>
export default {
  data(){
    return{
      SelectedMedicine:'',
      val:false,
      text:'',
      snackbar:false,
      colour:'green',
      newName:'',
      newBrand:'',
      valid: true,
    rules: [
      v => (!!v && v.length > 0 && v.length <= 16 && !v.includes(' ')) || 'Invalid characters',
      v => !/[!@#$%^&*(),.?":{}|<>]/g.test(v) || 'Invalid characters',],
    
        list:[],
        brandlist:'',
        Qty: null,
            numberRules1: [ value => !!value || 'Qty is required', value => (value >= 0) || 
            'Qty must be positive',
            v => /^-?\d+$/.test(v) || 'Enter a valid integer number'],
      price:null,
        numberRules2: [ value => !!value || 'Price is required', value => (value >= 0) || 
            'Price must be positive',
            v => /^-?\d+$/.test(v) || 'Enter a valid integer number'],
      demo: false,
    }
  },
  methods:{
    newAdd() {
      if(this.newName!=null && this.newBrand!=null){
          let addFound = false;
          for(let add of this.$store.state.MedicineMaster){
              if(add.MedicineName==this.newName && add.Brand==this.newBrand ){
                this.text="Medichines already exists";
                this.snackbar=true;
                addFound = true;
                break;
              }
              }

            if(!addFound){
                this.$store.commit('addMedicine', {MedicineName: this.newName, Brand: this.newBrand});
                this.text="Medichines Added SUccessfully";
                this.snackbar=true;
              }
              this.demo=false;
            }
          else{
                this.text="Please fill the details";
                this.snackbar=true;
          }
          this.newName=null;
          this.newBrand=null;
    },

    change(){
      if(this.SelectedMedicine!='' && this.brandlist!='' && this.Qty!=null && this.price!=null){
      let j=0;
      let stockUpdate=false;
      for(let i of this.$store.state.Stock){
      if(i.MedicineName==this.SelectedMedicine){
        this.$store.state.Stock[j].quantity=Number(this.$store.state.Stock[j].quantity)+Number(this.Qty);
        this.$store.state.Stock[j].unitprice=Number(this.$store.state.Stock[j].unitprice)+Number(this.price);
        stockUpdate=true;
        this.text="Medichines Updated Successfully";
        this.snackbar=true;
        this.color='green';
        break;
      }
      j++;
    } 
    if(!stockUpdate){
      this.$store.commit('stockUpdate', {MedicineName: this.SelectedMedicine, quantity:this.Qty, unitprice:this.price})
    }
  }
  else{
    this.text="Please choose the details";
    this.snackbar=true;
  }
    },
    Addbtn(){
      this.demo = true
        }
  },
  watch: {
    SelectedMedicine: function(newValue) {
      for(let i of this.$store.state.MedicineMaster){
        if(newValue==i.MedicineName)
        {
          this.brandlist=i.Brand;
        }
      }
    }
 },
 computed:{
  tempvariable(){
    let a=this.$store.state.MedicineMaster;
    a.forEach((element) => {
       this.list.push(element.MedicineName);
    });
    return 0;
  }
 }
  };
</script>