<template>
  <nav>
    <router-link to="/">
      <img src="@/assets/logo.png" />
    </router-link>
    <div class="hamburger" @click="emitToggleMenu"><div></div></div>
  </nav>
</template>

<script>
export default {
  methods: {
    emitToggleMenu() {
      this.$emit('toggleMenu');
    },
  },
};
</script>

<style lang="scss" scoped>
nav {
  position: fixed;
  z-index: 5;
  display: flex;
  justify-content: space-between;
  width: 100%;
  background: linear-gradient(135deg, var(--white) 0% 20%, var(--red) 20% 80%);
  box-shadow: 0px 0px 2px var(--dark);
  padding: 10px 20px;
  a {
    display: flex;
    align-items: center;
  }
  .hamburger {
    display: none;
  }
  @media only screen and (max-width: 425px) {
    background: linear-gradient(135deg, var(--white) 0% 70%, var(--red) 70%);
    .hamburger {
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: var(--white);
      color: var(--red);
      border: 0;
      border-radius: 3px;
      padding: 8px;
      > div {
        position: relative;
        width: 1.5em;
        height: 3px;
        border-radius: 3px;
        background-color: var(--red);
        &:before ,&:after {
          content: '';
          position: absolute;
          width: inherit;
          height: inherit;
          border-radius: 3px;
          background-color: var(--red);

        }
        &:before {
          transform: translateY(-7px);
        }
        &:after {
          transform: translateY(7px);
        }

      }
    }
  }
}
</style>
