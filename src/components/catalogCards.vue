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

```css
<style scoped>
.main {
  /* Cores primárias e variantes */
  --primary-color: rgb(243, 80, 80);     /* Vermelho principal usado em destaque e botões */
  --primary-hover: rgb(209, 13, 13);     /* Tom mais escuro para efeitos hover */
  
  /* Cores de fundo */
  --dark-bg: rgb(37, 37, 37);            /* Fundo escuro para cards e botões */
  --darker-bg: rgb(32, 32, 32);          /* Fundo escuro para modais e containers */
  --model-bg: rgb(128, 125, 125);        /* Fundo cinza para os cards de modelo */
  --modal-overlay: rgba(0, 0, 0, 0.7);   /* Sobreposição escura para modais */
  
  /* Cores de texto */
  --text-color: rgb(255, 255, 255);      /* Branco para texto principal */
  --text-light: rgb(180, 180, 180);      /* Cinza claro para texto secundário */
  
  /* Cores premium */
  --gold: gold;                          /* Dourado para elementos premium */
  --gold-shadow: rgba(255, 215, 0, 0.7); /* Sombra dourada para elementos premium */
  
  /* Cores de plataformas sociais */
  --whatsapp: rgb(91, 199, 108);         /* Verde WhatsApp */
  --whatsapp-hover: rgb(59, 133, 70);    /* Verde escuro para hover */
  --telegram: rgb(47, 93, 185);          /* Azul Telegram */
  --telegram-hover: rgb(24, 55, 116);    /* Azul escuro para hover */
  --instagram-gradient: linear-gradient(to right, rgb(240, 163, 255), rgb(243, 80, 80)); /* Gradiente Instagram */
  --instagram-hover: linear-gradient(to right, rgb(230, 112, 253), rgb(238, 72, 72));   /* Gradiente hover */

  /* Estrutura principal */
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

/***************************************
 * ESTRUTURA PRINCIPAL
 ***************************************/
/* Grid de modelos */
.models.grid {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 20px;
  width: 100%;
  max-width: 1600px;
  margin-left: auto;
  margin-right: auto;
  padding: 20px;
}

/***************************************
 * CARDS DE MODELOS
 ***************************************/
/* Estilo básico do card */
.model {
  width: 250px;
  height: 444px;
  background-color: var(--model-bg);
  position: relative;
  overflow: hidden;
  border-radius: 15px;
  cursor: pointer;
  margin: 0; 
}

.model:hover {
  box-shadow: 0 10px 10px rgba(145, 0, 0, 0.2);
  animation: bottom-shadow 0.5s ease-in-out;
}

.model img {
  width: 100%;
  height: 320px;
  transition: transform 0.5s;
  object-fit: cover;
}

.model_img:hover {
  transform: scale(1.05);
  transition: transform 0.5s;
}

/* Estilo para cards premium */
.model.premium {
  position: relative;
  box-shadow: 0 0 15px var(--gold-shadow);
  border: 2px solid var(--gold);
  animation: premium-blink 1s infinite;
}

.model.premium::before {
  content: "";
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  border-radius: 17px; 
  background: linear-gradient(45deg, var(--gold), #ff9900, #ffcc00, var(--gold));
  background-size: 400% 400%;
  z-index: -1;
}

.model.premium::after {
  content: url("../assets/premium.svg");
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: var(--gold);
  z-index: 1;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Informações sobre o modelo */
.model_info {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: var(--dark-bg);
  height: 124px;
  color: var(--text-color);  
  padding: 20px 10px;
  text-align: left;
  font-family: "Roboto";
  font-size: 13px;
}

.model_info p {
  font-size: 15px;
  font-weight: 300;
  color: var(--text-light);
}

.carrer {
  display: flex;
  gap: 1rem;
}

span {
  background: var(--primary-color);
  color: var(--text-color);
  padding: 5px 12px;
  font-size: 12px;
  font-weight: 400;
  border-radius: 15px;
  font-family: "inter";
  margin-top: 1rem;
}

/***************************************
 * SISTEMA DE PAGINAÇÃO
 ***************************************/
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 30px 0;
  width: 100%;
  gap: 15px;
}

/* Botões de navegação (anterior/próximo) */
.pagination-button {
  background-color: var(--dark-bg);
  color: var(--text-color);
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
  font-family: "Roboto", sans-serif;
  transition: background-color 0.3s;
}

.pagination-button:hover:not(:disabled) {
  background-color: var(--primary-color);
}

.pagination-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.page-numbers {
  display: flex;
  gap: 8px;
}

.page-number {
  width: 35px;
  height: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--dark-bg);
  color: var(--text-color);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  font-family: "Roboto", sans-serif;
  transition: all 0.3s;
}

.page-number:hover {
  background-color: rgb(70, 70, 70);
}

.page-number.active {
  background-color: var(--primary-color);
  font-weight: bold;
}

/***************************************
 * MODAL DE DETALHES
 ***************************************/
/* Container do modal */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: var(--modal-overlay);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  overflow: auto; /* Permite rolagem no modal */
}

/* Conteúdo do modal */
.modal_container {
  background: var(--darker-bg);
  border-radius: 8px;
  max-width: 900px;
  height: 600px;
  max-height: 90%;
  overflow-y: auto;
  position: relative;
  display: flex;
  flex-direction: row;
}

/* Seção de galeria de imagens */
.model_galery {
  display: flex;
  flex-direction: column;
  width: 50%;
  height: 100%;
  overflow: hidden;
  border-radius: 15px;
}

.div1 {
  width: 100%;
  height: 70%;
  object-fit: cover;
  padding: 20px;
  border-radius: 30px;
}
.galery_footer {
  display: flex;
  justify-content: space-between;
  width: 100%;
  height: 30%;
  padding: 0px 0 20px 20px;
  gap: 20px;
}

/* Miniaturas de imagens */
.img_preview {
  width: 100%;
  object-fit: cover;
  border-radius: 15px;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.img_preview:hover {
  transform: scale(1.05);
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}

/* Seção de detalhes do modelo */

.model_details {
  padding: 20px;
  width: 50%;
  height: 100%;
  font-family: "Inter";
  color: var(--text-color);
}

.model_details h2 {
  font-size: 2rem;
}

.model_details h3 {
  font-size: 1.5rem;
  margin-top: 2rem;
}

.model_details p {
  color: var(--text-light);
  font-size: 1.1rem;
}

.modal_carrer {
  margin-top: 0.5rem;
  gap: 0.5rem;
  display: flex;
}

/* Seção de contato */
.contact {
  margin-top: 2rem;
}

.contact_links {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  align-items: center;
}

.contact_link {
  width: 100%;
  text-align: center;
  text-decoration: none;
  padding: 15px;
  border-radius: 25px;
  color: var(--text-color);
  transition: all 0.3s ease;
}

/***************************************
 * LINKS DE PLATAFORMAS SOCIAIS
 ***************************************/
/* WhatsApp */
.whatsapp {
  background: var(--whatsapp);
}

.whatsapp:hover {
  background: var(--whatsapp-hover);
}

/* Telegram */
.telegram {
  background: var(--telegram);
}

.telegram:hover {
  background: var(--telegram-hover);
}

/* Instagram */
.instagram {
  background: var(--instagram-gradient);
}

.instagram:hover {
  background: var(--instagram-hover);
}

/* Botão de fechar o modal */
.close_button {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  top: 10px;
  right: 10px;
  width: 30px;
  height: 30px;
  font-size: 30px;
  border-radius: 50%;
  border: none;
  cursor: pointer;
  background: var(--primary-color);
  color: var(--text-color);
}

.close_button:hover {
  background: var(--primary-hover);
}

/***************************************
 * ANIMAÇÕES
 ***************************************/
/* Efeito de piscada para elementos premium */
@keyframes premium-blink {
  0% {
    box-shadow: 0 0 5px rgba(255, 215, 0, 0.3);
  }
  50% {
    box-shadow: 0 0 10px rgba(255, 215, 0, 1);
  }
  100% {
    box-shadow: 0 0 5px rgba(255, 215, 0, 0.3);
  }
}

/***************************************
 * RESPONSIVIDADE
 ***************************************/
@media (max-width: 768px) {
  /* Ajustes dos cards para telas menores */
  .model {
    width: 100%;
    height: 400px;
  }
  
  .model img {
    height: 280px;
  }
  
  .model_info {
    height: 120px;
  }
  
  .carrer {
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  
  /* Ajustes do modal para telas menores */
  .modal {
    align-items: flex-start;
    padding: 10px 0;
    overflow-y: scroll !important;
  }
  
  .modal_container {
    max-width: 95%;
    height: auto;
    max-height: 180vh;
    margin: 10px auto 50px auto; 
    flex-direction: column;
    overflow-y: visible; 
  }

  .model_galery {
    width: 100%;
  }
  
  .div1 {
    width: 100%;
    height: auto;
    min-height: 300px;
    max-height: none; /* Remove restrição de altura */
    padding: 10px;
    object-fit: contain; /* Garante que a imagem inteira seja visível */
  }
  
  .galery_footer {
    padding: 10px;
    gap: 10px;
    width: 100%;
    height: auto;
    object-fit: cover;
    display: flex;
    flex-direction: row;
  }

  .img_preview {
    width: 100%;
    height: 100px;
    object-fit: cover;
    width: 100px;
    max-width: 300px;
  }
  
  .model_details {
    padding: 15px;
    width: 100%;
    height: auto;
  }
  
  /* Responsividade para a paginação */
  .pagination {
    flex-direction: column;
    gap: 10px;
  }
  
  .page-numbers {
    flex-wrap: wrap;
    justify-content: center;
  }
}
</style>
```