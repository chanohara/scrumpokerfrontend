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
<br>
<div>
  <button class="btn btn-indigo outline" v-if="isAdmin(ownerId)" @click="reveal()">Reveal</button>   
</div>
<br>
<div>
  <p v-if="revealed == 1">
    The average vote is: {{ averageVote }}
  </p>
  <p v-else>
    Voting in process.
  </p>
</div>

</template>

<script>
export default {
    data() {
        return {
            revealed: false,
            users: [ { name: "Alex", vote: 99 } ],
            ownerId: 1,
            roomId: this.extRoomId,
            fibonacci: [1,2,3,5,8,13,21,34,55,89],
            connection: null,
            averageVote: 0
        }
    },
    props: ['extRoomId'],
    async created() {
        console.log("Starting connection to WebSocket Server")
        this.connection = new WebSocket("ws://localhost:3000/")

        const v = this;

        this.connection.onmessage = function(event) {
          axios.get("http://localhost:3000/rooms/" + v.roomId)
            .then( res => {
              v.users = [];
              for (let i in res.data) {
                v.users.push( { name: res.data[i].name, vote: res.data[i].current_vote });
                v.revealed = res.data[i].revealed;
              }
            });
        }

        this.connection.onopen = function(event) {
          console.log("Successfully connected to the echo websocket server...")
        }
        await this.loadData();          
    },
    methods: {      
      voteClicked(vote){
        axios
          .post('http://localhost:3000/rooms/vote', { roomId: this.roomId, userId: $cookies.get("userId"), vote: vote } )
          .then( res => {
            router.push( {name: 'room', params: { extRoomId: this.roomId } });
          });
      },
      async loadData() { 
        const response = await axios.get("http://localhost:3000/rooms/" + this.roomId);
        this.users = [];
        for (let i in response.data) {
            this.users.push( { name: response.data[i].name, vote: response.data[i].current_vote });
            this.revealed = response.data[i].revealed;
            this.ownerId = response.data[i].ownerId;
            this.averageVote = response.data[i].average_vote;
        }  
      },
      reveal(){
        axios
        .post('http://localhost:3000/rooms/reveal', { roomId: this.roomId } )
        .then( res => {
          router.push( {name: 'room', params: { extRoomId: this.roomId } });
        });
      },       
      isAdmin(user){
        if(user == $cookies.get("userId")){
            return true;
          } else {
            return false;
          }
      }
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