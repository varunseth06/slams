<template>
  <div class="starter-page">
    <div class="section text-center">
          <div v-for="subCategory in searchresults" class="card">
            <img class="card-img-top" v-bind:src="subCategory.logoURL" alt="desc">
            <div class="card-footer">
              <h5 class="card-title">{{subCategory.name}}</h5>
            </div>
          </div>
    </div>
  </div>
</template>
<script>

import http from 'axios';


export default {
props: {
       categoryId : String
   },
 data() {
    return {
      searchresults: {},
      providerDetails:{}
    }
 },
mounted() {
     this.loadData();
},
 methods: {
    loadData() {
      http
        .get('http://192.168.10.20:3000/api/categories/'+this.$props.categoryId + '/providers')
        .then(response => (this.searchresults = response.data))
        .catch(e => {
          //this.errors.push(e);
          console.log("Errors" + e);
        });
    },
    getImgUrl(pic) {
         return require('../../public/img/'+pic)
    },
    naviate(){
      alert('clicked');
      //router --name: 'spvdetails', params: { providerDetails: this.providerDetails }
    }
}
  };
</script>
<style>
.starter-page {
  min-height: calc(100vh - 95px);
}
</style>
