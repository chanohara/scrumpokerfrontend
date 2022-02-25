<template>
  You are {{ userName }} in the room {{ roomId }}. Please give your vote
</template>

<script>
export default {
    data() {
        return {
            userName: 'Dunno',
            roomId: this.extRoomId
        }
    },
    props: ['extRoomId'],
    created() {
        console.log(this.extRoomId);
    }
}
</script>

<style>

</style>