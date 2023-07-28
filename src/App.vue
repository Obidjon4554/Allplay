<template>
  <TheNavbar />
  <RouterView />
</template>

<script>
import TheNavbar from './components/TheNavbar.vue';

export default {
  components: { TheNavbar }
}
</script>

<style>
body {

  background-color: #000;
  color: #fff;
  font-family: "Littera Text", helvetica, arial, sans-serif;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  margin: 0 auto;
  text-align: left;
  justify-content: center;
  width: 100%;
  min-height: 100vh;
  align-items: center;
  }
</style>