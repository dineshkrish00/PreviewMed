<template>
    <div>
      <v-toolbar app outlined class="green white--text">
        <v-img src="../../assets/med1.png" class="shrink mr-0" cover width="50"></v-img>
        <v-toolbar-title class="text-h5 font-weight-medium text-uppercase pl-10">{{ this.$store.state.useractive.role }}</v-toolbar-title>
        <v-spacer></v-spacer>
        <navbar  content="Dashboard" iconname="view-dashboard-edit-outline" navigate="/dashboard"></navbar>
        <navbar v-if="role=='System Admin'" content="AddUser" iconname="account-plus-outline" navigate="/adduser"></navbar>
        <navbar v-if="role=='System Admin'"  content="Login history" iconname="history" navigate="/history"></navbar>   
        <navbar v-if="role=='Inventry' ||role=='Manager'"  content="Stock entry" iconname="stocking" navigate="/stockentry"></navbar>
        <navbar v-if="role=='Inventry' || role =='Manager' ||role=='Biller'"  content="Stock view" iconname="eye-arrow-left-outline" navigate="/stockview"></navbar>
        <navbar v-if="role=='Biller'"  content="Bill entry" iconname="cash" navigate="/billentry"></navbar>
        <navbar v-if="role=='Manager'" content="Sales report" iconname="chart-line" navigate="/salesreport"></navbar>
        <v-btn small class="ml-4 white green--text" large @click="logout"><v-icon>mdi-logout</v-icon>LOGOUT</v-btn>
    </v-toolbar>
   
</div>
</template>
<script>
import navbar from "../Navbar/Navtemp.vue";
export default{
  data(){
    return{
      role : this.$store.state.useractive.role,
      userid:'',
      date:'',
    }
  },
  components : {
    navbar
  },
  methods:{
  logout(){
    this.$store.state.useractive.role = null;
    for(let i of this.$store.state.Login){
      if(i.role==this.role){
        this.userid=i.userid;
        console.log("passing 1"+this.userid);
      }
    }
    let entry = {userid : this.userid , Type : 'logout' , Date : this.date=this.convertDate(new Date())};
    this.$store.state.log_history.push(entry);
    this.$router.push('/');
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
  }

}
</script>
