<template>
  <v-container class="auto d-flex justify-center align-center"> 
    <v-card class="elevation-16 mt-15" width="350px">
    <v-card-text >
      <v-form v-model="valid" ref="form" lazy-validation class="mp-16 py-8  px-6" >
        <div class="d-flex justify-center">
    <v-img src="../assets/med1.png" class="shrink mr-0"  width="100"></v-img> </div>
     <h2 class="green--text text-h5 font-weight-bold py-8 text-center">LOG-IN</h2>
     <v-text-field v-model="email" :rules="emailRules" outlined  label="E-mail" required class="py-2" ></v-text-field>
     <v-text-field  v-model="password" :rules="passwordRules" outlined  label="Password"  required :type="'password'"></v-text-field>
     <div class="d-flex justify-center">

     <v-btn color="green" class="white--text" @click="submit">Login <v-icon right>
        mdi-open-in-new 
      </v-icon></v-btn>
    </div>
     <v-snackbar v-model="snackbar" class="" >{{ text }}
      <template v-slot:action="{ attrs }">
        <v-btn :color="colour" text v-bind="attrs" @click="snackbar = false">Close</v-btn></template>
        </v-snackbar>
    </v-form>
</v-card-text>
</v-card>
  </v-container>
</template>

<style scoped>
 .d-flex-align-center {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
 }
</style>

<script>
export default {
  name: "loginname",
  data() {
      return{
          valid: true,
          colour: '',
     emailRules: [ v => !!v || 'E-mail is required',   v => /.+@.+\..+/.test(v) || 'E-mail must be valid'],
     passwordRules: [ v => !!v || 'Password is required'],
     snackbar:false, passing: false, email: '',   text:'', password: '', log:"logout", date:'', role:'',
      }
  },
  methods:{
      submit(){
        let one=this.$store.state.Login;
          if(this.passing==false){
                 one.forEach((val) => {
                      if(val.userid==this.email && val.pass==this.password){
                       console.log('hiiii')
                      
                        this.passing=true;   
                        this.role=val.role;
                        this.submitLog();               
                      }
                 });
                 if(this.passing==false){
                      this.text="Invalid user details";
                      this.colour='red';
                      this.snackbar = true;
                 }
                 
              }
              if(this.passing==true){
                          this.snackbar=true;  
                          this.text="login Successfully";
                          this.log="login";
                        this.$router.push('/dashboard');           
                        }
      },
      submitLog() {
        let entry = {userid : this.email , Type : 'login' , Date : this.date=this.convertDate(new Date())};
          this.$store.state.log_history.push(entry);
          //userid updating
          this.$store.commit('updateuser',this.role);
        console.log('pushed'+this.role)
      }, 
      convertDate(date) {
      var day = date.getDate().toString().padStart(2, '0');
      var month = (date.getMonth() + 1).toString().padStart(2, '0'); 
      var year = date.getFullYear();

      var hours = date.getHours();
      var minutes = date.getMinutes();
      var seconds = date.getSeconds();

    return day  + '/' + month + '/' + year + ' ' + hours + ':' + minutes + ':' + seconds;
  },
  },
}
</script>