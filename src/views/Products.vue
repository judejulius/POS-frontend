<template>
<section>
<div class="products" >
  <div style="text-align:center">
<h1 style="text-align:center">Products</h1>
</div>
    </div>


<div class="container">
<div class="row">
                <label for="" class="form-label">Search by category</label>
                <input type="text" class="form-select" name="addCategory" id="addCategory" v-model="search">
                  
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
<div>
</div>
</section>
</template>

<script>

export default {
  data() {
    return {
      products: null,
    search:""
      
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






