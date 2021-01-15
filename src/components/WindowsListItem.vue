<template>
  <div class="window border border-secondary rounded p-2 mb-2" :class="{expanded: isExpanded}">
    <div class="top-row d-flex" @click="toggleExpand">
      <div class="window-name fw-bold pe-3">{{window.name}}</div>
      <div class="room-name text-muted">{{window.roomName}}</div>

      <div class="open-status ms-4" :class="{open: isWindowOpen, closed: !isWindowOpen}">
        <template v-if="isWindowOpen">
          <span class="icon">&#x2B24;</span> Open
        </template>
        <template v-else>
          <span class="icon">&#x2716;</span> Closed
        </template>
      </div>

      <div class="expand-button ms-auto">
        {{ isExpanded ? '&#9660;' : '&#9658;' }}
      </div>
    </div>
    <template v-if="isExpanded">
      <hr/>
      
      <div class="mb-3 row" v-if="renameName">
        <label for="namewindow" class="col-sm-4 col-form-label">New Name</label>
        <div class="col-sm-8">
            <input type="text" class="form-control" id="namewindow" :placeholder="window.name"  v-model="newNamewinodw">
        </div>
      </div>

      <div class="details d-flex">
        <button type="button" class="btn btn-secondary me-2" @click="switchWindow">{{ isWindowOpen ? 'Close' : 'Open' }} window</button>
        <button type="button" class="btn btn-danger me-2" @click="deleteWindow">Delete window</button>
        <div v-if="!renameName">
          <button type="button" class="btn btn-warning me-2" @click="getRenameWindow">Update Window</button>
        </div>

        <div v-if="renameName">
          <button type="button" class="btn btn-warning me-2" @click="renameWindow">Rename Window</button>
        </div>

      </div>
    </template>
  </div>
</template>

<script>
import axios from 'axios';
import {API_HOST} from '../config';

export default {
  name: 'WindowsListItem',
  props: ['window'],
  data: function() {
    return {
      isExpanded: false,
      renameName: false,
      newNamewinodw:''
    }
  }, 
  computed: {
    isWindowOpen: function() {
      return this.window.windowStatus === 'OPEN'; 
    }
  },
  methods: {
    toggleExpand() {
      this.isExpanded = !this.isExpanded;
    },
    async switchWindow() {
      let response = await axios.put(`${API_HOST}/api/windows/${this.window.id}/switch`);
      let updatedWindow = response.data;
      this.$emit('window-updated', updatedWindow);
    },

    async deleteWindow() {
      let response = await axios.delete(`${API_HOST}/api/windows/${this.window.id}`);
      location.reload();
    },
    getRenameWindow(){
      this.renameName = true;
    },
    renameWindow(){
      console.log("I am here");
      console.log(this.newNamewinodw);

      const json_request = JSON.stringify({ id:this.window.id, name: this.newNamewinodw, roomId :parseInt(this.window.roomId), roomName : this.window.roomName, windowStatus:this.window.windowStatus });
        console.log(json_request);
        axios.post(`${API_HOST}/api/windows/`, json_request,  {
            headers: {
                'Content-Type': 'application/json'
            }}).then((response) => {
                console.log(response);
                // this.winodw.name = this.newNamewinodw;
                this.renameName = false;
                }, (error) => {
                    console.log(error);
                });     
      

    }
  }
}
</script>

<style lang="scss" scoped>

.open-status {
  .icon {
    position: relative;
  }

  &.open {
    color: #198754;
    .icon {
      font-size: 12px;
      top: -3px;
    }
  }

  &.closed {
    color: #dc3545;
  }
}

.window {
  .top-row {
    cursor: pointer;
  }
}
</style>
