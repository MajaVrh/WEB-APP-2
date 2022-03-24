<template>

  <v-app>
  

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
//MV
</script>

<style scoped>
.v-main{background-color: rgb(238, 238, 238);}

</style>
