<template>
  <body>
    <div class="main">
      <header>
        <h1><strong>Studio</strong> Catalog</h1>
        <p><span><img src="../assets/people.svg" alt=""></span>  {{ modelsCount }} modelos</p>
      </header>
      <catalog-cards 
        ref="catalogCardsRef"
        @models-loaded="updateModelsCount"
      ></catalog-cards>
    </div>
  </body>
</template>

<script>
// @ is an alias to /src
import catalogCards from '@/components/catalogCards.vue'

export default {
  name: 'HomeView',
  components: {
    catalogCards
  },
  
  data() {
    return {
      modelsCount: 0
    }
  },
  
  mounted() {
    // Alternativa: usar o método getModelsCount em vez de depender do evento
    setTimeout(() => {
      this.checkModelsCount();
    }, 500);
  },
  
  methods: {
    updateModelsCount(count) {
      this.modelsCount = count;
      console.log('Quantidade atualizada:', count);
    },
    
    checkModelsCount() {
      if (this.$refs.catalogCardsRef) {
        const count = this.$refs.catalogCardsRef.getModelsCount();
        if (count > 0) {
          this.modelsCount = count;
        } else {
          setTimeout(() => {
            this.checkModelsCount();
          }, 500);
        }
      } else {
        setTimeout(() => {
          this.checkModelsCount();
        }, 500);
      }
    }
  }
}
</script>

<style scoped>
  body{
    background: rgb(5, 5, 5);
    width: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    font-family: "inter";
  }

  .main{
    width: 100%;
    min-height: 100vh;
    max-width: 1400px;
  }

  header{
    display: flex;
    color: white;
    align-items: center;
    justify-content: space-between;
    margin-top: 1rem;
    padding: 0 35px;
    width: 100%;
  }

  header h1{
    font-weight: 400;
    font-size: 1.6rem;
  }

  header strong{
    font-weight: 400;
    color: rgb(216, 45, 45);
  }

  header p {
    font-size: 1.1rem;
    font-weight: 300;
    display: flex;
    justify-content: center;
    flex-direction: row;
    align-items: center;
    gap: 5px;
  }

  header p span{
    display: flex;
    justify-content: center;
    align-items: center;
  }

  header p span img{
    width: 25px;
  }


</style>