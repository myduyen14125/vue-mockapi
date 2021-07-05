<template>
  <div class="search-bar">
    <form action="" v-on:submit.prevent="findUsers">
      <input type="text" v-model="name" placeholder="Find the users you care about..." required/>
      <div class="fa fa-search"></div>
    </form>
    
  </div>
</template>

<script>
import axios from 'axios'
export default {
  props: ['users'],
  data() {
    return {
      name: ''
    };
  },
  methods: {
    async findUsers(){
      const response = await axios.get('https://60d94868eec56d001747768f.mockapi.io/v1/users?username=' + this.name)
      console.log(response.data.length)
      if(response.data.length !== 0) {
        alert('hhehe')
      }
    }
  }
};
</script>

<style scoped>
.search-bar {
  width: 600px;
  margin: 20px auto;
  position: relative;
}
input {
  display: block;
  padding: 15px;
  border-radius: 15px;
  color: #2c3e50;
  width: 100%;
  outline: none;
  margin: 20px auto;
  box-sizing: border-box;
  transition: 0.4s;
  border: 1px solid gray;
  font-size: 1rem;
}
input:hover {
  border: 1px solid #2c3e50;
}
.search-bar .fa {
  position: absolute;
  right: 20px;
  top: 0;
  height: 100%;
  display: flex;
  align-items: center;
  font-size: 1.5rem;
  color: gray;
  cursor: pointer;
  transition: 0.3s;
}
.search-bar .fa:hover {
  color: #2c3e50;
}

@media screen and (max-width: 500px) {
  .search-bar {
    width: 400px;
  }
  input {
    font-size: 0.95rem;
  }
}
@media screen and (max-width: 420px) {
  .search-bar {
    width: 270px;
  }
  input {
    font-size: 0.85rem;
  }
  input::placeholder {
    color: white;
  }
}
</style>
