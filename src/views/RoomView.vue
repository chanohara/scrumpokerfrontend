<script setup>
import axios from 'axios';
import cookies from 'vue-cookies';
import router from '../router';
</script>

<template>
<div>You are in a room {{ roomId }}</div>
<br>
Please estimate:
<br>
<div>
  <ul class="vote-list">
    <li class="vote" v-for="num in fibonacci">
      <button class="btn btn-indigo outline" @click="voteClicked(num)">{{ num }}</button>            
    </li>
  </ul>
</div>
<div>
  <ul>
    <li v-for="user in users">
      <div>
      {{ user.name }} voted {{ user.vote }}
      </div>
    </li>
  </ul>
</div>
</template>

<script>
export default {
    data() {
        return {
            revealed: false,
            users: [ { name: "Alex", vote: 99 } ],
            roomId: this.extRoomId,
            fibonacci: [1,2,3,5,8,13,21,34,55,89]
        }
    },
    props: ['extRoomId'],
    async created() {
        const response = await axios.get("http://localhost:3000/rooms/" + this.roomId);
        this.users = [];
        for (let i in response.data) {
            this.users.push( { name: response.data[i].name, vote: response.data[i].current_vote });
            this.revealed = response.data[i].revealed;
        }  
    },
    methods: {      
      voteClicked(vote){
        axios
        .post('http://localhost:3000/rooms/vote', { roomId: this.roomId, userId: $cookies.get("userId"), vote: vote } )
        .then( res => {
          router.push( {name: 'room', params: { extRoomId: this.roomId } });
        });
      },
    }
}
</script>

<style>
ul.vote-list {
  list-style-type: none;
  margin: 0;
  padding: 0;
}
li.vote {
  display: inline;
  margin-right: 5px;
}
</style>