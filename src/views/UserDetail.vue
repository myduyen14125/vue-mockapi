<template>
  <div class="user-detail">
    <!-- <h1>{{$route.params.id}}</h1> -->
    <div class="cover">
      <Author v-bind:user="user" />
      <div class="edit-profile">
        <button class="big-edit" @click="editProfile">Edit Profile</button>
        <div
          class="menu-edit"
          v-bind:class="{ showMenuEdit: availableMenuEdit }"
        >
          <button @click="editName">Edit Your Name</button>
          <button @click="editPhone">Edit Phone Number</button>
          <button @click="deleteUser()">Delete Account</button>
          <hr style="margin: 1px" />
          <button @click="cancelEdit">Cancel</button>
        </div>
      </div>

      <div class="edit-modal" v-bind:class="{ showModal: availableModal }">
        <div class="modal-content">
          <h3>Edit your information below here !</h3>
          <hr />
          <form action="" v-on:submit.prevent="successEdit(user.id)">
            <input v-model="currentInput" type="text" required />
            <button type="submit">Submit</button>
            <button @click="cancelModal">Cancel</button>
          </form>
        </div>
      </div>
    </div>

    <div class="container">
      <p>{{ user.information }}</p>
      <br />
      <p>{{ user.information }}</p>
      <br />
      <p>{{ user.information }}</p>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Author from "../components/uncommon/Author.vue";

export default {
  components: {
    Author,
  },
  data() {
    return {
      currentInput: null,
      edit: "",
      availableModal: false,
      availableMenuEdit: false,
    };
  },
  computed: {
    user() {
      let activeUser = {};
      this.$store.state.persons.forEach((user) => {
        if (user.id == this.$route.params.id) activeUser = user;
      });
      return activeUser;
    },
  },
  methods: {
    editProfile() {
      this.availableMenuEdit = true;
    },
    editName() {
      this.edit = "name";
      this.availableModal = true;
      this.availableMenuEdit = false;
    },
    editPhone() {
      this.edit = "phone";
      this.availableModal = true;
      this.availableMenuEdit = false;
    },
    async successEdit(id) {
      if (this.edit === "name") {
        await axios
          .put(`https://60d94868eec56d001747768f.mockapi.io/v1/users/${id}`, {
            name: this.currentInput,
          })
          .then(function(response) {
            alert("Success Updated!");
            // const response = await axios.get(`https://60d94868eec56d001747768f.mockapi.io/v1/users/${id}`)
            // location.reload();
            console.log(response);
            console.log(response.data);
          })
          .catch(function(error) {
            console.log(error);
            alert("Error !");
          });
        const response = await axios.get(
          `https://60d94868eec56d001747768f.mockapi.io/v1/users/${id}`
        );
        console.log(response.data);
        this.user = response.data;
      }
      if (this.edit === "phone") {
        await axios.put(
          `https://60d94868eec56d001747768f.mockapi.io/v1/users/${id}`,
          {
            phoneNumber: this.currentInput,
          }
        );
        alert("Success Updated!");
        this.user.phoneNumber = this.currentInput;
        //Another way for a faster user experience
      }
      this.currentInput = "";
      this.availableModal = false;
    },
    cancelEdit() {
      this.availableMenuEdit = false;
    },
    cancelModal() {
      this.availableModal = false;
    },
    deleteUser() {
      this.$store.commit("deleteUser", this.user);
      alert("Success !");
      this.$router.push(`/users`);
    },
  },
};
</script>

<style scoped>
.user-detail {
  margin: 0;
  min-height: 30vh;
}
.container {
  width: 55%;
  text-align: left;
  margin: 50px auto;
}
button {
  margin-right: 15px;
  background-color: white;
  padding: 8px 15px;
  border: 1px solid #f8f8f8;
  border-radius: 5px;
  font-size: 0.95rem;
  transition: 0.4s;
  cursor: pointer;
}
button:hover {
  background-color: #1e2731;
  color: white;
}
.cover {
  position: relative;
  padding: 0;
  height: 50vh;
  background: linear-gradient(180deg, #f2f2f2, #ffffff);
  padding-bottom: 50px;
  box-sizing: border-box;
  box-shadow: 3px 3px 5px -4px rgba(0, 0, 0, 0.71);
}
.author {
  margin: auto;
}
.edit-profile {
  position: absolute;
  top: 87%;
  right: calc(50% - 230px / 2);
  width: 230px;
  padding: 0;
}
.big-edit {
  width: 150px;
  margin: auto;
  border: 1px solid gray;
}
.menu-edit {
  width: 100%;
  padding: 5px;
  border: 1px solid gray;
  border-radius: 5px;
  background-color: white;
  box-sizing: border-box;
  display: none;
  transition: 0.3s;
  box-shadow: 3px 4px 10px -5px rgba(0, 0, 0, 0.71);
}
.showMenuEdit {
  display: block;
}
.menu-edit button {
  width: 100%;
  text-align: left;
}

/* Modal */
.edit-modal {
  display: none;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}
.showModal {
  display: block;
}
.modal-content {
  margin: 12% auto;
  padding: 10px;
  width: 400px;
  box-shadow: 0 5px 8px 0 rgba(0, 0, 0, 0.2), 0 7px 20px 0 rgba(0, 0, 0, 0.17);
  background-color: white;
  border-radius: 6px;
  animation-name: modalopen;
}
input {
  display: block;
  padding: 15px;
  border-radius: 10px;
  background-color: #f3f3f3;
  color: #2c3e50;
  width: 90%;
  margin: 20px auto;
  border: none;
  box-sizing: border-box;
  transition: 0.4s;
  border: 1px solid white;
}
input:hover {
  border: 1px solid gray;
}
.modal-content button {
  border: 1px solid #2c3e50;
}

@media screen and (max-width: 992px) {
  .container {
    width: 75%;
    text-align: left;
    margin: 50px auto;
  }
}

@keyframes modalopen {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
