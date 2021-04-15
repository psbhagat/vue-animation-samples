<template>
  <router-view v-slot="slotProps">
    <transition name="route" mode="out-in">
      <component :is="slotProps.Component"></component>
    </transition>
  </router-view>

  <div class="container">
    <users-list></users-list>
  </div>
  <div class="container">
    <transition
      :css="false"
      @before-enter="beforeEnter"
      @before-leave="beforeLeave"
      @enter="enter"
      @after-enter="afterEnter"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
    >
      <p v-if="paraVisible">This is only visible sometimes....</p>
    </transition>
    <button @click="toggleParagraph">Toggle Paragraph</button>
  </div>

  <div class="container">
    <transition name="fade-button" mode="out-in">
      <button @click="showUsers" v-if="!usersAreVisible">Show Users</button>
      <button @click="hideUsers" v-else>Hide Users</button>
    </transition>
  </div>

  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>

  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>  

<script>
import UsersList from './components/ListData.vue';
export default {
  components: { UsersList },
  data() {
    return {
      dialogIsVisible: false,
      paraVisible: false,
      usersAreVisible: false,
      enterInterval: 0,
    };
  },
  methods: {
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    toggleParagraph() {
      this.paraVisible = !this.paraVisible;
    },
    showUsers() {
      this.usersAreVisible = true;
    },
    hideUsers() {
      this.usersAreVisible = false;
    },
    beforeEnter(e) {
      console.log('Before Enter!');
      console.log(e);
      e.style.opacity = 0;
    },
    beforeLeave(e) {
      console.log('Before Leave!');
      console.log(e);
    },
    enter(e, done) {
      console.log('Enter!');
      console.log(e);
      let round = 1;
      this.enterInterval = setInterval(() => {
        e.style.opacity = round * 0.1;
        round++;
        if (round > 10) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 100);
    },
    afterEnter(e) {
      console.log('After Enter!');
      console.log(e);
    },
    leave(e) {
      console.log('Leave!');
      console.log(e);
    },
    afterLeave(e) {
      console.log('After Leave!');
      console.log(e);
    },
    enterCancelled(e) {
      console.log('Enter Canncelled!');
      console.log(e);
      clearInterval(this.enterInterval);
    },
    leaveCancelled() {
      console.log('Leave Cancelled!');
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  /*transition: transform 0.3s ease-out;*/
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}
.animate {
  /*transform: translateX(-50px);*/
  animation: slide-fade 0.3s ease-out forwards;
}

.fade-button-enter-from {
  opacity: 0;
}

.fade-button-enter-active {
  transition: opacity 0.3s ease-in;
}
.fade-button-enter-to {
  opacity: 1;
}

.route-enter-from {
  opacity: 0;
}

.route-enter-active {
  transition: opacity 2s ease-in;
}

.router-enter-to {
  opacity: 1;
}
</style>