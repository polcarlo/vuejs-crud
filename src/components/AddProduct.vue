<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          required
          v-model="product.title"
          name="title"
        />
      </div>
       <div class="form-group">
        <label for="title">Price</label>
        <input
          type="text"
          class="form-control"
          id="price"
          required
          v-model="product.price"
          name="Price"
        />
      </div>

      <div class="form-group">
        <label for="title">Description</label>
        <input
          type="text"
          class="form-control"
          id="description"
          required
          v-model="product.description"
          name="description"
        />
      </div>
       <div class="form-group">
        <label for="title">Image URL</label>
        <input
          type="text"
          class="form-control"
          id="image"
          required
          v-model="product.image"
          name="Image"
        />
      </div>
       <div class="form-group">
        <label for="title">Category</label>
        <input
          type="text"
          class="form-control"
          id="category"
          required
          v-model="product.category"
          name="category"
        />
      </div>

      <button @click="saveProduct" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <h1>Id: {{ this.product.id}}</h1>
      <h1>Title: {{ this.product.title}}</h1>
      <h2>Description: {{ this.product.description}}</h2>
      <h3>Price: {{ this.product.price }}</h3>
      <h4>Image Url: {{ this.product.image}}</h4>
      <h5>Category: {{ this.product.category}}</h5>
      <button class="btn btn-success" @click="newProduct">Add</button>
    </div>
  </div>
</template>

<script>
import ProductDataService from "../services/ProductDataService";

export default {
  name: "add-product",
  data() {
    return {
      product: {
        id: null,
        title: "",
        description: "",
        image: "",
        category: ""

      },
      submitted: false
    };
  },
  methods: {
    saveProduct() {
      var data = {
         title: this.product.title,
         price: this.product.price,
         description: this.product.description,
         image: this.product.image,
         category: this.product.category,

      };

      ProductDataService.create(data)
        .then(response => {
          this.product.id = response.data.id;
          this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    
    newProduct() {
      this.submitted = false;
      this.Product = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>
