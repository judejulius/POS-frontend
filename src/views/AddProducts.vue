<template>
  <section class="create">
    <div class="container">
      <div class="form-body">
        <div class="row">
          <div class="form-holder">
            <div class="form-content">
              <div class="form-items">
                <h3>Add product</h3>
                <p>Fill in the data below.</p>
                <form
                  class="requires-validation"
                  @submit.prevent="createProduct"
                >
                  <div class="col-md-12">
                    <input
                      class="form-control"
                      type="text"
                      placeholder="Product Name"
                      required
                      v-model="name"
                    />
                  </div>

                  <div class="col-md-12">
                    <input
                      class="form-control"
                      type="text"
                      placeholder="Product Category"
                      required
                      v-model="category"
                    />
                  </div>
                  <div class="col-md-12">
                    <input
                      class="form-input neu-border-inset"
                      type="text"
                      v-model="img"
                      placeholder="Product Image"
                      required
            
                    />
                  </div>
                 
                  <div class="col-md-12">
                    <input
                      class="form-control"
                      type="text"
                      placeholder="Product Price"
                      required
                      v-model="price"
                    />
                  </div>
                  <div class="form-button mt-3">
                    <button id="submit" type="submit" class="btn btn-primary">
                      Create
                    </button>
                  </div>
                </form>
              </div>
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
      name: "",
      category: "",
      img: "",
      price: "",
    };
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
};
</script>
<style scoped>

</style>