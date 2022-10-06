<template>
  <v-app>
    <v-main>
      <Velha/>
    </v-main>
  </v-app>
</template>

<script>
import Velha from './components/JogoDaVelha.vue'

export default {
  name: 'App',

  components: {
    Velha
  },

  data: () => ({
    //
  }),
};
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app{
  height: 100vh;
  width: 100vw;
  background: linear-gradient(90deg, rgb(202, 189, 1), rgb(255, 166, 0));
}
::-webkit-scrollbar{
  display: none;
}
</style>
