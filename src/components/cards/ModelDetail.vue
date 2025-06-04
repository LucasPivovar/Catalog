<<<<<<< HEAD
<template>
  <body>
    <main>
        <div class="back-button-container">
          <button @click="goBack" class="back-button">
            ← Voltar para a lista
          </button>
        </div>
        
        <div v-if="isLoading" class="loading">Carregando...</div>        
        <div v-else-if="error" class="error">Erro: {{ error }}</div>
        
        <div class="sidebox" v-else-if="model">
          <h1 class="model-title">{{ model.name }}</h1>
          <div class="content">
            <!-- Gallery Box -->
            <div class="box gallery-box">
              <img v-if="currentImage" :src="getImageUrl(currentImage)" alt="model" class="main-image">
              <div v-else class="no-image">Imagem não disponível</div>
              
              <div class="gallery-footer" v-if="images.length">
                <img 
                  v-for="(img, index) in images" 
                  :key="index"
                  :src="getImageUrl(img)" 
                  alt="model" 
                  class="img-preview" 
                  @click="changeImage(img)"
                  :class="{ active: currentImage === img }"
                >
              </div>
            </div>
            <div class="box details-box">
              <div class="model-main">
                <div class="contact-links" v-if="hasContacts">
                  <a 
                    v-for="(contact, type) in contacts" 
                    :key="type"
                    :href="contact" 
                    target="_blank" 
                    :class="['contact-link', type]"
                  >
                    {{ formatContactType(type) }}
                  </a>
                </div>
                <div class="review-section" v-if="hasComments">
                  <h3>Review</h3>
                  <div class="overall-rating">
                    <div class="stars" v-html="getStarsHtml(averageRating)"></div>
                    <div class="rating-text">{{ averageRating.toFixed(1) }}/5 ({{ model.comments.length }} avaliações)</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="reviews-container" v-if="hasComments">
          <h3 class="reviews-title">Avaliações</h3>
          <div class="reviews-list">
            <div 
              v-for="comment in model.comments" 
              :key="comment.id" 
              class="review-item"
            >
              <div class="review-stars" v-html="getStarsHtml(comment.stars)"></div>
              <p class="review-text">{{ comment.comment }}</p>
            </div>
          </div>
        </div>
    </main>
  </body>
</template>

<script>
import '@/assets/css/global.css'
import '@/assets/css/model_detail.css'

export default {
  name: 'ModelDetail',

  props: {
    modelId: {
      type: [String, Number],
      required: true
    }
  },

  emits: ['back-to-list'],
  
  data() {
    return {
      model: null,
      currentImage: null,
      isLoading: false,
      error: null,
      apiPaths: [
        `/api/models/{id}/dados.json`,
        `./api/models/{id}/dados.json`,
        `/public/api/models/{id}/dados.json`
      ]
    }
  },

  computed: {
    images() {
      const imageData = this.model?.images?.[0]
      if (!imageData) return []
      
      return [imageData.imagem1, imageData.imagem2, imageData.imagem3].filter(Boolean)
    },

    contacts() {
      const contactData = this.model?.contact?.[0]
      if (!contactData) return {}
      
      return Object.fromEntries(
        Object.entries(contactData).filter(([, value]) => value)
      )
    },

    hasContacts() {
      return Object.keys(this.contacts).length > 0
    },

    hasComments() {
      return this.model?.comments?.length > 0
    },

    averageRating() {
      if (!this.hasComments) return 0
      
      const totalStars = this.model.comments.reduce((sum, comment) => sum + comment.stars, 0)
      return totalStars / this.model.comments.length
    }
  },

  watch: {
    modelId: {
      handler: 'fetchModel',
      immediate: true
    }
  },

  methods: {
    async fetchModel() {
      if (!this.modelId) {
        this.setError('ID do modelo não fornecido')
        return
      }

      this.setLoadingState(true)
      
      for (const pathTemplate of this.apiPaths) {
        const path = pathTemplate.replace('{id}', this.modelId)
        
        try {
          const response = await fetch(path)
          if (response.ok) {
            const data = await response.json()
            this.setModelData(data)
            return
          }
        } catch (error) {
          continue
        }
      }
      
      this.setError('Modelo não encontrado')
    },

    setLoadingState(loading) {
      this.isLoading = loading
      if (loading) {
        this.error = null
      }
    },

    setError(message) {
      this.error = message
      this.isLoading = false
    },

    setModelData(data) {
      this.model = data
      this.currentImage = this.images[0] || null
      this.isLoading = false
    },
    
    changeImage(imageUrl) {
      this.currentImage = imageUrl
    },

    getImageUrl(imagePath) {
      if (!imagePath || imagePath.startsWith('http')) return imagePath || ''
      
      const cleanPath = imagePath.replace(/^\.\//, '')
      return cleanPath.startsWith('/') ? cleanPath : '/' + cleanPath
    },

    formatContactType(type) {
      return type.charAt(0).toUpperCase() + type.slice(1)
    },

    goBack() {
      this.$emit('back-to-list')
    },

    getStarsHtml(rating) {
      return Array.from({ length: 5 }, (_, i) => {
        const starClass = i + 1 <= rating ? 'star filled' : 'star'
        return `<div class="${starClass}">★</div>`
      }).join('')
    }
  }
}
</script>
=======
<template>
  <body>
    <main>
      <div class="container">
        <!-- Botão de voltar -->
        <div class="back-button-container">
          <button @click="goBack" class="back-button">
            ← Voltar para a lista
          </button>
        </div>
        
        <div v-if="isLoading" class="loading">Carregando...</div>        
        <div v-else-if="error" class="error">Erro: {{ error }}</div>

        <div class="sidebox" v-else-if="model">
          <h1 class="model-title">{{ model.name }}</h1>
          <div class="content">
            <!-- CORRIGIDO: box → div class="box" -->
            <div class="box gallery-box">
                <img v-if="currentImage" :src="getImageUrl(currentImage)" alt="model" class="main-image">
                <div v-else class="no-image">Imagem não disponível</div>
                
                <div class="gallery-footer" v-if="model.images?.[0]">
                <img 
                    v-for="(img, key) in getImages()" 
                    :key="key"
                    :src="getImageUrl(img)" 
                    alt="model" 
                    class="img-preview" 
                    @click="changeImage(img)"
                    :class="{ active: currentImage === img }"
                >
                </div>
            </div>
            
            <!-- CORRIGIDO: box → div class="box" -->
            <div class="box details-box">
              <div class="model-main">
                <div class="contact-links">
                  <a 
                    v-for="(contact, type) in getContacts()" 
                    :key="type"
                    :href="contact" 
                    target="_blank" 
                    :class="['contact-link', type]"
                  >
                    {{ type.charAt(0).toUpperCase() + type.slice(1) }}
                  </a>
                </div>

                <!-- Seção de Review -->
                <div class="review-section" v-if="model.comments && model.comments.length > 0">
                  <h3>Review</h3>
                  <div class="overall-rating">
                    <div class="stars" v-html="getStarsHtml(getAverageRating())"></div>
                    <div class="rating-text">{{ getAverageRating().toFixed(1) }}/5 ({{ model.comments.length }} avaliações)</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Seção de Avaliações -->
        <div class="reviews-container" v-if="model && model.comments && model.comments.length > 0">
          <h3 class="reviews-title">Avaliações</h3>
          <div class="reviews-list">
            <div 
              v-for="comment in model.comments" 
              :key="comment.id" 
              class="review-item"
            >
              <div class="review-stars" v-html="getStarsHtml(comment.stars)"></div>
              <p class="review-text">{{ comment.comment }}</p>
            </div>
          </div>
        </div>
      </div>
    </main>
  </body>
</template>

<script>
import '@/assets/css/global.css';
import '@/assets/css/model_detail.css';

export default {
  name: 'ModelDetail',

  props: {
    modelId: {
      type: [String, Number],
      required: true
    }
  },

  emits: ['back-to-list'],
  
  data() {
    return {
      model: null,
      currentImage: null,
      isLoading: false,
      error: null
    }
  },

  created() {
    this.fetchModel()
  },

  watch: {
    modelId() {
      this.fetchModel()
    }
  },

  methods: {
    async fetchModel() {
      const id = this.modelId;
      
      if (!id) {
        this.error = 'ID do modelo não fornecido';
        return;
      }

      this.isLoading = true;
      this.error = null;
      
      const paths = [
        `/api/models/${id}/dados.json`,
        `./api/models/${id}/dados.json`,
        `/public/api/models/${id}/dados.json`
      ];
      
      for (const path of paths) {
        try {
          const response = await fetch(path);
          if (response.ok) {
            const data = await response.json();
            this.model = data;
            this.currentImage = data.images?.[0]?.imagem1 || null;
            this.isLoading = false;
            return;
          }
        } catch (error) {
          continue;
        }
      }
      
      this.error = 'Modelo não encontrado';
      this.isLoading = false;
    },
    
    changeImage(imageUrl) {
      this.currentImage = imageUrl;
    },

    getImageUrl(imagePath) {
      if (!imagePath || imagePath.startsWith('http')) return imagePath || '';
      
      const cleanPath = imagePath.replace(/^\.\//, '');
      return cleanPath.startsWith('/') ? cleanPath : '/' + cleanPath;
    },

    getImages() {
      const images = this.model.images?.[0];
      if (!images) return [];
      
      return [images.imagem1, images.imagem2, images.imagem3].filter(Boolean);
    },

    getContacts() {
      const contact = this.model.contact?.[0];
      if (!contact) return {};
      
      return Object.fromEntries(
        Object.entries(contact).filter(([, value]) => value)
      );
    },

    goBack() {
      // Emitir evento para o componente pai voltar para a lista
      this.$emit('back-to-list');
    },

    getAverageRating() {
      if (!this.model.comments || this.model.comments.length === 0) {
        return 0;
      }
      
      const totalStars = this.model.comments.reduce((sum, comment) => sum + comment.stars, 0);
      return totalStars / this.model.comments.length;
    },

    getStarsHtml(rating) {
      let starsHtml = '';
      for (let i = 1; i <= 5; i++) {
        const starClass = i <= rating ? 'star filled' : 'star';
        starsHtml += `<div class="${starClass}">★</div>`;
      }
      return starsHtml;
    }
  }
}
</script>

<style scoped>
.back-button-container {
  margin-bottom: 20px;
  padding: 0 20px;
  position: fixed;
  bottom: 0;
  left: 0;
}

.back-button {
  background: var(--primary);
  color: var(--text-primary);
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.back-button:hover {
  opacity: 0.8;
  transform: translateY(-2px);
}

.back-button:active {
  transform: translateY(0);
}
</style>
>>>>>>> 02e6bf8ab80cdc5de837834f9695e9c3379773c6
