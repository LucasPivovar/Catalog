<template>
  <div class="main">
    <catalog-cards></catalog-cards>
  </div>
</template>

<script>
// @ is an alias to /src
import catalogCards from '@/components/catalogCards.vue'

export default {
  name: 'HomeView',
  components: {
    catalogCards
  }
}
</script>

<style scoped>
  .main{
    background: rgb(20, 20, 20);
    width: 100%;
    min-height: 100vh;
  }

</style>
