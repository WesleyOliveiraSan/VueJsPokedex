<template>
  <div id="app">
    <Header @toggleMenu="toggleMenu" />
    <transition name="slide-fade">
      <div v-if="open" id="overlay" @click="toggleMenu">
        <router-link to="/"><div>Home</div></router-link>
      </div>
    </transition>
    <div id="routes">
      <router-view />
    </div>
  </div>
</template>

<script>
// Components
import Header from '@/components/Header.vue';

export default {
  components: {
    Header,
  },
  data() {
    return {
      open: false,
    };
  },
  methods: {
    toggleMenu() {
      this.open = !this.open;
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Signika&display=swap");
/* Custom Colors */
:root {
  --dark-red: #99292b;
  --red: #c7383a;
  --dark-yellow: #dcae3d;
  --yellow: #ffcb4c;
  --blue: #6bd0fa;
  --white: #fff;
  --dark: #222;
}

* {
  font-family: "Signika", sans-serif;
  margin: 0;
  padding: 0;
  outline: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

html,
body,
#app {
  min-height: 100%;
}

body {
  background-color: var(--white);
  color: var(--dark);
  -webkit-font-smoothing: antialiased !important;
}

body,
input,
button {
  color: var(--dark);
  font-size: 14px;
  font-family: Signika, Arial, Helvetica, sans-serif;
}

button {
  cursor: pointer;
}

ul {
  list-style-type: none;
}

input,
textarea,
button,
select,
a {
  outline: none;
  box-shadow: none;
  text-decoration: none;
  color: #222;
}

#routes {
  padding: 20px;
  padding-top: 80px;
}
#overlay {
  position: fixed;
  z-index: 5;
  margin-top: 8.8vh;
  width: 100%;
  height: 91.2vh;
  background-color: rgba(#c7383a, 0.9);
  transition: 0.3s;
  div{
    width: 100%;
    padding: 14px 6px;
    text-align: center;
    font-size: 18px;
  color: var(--white);
    &:hover{
      background-color: rgba(#99292b, 0.9);
    }
  }
}

.slide-fade-enter-active {
  transition: all .8s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
{
  transform: translateX(20px);
  opacity: 0;
}
</style>
