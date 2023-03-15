<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <JoyStick />
  </div>
</template>

<script>
// @ is an alias to /src
import JoyStick from '@/components/JoyStick.vue'

export default {
  name: 'HomeView',
  components: {
    JoyStick
  }
}
</script>
