<template>
<section>
<div class="products" >
  <div style="text-align:center">
<h1 style="text-align:center">Products</h1>
</div>
    </div>


<div class="container">
<div class="row">
  <div>
<label for="" class="form-label">Search by Name</label>
<input type="text" class="form-select" name="addCategory" id="addCategory" v-model="search" style="width:30%;">   
<button
      type="button"
      class="btn btn-primary"
      data-bs-toggle="modal"
      data-bs-target="#exampleModal"
    >
      Add a product
    </button>                 
  </div>
<div v-for="product of filterProducts" :key="product.name" class="card" style="width: 25rem;">
  <img :src="product.img" class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">{{product.name}}</h5>
    <p class="card-text">R{{product.price}}</p>
    <p class="card-text">{{product.category}}</p>
    <a class="login btn btn-primary text-white">Add to cart</a>
  </div>
</div>
</div>
</div>

<div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <button
          type="button"
          class="btn-close"
          data-bs-dismiss="modal"
          aria-label="Close"
        ></button>

        <div class="modal-body">
          <div class="modal-content">
            <div class="modal-body">
              <div class="mb-3">
                <label for="addTitle" class="form-label">Name</label>
                <input
                  class="form-control"
                  type="text"
                  name="addTitle"
                  id="addTitle"
                  v-model="name"
                />
              </div>
              <div class="mb-3">
                <label for="" class="form-label">Category</label>
                <select
                  class="form-select"
                  name="addCategory"
                  id="addCategory"
                  v-model="category"
                >
                  <option value="Samsung">Samsung</option>
                  <option value="Huawei">Huawei</option>
                  <option value="Iphone">Iphone</option>
                </select>
              </div>
              <div class="mb-3">
                <label for="addPrice" class="form-label">Price</label>
                <input
                  class="form-control"
                  type="text"
                  name="addPrice"
                  id="addPrice"
                  v-model="price"
                />
              </div>
              <div class="mb-3">
                <label for="addImg" class="form-label">Image URL</label>
                <input
                  class="form-control"
                  type="text"
                  name="addImg"
                  id="addImg"
                  v-model="img"
                />
              </div>
            </div>

            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <br />
            <button
              type="button"
              class="btn btn-primary"
              data-bs-dismiss="modal"
              @click="createProduct()"
            >
              Create Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
</template>
<script>

export default {
  data() {
    return {
      products: null,
    search:"",
    name: "",
    img: "",
    price: "",
    category: "",
      
    };
  },
  // fetching product
  created() {
    if (localStorage.getItem("jwt")) {
      fetch("http://localhost:2000/products", {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.products = json;
          this.products.forEach(async (product) => {
            await fetch(
              "http://localhost:2000/products" + product.name,
              {
                method: "POST",
                headers: {
                  "Content-type": "application/json; charset=UTF-8",
                  Authorization: `Bearer ${localStorage.getItem("jwt")}`,
                },
              }
            )
              .then((response) => response.json())
              .then((json) => {
                product.name = json.name;
              });
          });
        })
        .catch((err) => {
          alert("Log in failed");
          this.$router.push({ name: "Login" });
        });
    } else {
      alert("Not logged in");
      this.$router.push({ name: "Login" });
    }
    
  },
  methods: {
createProduct() {
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch("http://localhost:2000/products", {
        method: "POST",
        body: JSON.stringify({
          name: this.name,
          category: this.category,
          price: this.price,
          img: this.img,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("Product Created");
          this.$router.push({ name: "Products" });
        })
        .catch((err) => {
          alert(err);
        });
    },
  },
  computed:{
    filterProducts:function(){
      return this.products.filter((product) =>{
        return product.category.match(this.search)
      })
    }
  }
  
  
}
</script>

<style scoped>
img{
  object-fit: contain !important;
  width: 350px;
  height: 200px;
}
section{
  background-color: white;
  height: 100%;
}
.row{
gap: 10px;
}
</style>






