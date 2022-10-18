<template>
  <v-app>
    <v-main>
      <qrCode/>
    </v-main>
  </v-app>
</template>

<script>
import qrCode from './components/qr-code.vue'

export default {
  name: 'App',

  components: {
    qrCode
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
  background: hsl(212, 45%, 89%);
}
::-webkit-scrollbar{
  display: none;
}
</style>
