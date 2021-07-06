<template>
  <div class="users">
    <h1>Lists of users</h1>
    <h4>Want to become an user?
      <router-link to="/register">Click here</router-link>
    </h4>
    
    <button @click="cancelFindUser">Cancel Find Users</button>

    <div class="search-bar">
      <form action="" v-on:submit.prevent="findUsers">
        <input type="text" v-model="findName" placeholder="Find the users you care about..."/>
        <div class="fa fa-search"></div>
      </form>
    
    </div>

    <template v-for="user in users" >
      <div class="user-container" :key="'user-'+user.id" @click="getUser(user.id)">
        
        <img class="photo" :src="user.avatar" alt="#">
        <div class="content">
          <h2 style="margin-bottom: 2px">{{ user.name }}</h2>
          <em class="username" style="margin-bottom: 15px; display: block">@{{ user.username }}</em>
          <em class="" style="color: gray">Quote: {{ user.description }}</em>
          <h4 class="phone">Phone number: {{ user.phoneNumber }}</h4>
        </div>
      </div>
    </template>
    <div v-for="listFindUser in listFindUsers"  :key="'listuser-'+listFindUser.id" @click="getUser(listFindUser.id)">
      <div class="user-container">
        
        <img class="photo" :src="listFindUser.avatar" alt="#">
        <div class="content">
          <h2 style="margin-bottom: 2px">{{ listFindUser.name }}</h2>
          <em class="username" style="margin-bottom: 15px; display: block">@{{ listFindUser.username }}</em>
          <em class="" style="color: gray">Quote: {{ listFindUser.description }}</em>
          <h4 class="phone">Phone number: {{ listFindUser.phoneNumber }}</h4>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
export default ({
  name: 'users',
  components: {
    
  },
  data() {
    return {
      users: 0,
      notBeFound: false,
      listFindUsers: [],
      tempUsers: [],
    }
  },
  async created() {
    const response = await axios.get(`https://60d94868eec56d001747768f.mockapi.io/v1/users`)
    this.users = response.data
    console.log(response.data)
  },
  methods: {
    getUser(id){
      this.$router.push(`/user/${id}`) 
    },
    async findUsers(){
      this.tempUsers = this.users
      console.log(this.tempUsers)
      let count = 0
      this.listFindUsers = []
      const response = await axios.get(`https://60d94868eec56d001747768f.mockapi.io/v1/users`)
      
      for(let i = 0; i < response.data.length; i++){
        if(response.data[i].username === this.findName){
          this.listFindUsers[count] = response.data[i]
          console.log(this.listFindUsers[count])
          count++
        }
      }
      if(count == 0 || this.findName === '') {
        alert('There is no result!')
      }
      else {
        this.users = {}
      }
      console.log('count = ' + count)
      console.log(this.listFindUsers)
      
    },
    cancelFindUser(){
      this.users = this.tempUsers
      console.log(this.tempUsers)
    }
  }
    
})
</script>

<style scoped>
.users{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
}
.user-container{
  display: flex;
  align-items: center;
  padding: 1rem;
  box-shadow: 3px 4px 4px -5px rgba(0,0,0,0.71);
  width: 760px;
  margin: 1.6rem auto;
  text-align: left;
  font-size: 1rem;
  cursor: pointer;
  transition: 0.4s;
}
.user-container:hover{
  box-shadow: 5px 8px 10px -5px rgba(0,0,0,0.71);
}
  img.photo{
    min-width: 250px;
    min-height: 180px;
    object-fit: cover;
  }
  .content{
    padding: 10px 25px;
  }
  .content h3{
    margin: 10px 0;
    font-size: 1.2rem;
  }

/* Search-bar */
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

/* Dynamic CSS */
.notBeFound{
  display: none;
}

/* Responsive */
@media screen and (max-width: 992px) {
  .content{
    font-size: 0.95rem;
  }
  .user-container{
    width: 700px;
  }
  img.photo{
    min-width: 240px;
    min-height: 160px;
  }
}
@media screen and (max-width: 768px) {
  .content{
    font-size: 0.8rem;
  }
  .user-container{
    width: 530px;
  }
  img.photo{
    min-width: 150px;
    min-height: 110px;
  }
  h3.phone{
    font-size: 0.9rem
  }
}
@media screen and (max-width: 567px){
  .content h3{
    font-size: 1rem;
  }
  .user-container{
    width: 90%;
  }
  img.photo{
    min-width: 130px;
    min-height: 100px;
  }
}
@media screen and (max-width: 420px){
  .user-container .content .phone{
    display: none;
  }
}
</style>