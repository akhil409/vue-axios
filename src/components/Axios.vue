<template>
  <v-container>
    <v-row class="text-center">
        <h1>Get Request</h1>
        <div >
          <button @click="display=!display" class="btn btn-info">
           <span v-if="!display">Show Data</span>
           <span v-else>Hide Data</span>
          </button>
          <table class="table table-bordered table-striped mt-5" v-if="display">
               <thead>
                  <tr>
                      <th>Id</th>
                      <th>Name</th>
                      <th>Username</th>
                      <th>Email</th>
                      <th>Address</th>
                      <th>Mobile No.</th>
                      <th>Website</th>
                      <th>Company</th>
                  </tr>
               </thead>
               <tbody>
                  <tr v-for="(value,index) in fakeData"  :key="index">
                    <td> {{value.id}}  </td>
                    <td v-text="value.name"></td>
                    <td> {{value.username}}  </td>
                    <td> {{value.email}}  </td>
                    <td> Street:{{value.address.street}},{{value.address.suite}},{{value.address.city}},{{value.address.zipcode}},{{value.address.geo.lat}},{{value.address.geo.lng}} </td>
                    <td> {{value.phone}}  </td>
                    <td> {{value.website}}  </td>
                    <td> {{value.company.name}}  </td>
                  </tr>
               </tbody>
          </table>
          <!-- <tbody>
            <tr v-for="(value,index) in data" :key="index">
              <td> {{value.id}}</td>
              
            </tr>
          </tbody> -->
        </div>
        <!-- Submit Form -->
        <div class="form">
          <div class="alert alert-success" v-if="isSuccess">Post Created Successfully</div>
           <form @submit.prevent="postRequest">
              <div class="form-group">
                <input type="text" placeholder="Enter Title" v-model="title" class="form-control">
              </div>
              <div class="mt-3">
                <button type="submit" class="btn btn-success">Create Post</button>
              </div>
           </form>
        </div>
        <!-- Posted Details from Firebase -->
        <h1>Posted Details from Firebase</h1>
        <table class="table table-bordered table-striped mt-5" v-if="showPosts">
             <thead>
              <tr>
                 <th>Id</th>
                 <th>Title</th>
              </tr>
             </thead>
             <tbody>
              <tr v-for="(value,index) in postDetails" :key="index">
                <td> {{value.id}} </td>
                <td>  {{value.title}} </td>
              </tr>
             </tbody>
        </table>
    </v-row>
  </v-container>
</template>

<script>
import Vue from 'vue';
import axios from 'axios';
import VueAxios from 'vue-axios';
Vue.use(VueAxios, axios);

  export default {
    name: 'Axios',

    data(){
      return{
        fakeData:[],
        data:[],
        display:false,
        title:'',
        isSuccess:false,
        postDetails:[],
        showPosts:true,
      }
    },

    methods:{
      
    getUsers: function() {
         axios.get('https://jsonplaceholder.typicode.com/users').then((response)=>{
              this.fakeData = response.data
              console.log(response.data)
         }).catch((error)=>{
          console.log('Error while fetching Data',error)
         }).finally(()=>{
          console.log('Loading Data completed');
         })
    },

    postRequest: function(){
      axios.post(`https://samplerequests-620b4-default-rtdb.firebaseio.com/posts.json`,{ title: this.title }).then((res)=>{
        this.isSuccess = true;
        console.log("Post request data is ",res);
        this.$emit('postCreated');
      }).catch((err)=>{
        console.log('Error while posting Data to Firebase',err);
      })
    },

    getPostDetails: function(){
      axios.get('https://samplerequests-620b4-default-rtdb.firebaseio.com/posts.json').then((res)=>{
        console.log("Success", res.data);
        // this.sample = res.data
        this.formatePostDetails(res.data)

      }).catch((err)=>{
        console.log('Error while fetching data',err);
      })
    },

    formatePostDetails(posts){
         for(let key in posts){
            this.postDetails.push({ ...posts[key], id:key })
         }
         console.log(this.postDetails);
    },
    
     onCreatePost() {
          this.showPosts = false;
          setTimeout(()=>{
            this.showPosts = true;
          },1000)
        }

    },
    mounted(){
      this.getUsers();
      this.getPostDetails();
    },
    updated(){
      this.postRequest
    }

  }
</script>
