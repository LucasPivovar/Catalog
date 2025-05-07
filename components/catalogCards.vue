<template>
  <div class="main">
    <section class="models grid">
       <div 
         v-for="item in paginatedModels"
         :key="item.id" 
         @click="getModel(item.id)"
         class="model"
         :class="item.subscription"
       >
         <img :src="item.img" :alt="item.name" class="model_img">
         <div class="model_info">
           <h2 class="model_title">{{ item.name }}</h2>
           <p class="model_age">{{ item.age }} anos</p>
           <div class="carrer">
            <span>{{ item.carrer[0].carrer1 }}</span>
            <span>{{ item.carrer[0].carrer2 }}</span>
           </div>
         </div>
       </div>
    </section>

    <!-- Paginação -->
    <div class="pagination">
      <button 
        @click="goToPage(currentPage - 1)" 
        :disabled="currentPage === 1"
        class="pagination-button"
      >
        &laquo; Anterior
      </button>
      
      <div class="page-numbers">
        <button 
          v-for="page in totalPages" 
          :key="page" 
          @click="goToPage(page)"
          class="page-number"
          :class="{ active: page === currentPage }"
        >
          {{ page }}
        </button>
      </div>
      
      <button 
        @click="goToPage(currentPage + 1)" 
        :disabled="currentPage === totalPages"
        class="pagination-button"
      >
        Próxima &raquo;
      </button>
    </div>

    <section class="modal" v-if="model" @click.self="closeModel">
      <div class="modal_container">
        <div class="model_galery">
          <img :src="currentImage" alt="model" class="div1">
          <div class="galery_footer">
            <img :src="model.images[0].imagem1" alt="model" class="img_preview" @click="changeImage(model.images[0].imagem1)">
            <img :src="model.images[0].imagem2" alt="model" class="img_preview" @click="changeImage(model.images[0].imagem2)">
            <img :src="model.images[0].imagem3" alt="model" class="img_preview" @click="changeImage(model.images[0].imagem3)">
          </div>
        </div>

        <div class="model_details">
          <div class="model_header">
            <h2 class="model_title">{{ model.name }} </h2>
            <p class="model_age">{{ model.age }} anos</p>
            
            <div class="modal_carrer">
              <span>{{ model.carrer[0].carrer1 }}</span>
              <span>{{ model.carrer[0].carrer2 }}</span>
            </div>
          </div>
          <div class="model_main">
            <h3>Biografia</h3>
            <p>{{ model.biography }}</p>
            
            <h3>Contato</h3>
            <div class="contact_links">
              <a 
                v-if="model.contact && model.contact[0].whatsapp" 
                :href="model.contact[0].whatsapp" 
                target="_blank" 
                class="contact_link whatsapp"
              >
                WhatsApp
              </a>
              
              <a 
                v-if="model.contact && model.contact[0].telegram" 
                :href="model.contact[0].telegram" 
                target="_blank" 
                class="contact_link telegram"
              >
                Telegram
              </a>
              
              <a 
                v-if="model.contact && model.contact[0].instagram" 
                :href="model.contact[0].instagram" 
                target="_blank" 
                class="contact_link instagram"
              >
                Instagram
              </a>
            </div>
          </div>
        </div>
        
        <button class="close_button" @click="closeModel">×</button>
      </div>
    </section>
  </div>
</template>

<script>
// Import the CSS directly in the script
import '@/assets/css/catalog_cards.css';
import '@/assets/css/global.css';

export default {
  name: 'ModelGallery',
  emits: ['models-loaded'],

  data() {
    return {
      models: [],
      model: null,
      currentImage: null,
      isLoading: false,
      error: null,
      // Paginação
      currentPage: 1,
      modelsPerPage: 10
    }
  },

  created() {
    this.fetchModels()
  },
  
  computed: {
    // Calcular total de páginas
    totalPages() {
      return Math.ceil(this.models.length / this.modelsPerPage)
    },
    
    // Filtrar modelos para a página atual
    paginatedModels() {
      const start = (this.currentPage - 1) * this.modelsPerPage
      const end = start + this.modelsPerPage
      return this.models.slice(start, end)
    }
  },

  methods: {
    goToPage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page
        window.scrollTo({ top: 0, behavior: 'smooth' })
      }
    },
    
    fetchModels() {
      this.isLoading = true;
      this.error = null;
      
      fetch('./api/models.json')
        .then(response => {
          if (!response.ok) {
            throw new Error('Failed to fetch models');
          }
          return response.json();
        })
        .then(data => {
          this.models = data;
          this.isLoading = false;
          this.$emit('models-loaded', this.models.length);
        })
        .catch(error => {
          this.error = error.message;
          this.isLoading = false;
          console.error('Error fetching models:', error);
        });
    },

    getModel(id) {
      this.isLoading = true;
      this.error = null;
      
      fetch(`./api/models/${id}/dados.json`)
        .then(response => {
          if (!response.ok) {
            throw new Error(`Failed to fetch model with ID: ${id}`);
          }
          return response.json();
        })
        .then(data => {
          this.model = data;
          this.currentImage = data.images[0].imagem1;
          this.isLoading = false;
          
          document.body.style.overflow = 'hidden';
        })
        .catch(error => {
          this.error = error.message;
          this.isLoading = false;
          console.error(`Error fetching model ${id}:`, error);
        });
    },
    
    changeImage(imageUrl) {
      this.currentImage = imageUrl;
    },

    closeModel() {
      this.model = null;
      document.body.style.overflow = '';
    },

    getModelsCount() {
      return this.models.length;
    }
  }
}
</script>