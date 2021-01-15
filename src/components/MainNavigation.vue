<template>
  <ul class="nav nav-tabs">
    <li class="nav-item">
      <a class="nav-link" @click="windowTrigger"  v-bind:class="{active: windowsActive}" href="#">Windows</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" @click="roomTrigger" v-bind:class="{active: roomsActive}" href="#" tabindex="-1">Rooms</a>
    </li>
  </ul>
</template>


<script>
export default {
  name: 'MainNavigation',
  data: function() { 
    return { windowsActive: true, roomsActive: false}
  },
  methods: {
    windowTrigger() {
      this.windowsActive=true;
      this.roomsActive=false;
      this.$emit("update-display","windows");
    },
    roomTrigger() {
      this.windowsActive=false;
      this.roomsActive=true;
      this.$emit("update-display","rooms");
    }
  } 
}
</script>