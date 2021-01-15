<template>
  <div class="pt-3">
       <loading :active.sync="isLoading" 
        :can-cancel="true" 
        :is-full-page="fullPage"></loading>
      <form id="createNewRoom" @submit="checkForm">
      <p v-if="errors.length" style="color : red; text-align : left;">
          <b>Please correct the following error(s):</b>
      <ul>
          <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      </p>
        <div class="mb-3 row">
            <label for="roomName" class="col-sm-4 col-form-label" >Room Name</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" id="roomName" v-model="roomName">
        </div>
        </div>

        <div class="mb-3 row">
            <label for="roomId" class="col-sm-4 col-form-label">Room Id</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" id="roomId" v-model="roomId">
        </div>
        </div>

        <div class="mb-3 row">
            <label for="currentTemperature" class="col-sm-4 col-form-label">Current temperature</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" id="currentTemperature" name="currentTemperature" v-model="currentTemperature">
        </div>
        </div>

        <div class="mb-3 row">
            <label for="targetTemperature" class="col-sm-4 col-form-label">Target Temperature :</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" id="targetTemperature" name="targetTemperature" v-model="targetTemperature">
        </div>
        </div>

        <div class="center-block">
        <button type="submit" class="btn btn-success me-2" @click="ajouterRoom">Add room</button>
        </div>
  </form>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';
import Loading from 'vue-loading-overlay';
import 'vue-loading-overlay/dist/vue-loading.css';

export default {
  name: 'CreateNewRoom',
  props: ['valueCreateNewRoom'],
  
  data: function() {
    return {
        errors: [],
        roomName: null,
        roomId: null,
        currentTemperature: null,
        targetTemperature: null,
        buildingId: -6,
        showModal:false,
        isLoading:false,
        fullPage: true
    }
  },
  components: {
            Loading
  },
  methods: {
    ajouterRoom(){
      console.log(this.valueCreateNewRoom);
      
    },

    async checkForm(e) {
      if (this.roomName && this.roomId && this.currentTemperature && this.targetTemperature) {
        this.isLoading=true;
        const json_request = JSON.stringify({ roomId :parseInt(this.roomId), name : this.roomName, currentTemperature : parseFloat(this.currentTemperature), targetTemperature : parseFloat(this.targetTemperature), buildingId:-6 });
        console.log(json_request);
        console.log(json_request);
        axios.post(`${API_HOST}/api/rooms/`, json_request,  {
            headers: {
                'Content-Type': 'application/json'
            }}).then((response) => {
              this.isLoading=false;
                console.log(response);
                this.errors= [];
    this.roomName= null;
    this.roomId= null;
    this.currentTemperature= null;
    this.targetTemperature= null;
    this.showModal = true; 
    this.$emit('updateDisplay', false)
            }, (error) => {
                console.log(error);
            });


      }
      this.errors = [];

      if (!this.currentTemperature) {
        this.errors.push('Current temperature required.');
      }
      if (!this.targetTemperature) {
        this.errors.push('Target temperature required.');
      }
      if (!this.roomName) {
        this.errors.push('Room name required.');
      }
      if (!this.roomId) {
        this.errors.push('Room Id required.');
      }

      e.preventDefault();
      
    }

  }
}
</script>