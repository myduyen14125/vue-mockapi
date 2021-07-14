<template>
  <div class="register">
    <div class="register-container">
      <h1>Register</h1>

      <form action="" v-on:submit.prevent="addUser">
        <p style="text-align: left">* Create new account</p>
        <input type="text" placeholder="Username" required v-model="username" />
        <span style="color: red" v-bind:class="{ nonExistedAcc }"
          >This username has already existed !</span
        >
        <input
          type="password"
          placeholder="Password"
          minlength="6"
          required
          v-model="password"
        />
        <span style="color: red" v-bind:class="{ strongPassword }"
          >Password needs at least 1 number or special character</span
        >

        <p style="text-align: left">* Personal information</p>
        <input type="text" placeholder="Fullname" required v-model="name" />
        <input
          type="text"
          placeholder="Phone Number"
          required
          v-model="phoneNumber"
        />

        <div class="more">
          <div class="checkbox">
            <input type="checkbox" required />
            <span>I agree to your terms and conditions.</span>
          </div>
        </div>
        <button>Register</button>
      </form>

      <div class="redirect-box">
        <p>Have an account?</p>
        <router-link to="/login">Login here</router-link>
      </div>
    </div>
  </div>
</template>

<script>
const faker = require("faker");
export default {
  name: "Register",
  components: {},
  created() {
    console.log(this.$route);
  },
  data() {
    return {
      username: "",
      password: "",
      name: "",
      phoneNumber: "",
      strongPassword: true,
      nonExistedAcc: true,
    };
  },
  computed: {
    newUser() {
      return {
        createdAt: faker.date.recent(),
        name: this.name,
        avatar: faker.image.avatar(),
        phoneNumber: this.phoneNumber,
        information: faker.lorem.paragraph(),
        username: this.username,
        password: this.password,
        description: faker.lorem.sentence(),
        id: this.$store.state.maxID,
      };
    },
  },
  methods: {
    validPassword() {
      let check = 0;
      for (let i = 0; i < this.password.length; i++) {
        if (this.password[i] > "0" && this.password[i] < "9") {
          this.strongPassword = true;
          console.log(this.strongPassword);
          check = 1;
        }
      }
      if (check === 0) {
        this.strongPassword = false;
      }
      console.log("check = " + check);
    },
    checkAccount() {
      let response = "hehe";
      if (response.length === 0) {
        this.nonExistedAcc = false;
      }
    },
    addUser() {
      this.checkAccount();
      this.validPassword();
      if (this.strongPassword === true && this.nonExistedAcc === true) {
        this.$store.commit("addUser", this.newUser);

        alert("You have successfully registered !");
        this.$router.push(`/users`);
      } else {
        // this.strongPassword = true
        // this.nonExistedAcc = true
      }
    },
  },
};
</script>

<style scoped>
a {
  color: #2c3e50;
  border-bottom: 1px solid white;
  transition: 0.2s;
}
a:active {
  color: #1e2731;
}
a:hover {
  border-bottom: 1px solid #2c3e50;
}

.register {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 85vh;
}
.register-container {
  width: 450px;
  padding: 1rem;
  margin: auto;
  text-align: center;
  box-shadow: 5px 5px 13px -4px rgba(0, 0, 0, 0.71);
}
input {
  display: block;
  padding: 15px;
  border-radius: 10px;
  background-color: #f3f3f3;
  color: #2c3e50;
  width: 100%;
  outline: none;
  margin: 20px auto;
  border: none;
  box-sizing: border-box;
  transition: 0.4s;
  border: 1px solid white;
}
input:hover {
  border: 1px solid gray;
}
button {
  width: 100%;
  padding: 15px;
  font-size: 1.15rem;
  border-radius: 10px;
  border: 1px solid #1e2731;
  color: #2c3e50;
  margin: 20px auto;
  transition: 0.4s;
  cursor: pointer;
}
button:hover {
  background-color: #1e2731;
  color: white;
}
.more {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  font-size: 0.9rem;
}
.checkbox {
  display: flex;
  align-items: center;
}

input[type="checkbox"] {
  width: fit-content;
  margin: 10px;
}

.redirect-box {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* Dynamic css */
.strongPassword {
  display: none;
}
.nonExistedAcc {
  display: none;
}

@media screen and (max-width: 1140px) {
  .content {
    font-size: 0.9rem;
  }
  h1 {
    font-size: 1.5rem;
  }
}
@media screen and (max-width: 992px) {
  .intro p {
    width: 60%;
  }
}
@media screen and (max-width: 768px) {
  .intro p {
    width: 80%;
  }
}
@media screen and (max-width: 576px) {
  .intro p {
    width: 80%;
    font-size: 1.1rem;
  }
}
</style>
