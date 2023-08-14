<template>
  <main>
    <Banner />
    <div class="main-container">
      <h1>Create your burger: </h1>
      <BurgerForm />
    </div>
  </main>
</template>

<script>
  import Banner from '../components/Banner.vue';
  import BurgerForm from '../components/BurgerForm.vue';

  export default {
    name: 'HomeView',
    components: {
      Banner,
      BurgerForm
    }
  }
</script>

<style>

</style>
