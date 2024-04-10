<template>
  <router-link
    :to="navigate"
    class="ml-5 mr-1 text-decoration-none text-uppercase white--text text-body-1 font-weight-medium"
  >
    <v-container class="d-flex justify-center align-center">
      <v-icon class="white--text">mdi-{{ iconname }}</v-icon>
    </v-container>
    <v-divider></v-divider>
    <span>{{ content }}</span>
  </router-link>
</template>

<script>
export default {
  props: {
    iconname: String,
    content: String,
    navigate: String,
  },
};
</script>
