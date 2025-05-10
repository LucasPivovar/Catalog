<template>   
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
  <body>     
    <div class="main">   
      <div class="content_home">    
        <header>         
          <h1><strong>Studio</strong> Catalog</h1> 
          <div class="header-left">
            <p>
              <img src="../assets/icons/people.svg" alt="">
              {{ modelsCount }} modelos
            </p> 
            <a href="entrar" class="button">Acessar</a>
          </div>        
      
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
import '@/assets/css/home_view.css';
import '@/assets/css/global.css';

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

<style scoped>
  .main{
    font-family: "inter";   
    background: var(--bg-main);
  }    
  
  .content{     
    width: 100%;     
    min-height: 100vh;     
    max-width: 1400px;   
  }    
  
  header{     
    display: flex;     
    color: var(--text-primary);
    align-items: center;     
    justify-content: space-between;     
    margin-top: 1rem;     
    padding: 0 35px;     
    width: 100%;   
  }    

  .header-left{
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 20px;
  }
  
  header h1{     
    font-weight: 400;     
    font-size: 1.6rem;   
  }    
  
  header strong{     
    font-weight: 400;     
    color: var(--text-accent);
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
    background: transparent;
  }    
  
  header p span img{     
    width: 25px;   
  } 

  .button {
    color: var(--text-primary);
    text-decoration: none;
    transition: all 0.3s ease;
    font-weight: 500;
    padding: 15px;
    background: var(--primary);
    border-radius: 15px;
  }

</style>