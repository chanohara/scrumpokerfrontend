<script setup>
</script>

<template>
  <main>
    <div> <p> <h1>Scrum Planning Poker</h1> </p> </div>
    <button class="btn btn-indigo outline" @click="createRoom">Create Room
    </button>
    <br>
    <br>
    <button class="btn btn-indigo outline" @click="joinRoom">Join Room
    </button>
    <input v-model="message" placeholder="Enter Room ID">
  </main>
</template>

<script>
    export default {
        data() {
            return {

            }
        },
         methods: {
           createRoom(){
              alert('Created');
           },
           joinRoom(){
             alert('Joined');
           } 
         }
    }
</script>
