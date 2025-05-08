<template>
  <!-- Google Fonts import -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
  <div class="main">
    <div class="auth-container">
      <!-- Auth Header -->
      <h1>Crimson Auth</h1>
      <p v-if="activeTab === 'login'">Entre com sua conta para continuar</p>
      <p v-else>Crie sua conta para começar</p>
      
      <!-- Tab Buttons -->
      <div class="buttons">
        <a href="#" 
          @click.prevent="activeTab = 'login'" 
          :class="['button', activeTab === 'login' ? 'button-active' : '']">
          Login
        </a>
        <a href="#" 
          @click.prevent="activeTab = 'register'" 
          :class="['button', activeTab === 'register' ? 'button-active' : '']">
          Cadastro
        </a>
      </div>
      
      <!-- Login Form -->
      <div v-if="activeTab === 'login'" class="login">
        <!-- Email Field -->
        <div class="form-group">
          <label for="email">Email</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/Mail.svg" alt="Email">
            </div>
            <input 
              type="email" 
              id="email" 
              class="input-field" 
              placeholder="seu@email.com" 
              v-model="loginForm.email">
          </div>
        </div>
        
        <!-- Password Field -->
        <div class="form-group">
          <label for="password">Senha</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/Lock.svg" alt="Password">
            </div>
            <input 
              :type="showPassword ? 'text' : 'password'" 
              id="password" 
              class="input-field" 
              placeholder="••••••••" 
              v-model="loginForm.password">
            <div class="icon-right" @click="togglePassword">
              <img :src="showPassword ? '@/assets/icons/EyeOff.svg' : '@/assets/icons/Eye.svg'" alt="Toggle Password">
            </div>
          </div>
        </div>
        
        <!-- Remember Me & Forgot Password -->
        <div class="remember-forgot">
          <div class="remember-me">
            <input type="checkbox" id="remember" v-model="loginForm.remember">
            <label for="remember">Lembrar de mim</label>
          </div>
          <a href="#" class="forgot-password">Esqueceu a senha?</a>
        </div>
        
        <!-- Submit Button -->
        <button class="submit-button" @click="login">Entrar</button>
      </div>
      
      <!-- Register Form -->
      <div v-else class="register">
        <!-- Nome Completo Field -->
        <div class="form-group">
          <label for="fullName">Nome Completo</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/User.svg" alt="User">
            </div>
            <input 
              type="text" 
              id="fullName" 
              class="input-field" 
              placeholder="Seu nome completo" 
              v-model="registerForm.fullName">
          </div>
        </div>
        
        <!-- Email Field -->
        <div class="form-group">
          <label for="regEmail">Email</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/Mail.svg" alt="Email">
            </div>
            <input 
              type="email" 
              id="regEmail" 
              class="input-field" 
              placeholder="seu@email.com" 
              v-model="registerForm.email">
          </div>
        </div>
        
        <!-- CPF Field -->
        <div class="form-group">
          <label for="cpf">CPF</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/Document.svg" alt="Document">
            </div>
            <input 
              type="text" 
              inputmode="numeric"
              id="cpf" 
              class="input-field" 
              placeholder="000.000.000-00" 
              v-model="formattedCPF"
              @input="handleCPFInput">
          </div>
        </div>
        
        <!-- Data de Nascimento Field - Versão melhorada -->
        <div class="form-group">
          <label for="birthdate">Data de Nascimento</label>
          <div 
            class="input-container custom-datepicker" 
            @click="openDatePicker">
            <div class="icon-left">
              <img src="@/assets/icons/Calendar.svg" alt="Calendar">
            </div>
            <input 
              type="text" 
              id="birthdate-display" 
              class="input-field" 
              placeholder="DD/MM/AAAA" 
              :value="displayBirthdate"
              readonly>
            <!-- Date input real oculto -->
            <input 
              type="date" 
              id="birthdate-real" 
              ref="birthdateInput"
              v-model="registerForm.birthdate"
              class="hidden-date-input"
              @change="handleDateChange">
          </div>
        </div>
        
        <!-- Password Field -->
        <div class="form-group">
          <label for="regPassword">Senha</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/Lock.svg" alt="Password">
            </div>
            <input 
              :type="showPassword ? 'text' : 'password'" 
              id="regPassword" 
              class="input-field" 
              placeholder="••••••••" 
              v-model="registerForm.password">
            <div class="icon-right" @click="togglePassword">
              <img :src="showPassword ? '@/assets/icons/EyeOff.svg' : '@/assets/icons/Eye.svg'" alt="Toggle Password">
            </div>
          </div>
        </div>
        
        <!-- Terms and Conditions -->
        <div class="remember-forgot terms-container">
          <div class="accept-terms">
            <input type="checkbox" id="terms" v-model="registerForm.acceptTerms">
            <label for="terms">
              Eu aceito os <a href="#" class="terms-link">Termos de Uso</a> e 
              <a href="#" class="terms-link">Política de Privacidade</a>
            </label>
          </div>
        </div>
        
        <!-- Submit Button -->
        <button class="submit-button" @click="register">Criar Conta</button>
      </div>
    </div>
  </div>
</template>

<script>
import '@/assets/css/acess_view.css';
export default {
  name: 'AuthComponent',
  data() {
    return {
      activeTab: 'login',
      showPassword: false,
      loginForm: {
        email: '',
        password: '',
        remember: false
      },
      registerForm: {
        fullName: '',
        email: '',
        cpf: '', // Valor bruto, apenas números
        birthdate: '', // Formato YYYY-MM-DD para o input nativo
        password: '',
        acceptTerms: false
      },
      formattedCPF: '', // Valor formatado do CPF para exibição
      displayBirthdate: '' // Data formatada para exibição no campo visual
    }
  },
  methods: {
    togglePassword() {
      this.showPassword = !this.showPassword;
    },
    login() {
      // Login logic here
      console.log('Login attempt', this.loginForm);
    },
    register() {
      // Register logic here
      console.log('Register attempt', this.registerForm);
    },
    
    // CPF handling methods
    handleCPFInput(event) {
      // Remove todos os caracteres não numéricos
      const rawValue = event.target.value.replace(/\D/g, '');
      
      // Armazena o CPF bruto (apenas números)
      this.registerForm.cpf = rawValue.substring(0, 11);
      
      // Formata o CPF para exibição
      this.formatCPF();
    },
    
    formatCPF() {
      let cpf = this.registerForm.cpf;
      
      // Aplica a formatação de acordo com o comprimento
      if (cpf.length <= 3) {
        this.formattedCPF = cpf;
      } else if (cpf.length <= 6) {
        this.formattedCPF = cpf.substring(0, 3) + '.' + cpf.substring(3);
      } else if (cpf.length <= 9) {
        this.formattedCPF = cpf.substring(0, 3) + '.' + cpf.substring(3, 6) + '.' + cpf.substring(6);
      } else {
        this.formattedCPF = cpf.substring(0, 3) + '.' + cpf.substring(3, 6) + '.' + 
                           cpf.substring(6, 9) + '-' + cpf.substring(9, 11);
      }
    },
    
    // Date picker methods
    openDatePicker() {
      // Simula o clique no input nativo de data
      this.$refs.birthdateInput.click();
    },
    
    handleDateChange() {
      // Converte a data no formato YYYY-MM-DD para DD/MM/YYYY para exibição
      if (this.registerForm.birthdate) {
        const [year, month, day] = this.registerForm.birthdate.split('-');
        this.displayBirthdate = `${day}/${month}/${year}`;
      } else {
        this.displayBirthdate = '';
      }
    },
    
    // Método para limitar a data máxima selecionável (exemplo: não permitir datas futuras)
    getMaxDate() {
      const today = new Date();
      return today.toISOString().split('T')[0]; // Retorna YYYY-MM-DD
    }
  },
  mounted() {
    // Definir uma data máxima para o datepicker (opcional)
    if (this.$refs.birthdateInput) {
      this.$refs.birthdateInput.max = this.getMaxDate();
    }
  }
}
</script>


<style>
/* Estilos para o date picker personalizado */
.custom-datepicker {
  cursor: pointer;
}

/* Esconde o input nativo de data */
.hidden-date-input {
  position: absolute;
  opacity: 0;
  width: 1px;
  height: 1px;
  overflow: hidden;
  pointer-events: none;
  z-index: -1;
}

/* Remove o indicador de calendário nos navegadores */
input[type="date"]::-webkit-calendar-picker-indicator {
  display: none !important;
}

input[type="date"]::-webkit-inner-spin-button { 
  display: none !important;
}

input[type="date"]::-webkit-clear-button {
  display: none !important;
}

/* Estilos para o campo de CPF */
input[inputmode="numeric"]::-webkit-outer-spin-button,
input[inputmode="numeric"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Garante que o campo de data tenha a aparência consistente em todos navegadores */
.input-field[readonly] {
  cursor: pointer;
  color: var(--text-primary) !important;
}

/* Estilos para as setas do password toggle */
.icon-right img {
  width: 20px;
  opacity: 0.7;
  transition: opacity 0.2s;
}

.icon-right:hover img {
  opacity: 1;
}
</style>