<template>
  <div class="home">
    <HelloWorld/>
  </div>
</template>

<script>
// @ is an alias to /src
import Axios from '@/components/Axios.vue'

export default {
  name: 'Home',
  components: {
    Axios
  }
}
</script>
