<template>
  <div class="author">
    <span style="display: block; color: gray; text-align: center">ABOUT THE USER</span>
    <hr>
    <div class="box">
      <img :src="user.avatar" alt="">
      <div class="content">
        <h2>{{ user.name }}</h2>
        <h5 style="color: gray">@{{ user.username }}</h5>
        <p><i>" {{ user.description }} "</i></p>
        <h5>* Time join in: {{ user.createdAt }}</h5>
        <h5>* Phone number: {{ user.phoneNumber }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
// import axios from 'axios'
export default {
  props: ['user'],
  data() {
    return {
    };
  },
  // async created(){
  //   const response = await axios.get('https://60d94868eec56d001747768f.mockapi.io/v1/users/' + this.$route.params.id)
  //   this.user = response.data
  // }
};
</script>

<style scoped>
.author {
  width: 820px;
  margin: 30px auto;
  text-align: left;
  background-color: #1e2731;
  border-radius: 5px;
  color: white;
  padding: 20px;
  box-shadow: 0px 3px 3px -4px rgba(0,0,0,0.71);
}
.box {
  display: flex;
  align-items: center;
  justify-content: left;
}
h5{
  margin: 0;
  font-size: 1rem;
}
img {
  width: 180px;
  height: 180px;
  object-fit: cover;
  border-radius: 100%;
  margin: 20px 50px;
}
@media screen and (max-width: 600px) {
  .box{
    flex-direction: column;
    text-align: center;
  }
}
</style>
