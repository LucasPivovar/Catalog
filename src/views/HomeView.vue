<template>   
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="../assets/css/catalog_cards.css">
  <body>     
    <div class="main">   
      <div class="content_home">    
        <header>         
          <h1><strong>Studio</strong> Catalog</h1>         
          <p>
            <img src="../assets/icons/people.svg" alt="">
             {{ modelsCount }} modelos
          </p>       
        </header>       
        <catalog-cards          
          ref="catalogCardsRef"         
          @models-loaded="updateModelsCount"       
        ></catalog-cards>     
      </div>
    </div>      
  </body> 
  <a href="/access" class="access">Acessar</a>
  <a href="/dashboard" class="dashboard">Dashboard</a>
</template>  

<script>
import catalogCards from '@/components/catalogCards.vue';
import '@/assets/css/global.css';
import '@/assets/css/home_view.css';

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
    // Give child component time to initialize     
    this.$nextTick(() => {       
      this.checkModelsCount();     
    });   
  },      
  
  methods: {     
    // Update models count when event is emitted from child     
    updateModelsCount(count) {       
      this.modelsCount = count;       
      console.log('Quantidade atualizada:', count);     
    },          
    
    // Check models count if event wasn't triggered     
    checkModelsCount() {       
      if (this.$refs.catalogCardsRef) {         
        const count = this.$refs.catalogCardsRef.getModelsCount();         
        if (count > 0) {           
          this.modelsCount = count;         
        } else {           
          // Keep checking until we get models           
          setTimeout(() => {             
            this.checkModelsCount();           
          }, 500);         
        }       
      } else {         
        // Reference not available yet, try again         
        setTimeout(() => {           
          this.checkModelsCount();         
        }, 200);       
      }     
    }   
  } 
} 
</script>  