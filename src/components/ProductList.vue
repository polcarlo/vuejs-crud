<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input type="text" class="form-control" placeholder="Search by category"
          v-model="category"/>
        <div class="input-group-append">
          <button class="btn btn-outline-secondary" type="button"
            @click="searchCategory"
          >
            Search
          </button>
          
        </div>
       
      </div>
       <br/>
          <i>Note: There is no search title in the fakestoreapi docs for updating and adding. I have included a response to demonstrate that it is working.</i>
    </div>
    <div class="col-md-6">
      <h4>Product List</h4>
      <ul class="list-group">
        <li class="list-group-item"
          :class="{ active: index == currentIndex }"
          v-for="(product, index) in product"
          :key="index"
          @click="setActiveProduct(product, index)"
        >
          {{ product.title }}
          Category: {{ product.category }}
        </li>
      </ul>
    </div>
    <div class="col-md-6">
      <div v-if="currentProduct">
        <h4>Products</h4>
        <div>
          <label><strong>Title:</strong></label> {{ currentProduct.title }}
        </div>
        <div>
          <label><strong>Description:</strong></label> {{ currentProduct.description }}
        </div>
                <div>
          <label><strong>Price:</strong></label> {{ currentProduct.price }}
        </div>
                <div>
          <label><strong>category:</strong></label> {{ currentProduct.category }}
        </div>
        
       

        <router-link :to="'/product/' + currentProduct.id" style="color: red;">Edit</router-link>
      </div>
      <div v-else>
        <br />
        <p>Please click on a Products...</p>
      </div>
    </div>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";

export default {
  name: "product-list",
  data() {
    return {
      product: [],
      currentProduct: null,
      currentIndex: -1,
      category: ""
    };
  },
  methods: {
    retrieveProduct() {
      ProductDataService.getAll()
        .then(response => {
          this.product = response.data;
        })
        .catch(e => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveProduct();
      this.currentProduct = null;
      this.currentIndex = -1;
    },

    setActiveProduct(product, index) {
      this.currentProduct = product;
      this.currentIndex = index;
    },
    
    searchCategory() {
      ProductDataService.findByCategory(this.category)
        .then(response => {
          this.product = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.retrieveProduct();
  }
};
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>
