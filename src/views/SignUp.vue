<template>
  <section>
    
  <form @submit.prevent="register" class="form border">
    <h2 class="form-heading">SignUp</h2>
    <input
      class="form-input border-input"
      type="text"
      v-model="name"
      placeholder="Name"
      autocomplete="off"
    />
    <input
      class="form-input border-input"
      type="email"
      v-model="email"
      placeholder="Email"
      autocomplete="off"
    />
    <input
      class="form-input border-input"
      type="text"
      v-model="contact"
      placeholder="Contact Number"
      autocomplete="off"
    />
    <input
      class="form-input border-input"
      type="password"
      v-model="password"
      placeholder="Password"
      autocomplete="off"
    />
    <button type="submit" class="form-btn border">Sign up</button>
    <div>
      <p>Already have an account? <router-link to="/login" class="login">Login</router-link></p>
    </div>
  </form>
  </section>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      contact: "",
      password: "",
      about:"",
      msg: '',
      avatar:""
    };
  },
  methods: {
    register() {
         fetch('http://localhost:2000/users', {
  method: 'POST',
  body: JSON.stringify({
    email:this.email,
    password:this.password,
    name:this.name,
    contact:this.contact,
    about:this.about,
    avatar:this.avatar
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
      this.msg = `${ this.name }  Registered Successfuly`;
       alert("Logging in...");
          this.$router.push({ name: "Login" });
    },
  },
};
</script>
<style scoped>
section{
  background-color: white;
  height: 100;
}

.border {
  border: none !important;
}


.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 40px;
  gap: 20px;
  width: 100%;
  max-width: 600px;
  margin-inline: auto;
}

.form-heading {
  text-align: center;
  text-transform: uppercase;
}

.form-input,
.form-btn {
  border: none;
  outline: none;
  padding: 20px;
}
input{
    background-color: #e8f0fe;
}

.form-btn {
  cursor: pointer;
  transition: all 0.1s linear;
}

.form-btn:hover {
  transform: scale(1.05);
}

.form-social-login {
  display: flex;
  justify-content: space-between;
}

.form-social-btn {
  width: 45%;
  color: #333;
}
p{
  font-size: 25px;
}
.login {
  font-size: 25px;
  color: blue;
}
.login:hover{
  color: blue;
  border-bottom:1px solid blue ;
}
</style>