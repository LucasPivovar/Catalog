<template>
  <div class="main">
    <div class="boost-container">
      <h1 class="section-title">Impulsionamento</h1>
      <div class="boost-content">
        <div class="section-header">
          <h2>Seus Créditos</h2>
          <p class="subtitle">Gerencie seus créditos de impulsionamento</p>
        </div>
        
        <!-- Display de créditos disponíveis -->
        <div class="credits-display">
          <div class="credit-amount">{{ totalCredits }}</div>
          <div class="credit-label">créditos disponíveis</div>
        </div>
      
        <div class="section-header">
          <h2>Comprar Mais Créditos</h2>
        </div>
      
        <!-- Opções de compra de créditos -->
        <div class="credit-options">
          <div 
            v-for="option in creditOptions" 
            :key="option.amount"
            class="credit-option" 
            :class="{ 'selected': selectedOption === option.amount, 'popular': option.popular }"
            @click="selectCreditOption(option.amount, option.price)"
          >
            <div v-if="option.popular" class="popular-tag">Popular</div>
            <div class="option-amount">{{ option.amount }}</div>
            <div class="option-label">créditos</div>
            <div class="option-price">R$ {{ option.price.toFixed(2) }}</div>
          </div>
        </div>
      
        <!-- Botão de compra -->
        <button class="action-button" @click="buyCredits">
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="button-icon">
            <rect x="1" y="4" width="22" height="16" rx="2" ry="2"></rect>
            <line x1="1" y1="10" x2="23" y2="10"></line>
          </svg>
          Comprar Créditos
        </button>
      </div>
    
      <!-- Seção de Impulsionamento -->
      <div class="boost-content">
        <div class="section-header">
          <h2>Impulsionar Perfil</h2>
          <p class="subtitle">Escolha como deseja impulsionar seu perfil</p>
        </div>
        
        <!-- Abas de impulsionamento -->
        <div class="tab-buttons">
          <button 
            v-for="tab in tabs" 
            :key="tab.id"
            class="tab-button"
            :class="{ 'active': activeTab === tab.id }"
            @click="activeTab = tab.id"
          >
            {{ tab.name }}
          </button>
        </div>
        
        <!-- Conteúdo da aba de agendamento -->
        <div v-if="activeTab === 'schedule'" class="tab-content">
          <div class="schedule-fields">
            <!-- Campo de data (clicável) -->
            <div class="field">
              <label>Data de Início</label>
              <div class="date-picker clickable" @click="focusDateInput">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="date-picker-icon">
                  <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
                  <line x1="16" y1="2" x2="16" y2="6"></line>
                  <line x1="8" y1="2" x2="8" y2="6"></line>
                  <line x1="3" y1="10" x2="21" y2="10"></line>
                </svg>
                <input 
                  ref="dateInput"
                  type="date" 
                  v-model="scheduleDate" 
                  class="date-field"
                />
              </div>
            </div>
            
            <!-- Campo de horário (clicável) -->
            <div class="field">
              <label>Horário</label>
              <div class="time-input clickable" @click="focusTimeInput">
                <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="time-icon">
                  <circle cx="12" cy="12" r="10"></circle>
                  <polyline points="12 6 12 12 16 14"></polyline>
                </svg>
                <input 
                  ref="timeInput"
                  type="time" 
                  v-model="scheduleTime"
                  class="time-field"
                />
              </div>
            </div>
          </div>
          
          <!-- Informações de custo -->
          <div class="cost-info">
            <span class="cost-label">Custo:</span>
            <span class="cost-value">{{ boostCost }} créditos</span>
          </div>
          
          <p class="schedule-info">Impulsionamento programado por 24 horas na data selecionada.</p>
          
          <!-- Botão de agendamento -->
          <button class="action-button" @click="scheduleBoost">
            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="button-icon">
              <circle cx="12" cy="12" r="10"></circle>
              <polyline points="12 6 12 12 16 14"></polyline>
            </svg>
            Agendar Impulsão
          </button>
        </div>
        
        <!-- Conteúdo da aba de impulsionamento imediato -->
        <div v-if="activeTab === 'now'" class="tab-content">
          <div class="now-boost-content">
            <p class="boost-description">
              Seu perfil será impulsionado imediatamente por 24 horas após a confirmação.
            </p>
            
            <!-- Informações de custo -->
            <div class="cost-info">
              <span class="cost-label">Custo:</span>
              <span class="cost-value">{{ boostCost }} créditos</span>
            </div>
            
            <!-- Botão de impulsionamento imediato -->
            <button class="action-button" @click="boostNow">
              <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="button-icon">
                <polygon points="5 3 19 12 5 21 5 3"></polygon>
              </svg>
              Impulsionar Agora
            </button>
          </div>
        </div>
        
        <!-- Alerta simplificado sem componente -->
        <div v-if="showAlert" class="alert-container">
          <div class="alert">
            <span class="alert-message">{{ alertMessage }}</span>
            <button class="alert-close" @click="showAlert = false">×</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import '@/assets/css/boost_content.css';
import '@/assets/css/global.css';

export default {
  name: 'BoostContent',
  data() {
    return {
      // Dados do usuário
      totalCredits: 250,
      selectedOption: 1000,
      selectedPrice: 159.90,
      boostCost: 100,
      
      // Estado UI
      activeTab: 'schedule',
      scheduleDate: '',
      scheduleTime: '00:00',
      showAlert: false,
      alertMessage: '',
      
      // Opções pré-definidas
      creditOptions: [
        { amount: 100, price: 19.90, popular: false },
        { amount: 500, price: 89.90, popular: true },
        { amount: 1000, price: 159.90, popular: false }
      ],
      tabs: [
        { id: 'now', name: 'Impulsionar Agora' },
        { id: 'schedule', name: 'Agendar Impulsão' }
      ]
    }
  },
  methods: {
    // Métodos de compra de créditos
    selectCreditOption(amount, price) {
      this.selectedOption = amount;
      this.selectedPrice = price;
    },
    buyCredits() {
      if (!this.selectedOption) {
        this.showAlertMessage('Por favor, selecione uma opção de créditos.');
        return;
      }
      
      // Simula chamada de API
      setTimeout(() => {
        this.totalCredits += this.selectedOption;
        this.showAlertMessage(`${this.selectedOption} créditos adicionados com sucesso!`);
      }, 500);
    },
    
    // Métodos para focar nos inputs de data e hora
    focusDateInput() {
      this.$refs.dateInput.focus();
      this.$refs.dateInput.showPicker && this.$refs.dateInput.showPicker();
    },
    
    focusTimeInput() {
      this.$refs.timeInput.focus();
      this.$refs.timeInput.showPicker && this.$refs.timeInput.showPicker();
    },
    
    // Métodos de agendamento
    scheduleBoost() {
      // Validação
      if (this.totalCredits < this.boostCost) {
        this.showAlertMessage('Créditos insuficientes para agendar impulsionamento.');
        return;
      }
      
      if (!this.scheduleDate) {
        this.showAlertMessage('Por favor, selecione uma data para o agendamento.');
        return;
      }
      
      // Simula chamada de API
      setTimeout(() => {
        this.totalCredits -= this.boostCost;
        this.showAlertMessage(`Impulsionamento agendado para ${this.formatDisplayDate(this.scheduleDate)} às ${this.scheduleTime}!`);
      }, 500);
    },
    
    // Método para impulsionar agora
    boostNow() {
      // Validação
      if (this.totalCredits < this.boostCost) {
        this.showAlertMessage('Créditos insuficientes para impulsionar perfil.');
        return;
      }
      
      // Simula chamada de API
      setTimeout(() => {
        this.totalCredits -= this.boostCost;
        this.showAlertMessage('Perfil está sendo impulsionado por 24 horas!');
      }, 500);
    },
    
    // Método para formatação amigável da data
    formatDisplayDate(dateString) {
      if (!dateString) return '';
      
      const date = new Date(dateString);
      const day = date.getDate().toString().padStart(2, '0');
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      const year = date.getFullYear();
      
      return `${day}/${month}/${year}`;
    },
    
    // Método de alerta simplificado
    showAlertMessage(message) {
      this.alertMessage = message;
      this.showAlert = true;
      
      // Auto-esconde alerta após 5 segundos
      setTimeout(() => {
        this.showAlert = false;
      }, 5000);
    }
  }
}
</script>
