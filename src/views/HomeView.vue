<script setup>
import axios from 'axios';
import cookies from 'vue-cookies';
import router from '../router';
</script>

<template>
  <main>
    <div><p><h1>Scrum Planning Poker</h1></p></div>
    <input v-model="name" placeholder="Enter User Name">
    <br>
    <br>
    <button class="btn btn-indigo outline" @click="createRoom">Create Room
    </button>
    <br>
    <br>
    <button class="btn btn-indigo outline" @click="joinRoom">Join Room
    </button>
    <input v-model="roomId" placeholder="Enter Room ID">
  </main>
</template>

<script>
    export default {
        data() {
            return {
              name : "Dennis",
              roomId: 0,
            }
        },
         methods: {
           
           createRoom(){
             axios
              .post('http://localhost:3000/rooms', { name: this.name } )
              .then( res => {
                let d = new Date();
                d.setTime(d.getTime() + 1 * 24 * 60 * 60 * 1000);
                let expires = "expires=" + d.toUTCString();
                $cookies.set("userId",res.data.userId,"1d");
                router.push( {name: 'room', params: { extRoomId: res.data.roomId } });
              });
           },
           joinRoom(){
             axios
              .post('http://localhost:3000/rooms/join', { roomId: this.roomId, name: this.name } )
              .then( res => {
                let d = new Date();
                d.setTime(d.getTime() + 1 * 24 * 60 * 60 * 1000);
                let expires = "expires=" + d.toUTCString();
                $cookies.set("userId",res.data.userId,"1d");    
                router.push({ name: "room", params: { extRoomId: this.roomId } } ); 
              });
           } 
         }
    }
</script>
