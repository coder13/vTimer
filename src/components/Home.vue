<template>
  <v-layout justify-center align-center>
    <Timer/>
  </v-layout>
</template>

<script>
import Timer from './Timer';

export default {
  components: {
    Timer,
  },
  name: 'home',
};
</script>

<style scoped>
span#time {
  font-size: 20em;
}

span#centi {
  font-size: .75em;
}

</style>