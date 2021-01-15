<template>
<div>

<div v-if="showModal" class="alert alert-success alert-dismissible fade show" role="alert"> 
  <strong>Your room has been added!</strong>
  <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click="dismissModal"> 
    <span aria-hidden="true">&times;</span>
  </button>
</div>
  <div v-if="!createRoom">
    <room-list-item 
      v-for="room in rooms"
      :room="room"
      :key="room.id"  
      @room-deleted="deleteRoom"
    >
    </room-list-item>
    <button type="button" class="btn btn-success me-2" @click="createRoomNew">Create new room</button>
  </div>

  <div v-if="createRoom">
      <create-new-room :value-create-new-room="createRoom" @updateDisplay="updateValueDispal"></create-new-room>
    </div>


</div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import RoomListItem from './RoomListItem';
import CreateNewRoom from './CreateNewRoom';
export default {
  components: {
    RoomListItem,
    CreateNewRoom
  },
  name: 'RoomList',
  data: function() {
    return {
      /* Initialize rooms with an empty array, while waiting for actual data to be retrieved from the API */
      rooms: [],
      createRoom : false, 
      showModal : false
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/rooms`);
    let rooms = response.data;
    this.rooms = rooms;
  },
  methods: {
    updateRoom(newRoom) {
      /* Find the place of room objectw ith the same Id in the array, and replace it */
      let index = this.rooms.findIndex(room => room.id === newRoom.id);
      this.rooms.splice(index, 1, newRoom);
    },
    deleteRoom(id) {
      /* Find the place of room objectw ith the same Id in the array, and replace it */
      let index = this.rooms.findIndex(room => room.id === id);
      this.rooms.splice(index, 1);
    },
    createRoomNew(){
      this.createRoom = !this.createRoom;
    },
    async updateValueDispal(){
      this.createRoom = !this.createRoom;
      this.showModal = true ;

      let response = await axios.get(`${API_HOST}/api/rooms`);
      let rooms = response.data;
      this.rooms = rooms;

      
    },
    dismissModal(){
      this.showModal = false ;
    }
  }
}
</script>