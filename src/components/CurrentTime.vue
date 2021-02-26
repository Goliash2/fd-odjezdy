<template>
  <ion-title mode="ios" style="font-size: xx-large; text-align: right; padding: 0">{{ time }}</ion-title>
</template>

<script>
import {IonTitle} from '@ionic/vue';

export default {
  name: "CurrentTime.vue",
  components: { IonTitle },
  data() {
    return {
      interval: null,
      time: null
    }
  },
  beforeUnmount() {
    // prevent memory leak
    clearInterval(this.interval);
  },
  created() {
    // update the time every second
    this.interval = setInterval(() => {
      // Concise way to format time according to system locale.
      // In my case this returns "3:48:00 am"
      this.time = Intl.DateTimeFormat(navigator.language, {
        hour: 'numeric',
        minute: 'numeric',
        second: 'numeric'
      }).format()
    }, 1000)
  }
}
</script>

<style scoped>

</style>
