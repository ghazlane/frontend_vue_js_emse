<template>
  <div class="pt-3">
    <loading :active.sync="isLoading" 
        :can-cancel="true" 
        :is-full-page="fullPage"></loading>

      <form id="createNewWindow" @submit="checkForm">
     <p v-if="errors.length" style="color : red; text-align : left;">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
    </ul>
  </p>

    <div class="mb-3 row">
        <label for="namewindow" class="col-sm-4 col-form-label">Name</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" id="namewindow" v-model="namewinodw">
        </div>
    </div>

    <div class="mb-3 row">
        <label for="statusWindow" class="col-sm-4 col-form-label">Status window</label>
        <div class="col-sm-8">
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" name="statusWindow" id="statusWindowopen" v-model="statusWindow" value="OPEN">
                <label class="form-check-label" for="statusWindowopen">Open</label>
            </div>
            <div class="form-check form-check-inline">
                <input class="form-check-input" type="radio" name="statusWindow" id="statusWindowclose" v-model="statusWindow" value="CLOSED">
                <label class="form-check-label" for="statusWindowclose">Close</label>
            </div>
        </div>
    </div>

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

    <div class="center-block">
        <button type="submit" class="btn btn-success me-2" @click="ajouterWindow">Add window</button>
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
  name: 'CreateNewWindow',
  props: ['valueCreateNewWindow'],
  
  data: function() {
    return {
        errors: [],
        namewinodw: null,
        statusWindow: null,
        roomName: null,
        roomId: null,
        showModal:false,
        isLoading:false,
        fullPage: true
    }
  },
  components: {
            Loading
  },
  methods: {
    ajouterWindow(){
      console.log(this.valueCreateNewWindow);
    },

    async checkForm(e) {
      if (this.namewinodw && this.statusWindow && this.roomName && this.roomId) {
        this.isLoading=true;
        const json_request = JSON.stringify({ name: this.namewinodw, roomId :parseInt(this.roomId), roomName : this.roomName, windowStatus:this.statusWindow });
        console.log(json_request);
        axios.post(`${API_HOST}/api/windows/`, json_request,  {
            headers: {
                'Content-Type': 'application/json'
            }}).then((response) => {
                console.log(response);
                this.errors= [];
                this.namewinodw= null;
                this.statusWindow= null;
                this.roomName= null;
                this.roomId= null;
                this.showModal = true; 
                this.isLoading=false;
                this.$emit('updateDisplay', false)
                }, (error) => {
                    console.log(error);
                });     
      }
      this.errors = [];

      if (!this.namewinodw) {
        this.errors.push('Name required.');
      }
      if (!this.statusWindow) {
        this.errors.push('status Window required.');
      }
      if (!this.roomName) {
        this.errors.push('room name required.');
      }
      if (!this.roomId) {
        this.errors.push('room Id required.');
      }

      e.preventDefault();
      
    }

  }
}
</script>