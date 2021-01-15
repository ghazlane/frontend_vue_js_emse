<template>
<div>

<div v-if="showModal" class="alert alert-success alert-dismissible fade show" role="alert"> 
  <strong>Your window has been added!</strong>
  <button type="button" class="close" data-dismiss="alert" aria-label="Close" @click="dismissModal"> 
    <span aria-hidden="true">&times;</span>
  </button>
</div>

    <div v-if="!createWindow">
      <windows-list-item
        v-for="window in windows"
        :window="window"
        :key="window.id"  
        @window-updated="updateWindow"
      >
      </windows-list-item>
      <button type="button" class="btn btn-success me-2" @click="createWindowNew">Create new Window</button>
    </div>
    
    <div v-if="createWindow">
      <create-new-window :value-create-new-window="createWindow" @updateDisplay="updateValueDispal"></create-new-window>
    </div>


  </div>
</template>


<script>
import axios from 'axios';
import {API_HOST} from '../config';
import WindowsListItem from './WindowsListItem';
import CreateNewWindow from './CreateNewWindow';

export default {
  components: {
    WindowsListItem,
    CreateNewWindow
  },
  name: 'WindowsList'
    ,
  data: function() {
    return {
      /* Initialize windows with an empty array, while waiting for actual data to be retrieved from the API */
      windows: [],
      createWindow : false, 
      showModal : false
    }
  },
  created: async function() {
    let response = await axios.get(`${API_HOST}/api/windows`);
    let windows = response.data;
    this.windows = windows;
  },
  methods: {
    updateWindow(newWindow) {
      /* Find the place of window objectw ith the same Id in the array, and replace it */
      let index = this.windows.findIndex(window => window.id === newWindow.id);
      this.windows.splice(index, 1, newWindow);
    },
    createWindowNew(){
      this.createWindow = !this.createWindow;
    },
    async updateValueDispal(){
      console.log("mon pere est terminé");
      this.createWindow = !this.createWindow;
      this.showModal = true ;

      let response = await axios.get(`${API_HOST}/api/windows`);
      let windows = response.data;
      this.windows = windows;

      
    },
    dismissModal(){
      this.showModal = false ;
    }
  }
}
</script>
