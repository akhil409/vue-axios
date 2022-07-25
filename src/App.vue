<template>
  <v-app>
    <v-main>
      <Axios @postCreated="onCreatePost"/>
    </v-main>
  </v-app>
</template>

<script>
import Axios from './components/Axios.vue';

export default {
  name: 'App',

  components: {
    Axios
},

  data: () => ({
    showPosts:true
  }),
  methods:{
        onCreatePost() {
          this.showPosts = false;
          setTimeout(()=>{
            this.showPosts = true;
          },1000)
        }
  }
};
</script>
