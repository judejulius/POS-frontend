<template>
    <form @submit.prevent="updateprofile" class="form border">
    <h2 class="form-heading">Update Profile</h2>

    <input
      class="form-input border-input"
      type="text"
      v-model="name"
      placeholder="Name"
    />
    <input
      class="form-input border-input"
      type="email"
      v-model="email"
      placeholder="Email"
    />
    <input
      class="form-input border-input"
      type="text"
      v-model="contact"
      placeholder="Contact Number"
    />
    <input
      class="form-input border-input"
      type="password"
      v-model="password"
      placeholder="Password"
    />
    <button type="submit" class="form-btn border">Update</button>
  </form>
</template>
<script>
export default {

    data() {
    return {
      users: null,
      name:"",
      contact:"",
      email:"",
    };
  },
mounted() {
    if (localStorage.getItem("jwt")) {
      fetch("http://localhost:2000/users"  , {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.users = json;
          this.users.forEach(async (user) => {
            await fetch(
              "http://localhost:2000/users/" + user._id,
              {
                method: "GET",
                headers: {
                  "Content-type": "application/json; charset=UTF-8",
                  Authorization: `Bearer ${localStorage.getItem("jwt")}`,
                },
              }
            )
            
              .then((response) => response.json())
              
              .then((json) => {
               user._id = json.name;
              });
          });
        })
        .catch((err) => {
          alert("Not logged in");
          this.$router.push({ name: "Login" })
        });
    } else {
      alert("Login failed");
      this.$router.push({ name: "Login" });
    }
    
  },
  
}
</script>
<style>
.border {
  border: none !important;
}

input{
    background-color: #e8f0fe;
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