<template>
<!-- Not Used -->
  <div class="container-fluid">
    <div class="row">
      <div class="col-sm-1">
        <span></span>
      </div>
      <div class="col">
        <div class="header">
          <h5
            style="font-family: Circular, -apple-system, BlinkMacSystemFont, Roboto,'Helvetica Neue', sans-serif !important;" >
            Found {{searchresults.length}} Activites Near :&nbsp;
            <strong>{{this.addressData}}</strong>
          </h5>
        </div>{{searchresultsGrouped}}<br>{{searchresults}}
        <div v-for="category in searchresults">
          <h4 style>
            <strong>{{category.categoryName}}</strong>
          </h4>

          <subCateGory v-bind:categoryId="category.id" v-bind:statedata="this.searchresultsGrouped" v-bind:psearchresults="this.searchresults"/>
        </div>
      </div>
    </div>
    <!-- Modal -->
    <div
      class="modal fade"
      id="alertModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">...</div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import http from 'axios';
import subCateGory from './SListing';
import dh from './endpoints.js';
import Mapper from './CategoryMapper.js';
import {API_HOST} from './endpoints.js';

export default {
  components: {
    subCateGory
  },

  data() {
    return {
      searchresults: {},
      searchresultsGrouped: {},
      allCategoryData :[],
      errors: {},
      addressData: {},
      dataFromPrevScreeen :{}
    };
  },
  mounted() {
    if(!this.$route.params.statedata){
      this.loadData();
    }else{           
      this.searchresultsGrouped= this.$route.params.statedata;//Mapper.groupBy(this.$route.params.statedata, p=> p.parent_id,catData.data); 
       this.searchresults= this.$route.params.searchresults;//Mapper.groupBy(this.$route.params.statedata, p=> p.parent_id,catData.data); 
      
    }
  },
  created() {
    this.addressData = this.$route.params.addressData;
    this.allCategoryData=Mapper.getCategorydata();    
  },
  methods: {
    loadData() {
        http.get(API_HOST + '/api/categories/')
        .then(catData => {
          http
        .get('http://localhost:3000/search/city/' + this.addressData)
        .then(response => {
          this.searchresults = response.data;
          this.searchresultsGrouped=Mapper.groupBy(this.searchresults, p=> p.parent_id,catData.data);
        console.log('Got data=');
        console.log(JSON.stringify(this.searchresultsGrouped));
        })
        .catch(e => {
          //this.errors.push(e);
          console.log('Errors' + e);
        });
        })
        .catch(e => {
          //this.errors.push(e);
          console.log('Errors' + e);
        });



      console.log('dh=' + this.addressData);
      Mapper.getCategorydata().then(catData=>{
 

      });



    },
    getImgUrl(pic) {
      return require('../../public/img/' + pic);
    },
    navigate() {
      alert('navigated');
    }
  }
};
</script>

<style>
.card-title {
  text-align: center;
}

.card {
  margin: 0.5rem;
  margin-bottom: 1px;
}

.container-fluid {
  background: none;
  margin-top: 0px;
}

.card-img-top {
  width: 50%;
  height: 10vw;
  object-fit: fill;
}

.h4 {
  font-family: 'Arial Black", Gadget, sans-serif';
}

.starter-page {
  min-height: calc(100vh - 95px);
}
</style>