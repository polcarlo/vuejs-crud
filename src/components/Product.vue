<template>
  <div v-if="currentProduct" class="edit-form">
    <h4>Products</h4>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" id="title"
          v-model="currentProduct.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description"
          v-model="currentProduct.description"
        />
      </div>
       <div class="form-group">
        <label for="category">Category</label>
        <input type="text" class="form-control" id="category"
          v-model="currentProduct.category"
        />
      </div>
       <div class="form-group">
        <label>Price</label>
        <input type="text" class="form-control" id="price"
          v-model="currentProduct.price"
        />
      </div>
       <div class="form-group">
        <label>Image Url</label>
        <input type="text" class="form-control" id="image"
          v-model="currentProduct.image"
        />
      </div>
    </form>
    <br/>

    <button style="color: red"
      @click="deleteProduct"
    >
      Delete
    </button>

    <button type="submit" style="color: black"
      @click="updateProduct"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a Product...</p>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";

export default {
  name: "product",
  data() {
    return {
      currentProduct: null,
      message: ''
    };
  },
  methods: {
    getProduct(id) {
      ProductDataService.get(id)
        .then(response => {
          this.currentProduct = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },



    updateProduct() {
      ProductDataService.update(this.currentProduct.id, this.currentProduct)
        .then(response => {
          console.log(response.data);
          this.message = 'The product was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteProduct() {
      ProductDataService.delete(this.currentProduct.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "product" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getProduct(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
