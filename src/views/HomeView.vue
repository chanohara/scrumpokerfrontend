<script setup>
import axios from 'axios'
</script>

<template>
  <main>
    <div> <p> <h1>Scrum Planning Poker</h1> </p> </div>
    <input v-model="name" placeholder="Enter User Name">
    <br>
    <br>
    <button class="btn btn-indigo outline" @click="createRoom">Create Room
    </button>
    <br>
    <br>
    <button class="btn btn-indigo outline" @click="joinRoom">Join Room
    </button>
    <input v-model="room" placeholder="Enter Room ID">
  </main>
</template>

<script>
    export default {
        data() {
            return {
              name : "Dennis"
              }
        },
         methods: {
           
           createRoom(){
              axios.post('http://localhost:3001/room', { this.name } );
           },
           joinRoom(){
             alert('Joined');
           } 
         }
    }
</script>
