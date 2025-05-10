<template>
  <!-- Google Fonts import -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&family=Libre+Baskerville:wght@400;700&display=swap" rel="stylesheet">
  <div class="main">
    <div class="auth-container">
      <!-- Auth Header -->
      <h1>Crimson Auth</h1>
      <p>{{ activeTab === 'login' ? 'Entre com sua conta para continuar' : 'Crie sua conta para começar' }}</p>
      
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
            <div class="icon-right password-toggle" @click="togglePassword">
              <svg v-if="showPassword" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
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
        
        <!-- Data de Nascimento Field (MELHORADO) -->
        <div class="form-group">
          <label for="birthdate">Data de Nascimento</label>
          <div class="input-container date-container" @click="openDatePicker">
            <div class="icon-left">
              <img src="@/assets/icons/Calendar.svg" alt="Calendar">
            </div>
            <input 
              type="text" 
              class="input-field date-display" 
              placeholder="DD/MM/AAAA" 
              :value="displayBirthdate"
              readonly>
            <div class="icon-right">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="calendar-icon">
                <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
                <line x1="16" y1="2" x2="16" y2="6"></line>
                <line x1="8" y1="2" x2="8" y2="6"></line>
                <line x1="3" y1="10" x2="21" y2="10"></line>
              </svg>
            </div>
            <input 
              type="date" 
              ref="birthdateInput"
              v-model="registerForm.birthdate"
              class="hidden-date-input"
              @change="formatDate">
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
            <div class="icon-right password-toggle" @click="togglePassword">
              <svg v-if="showPassword" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
            </div>
          </div>
        </div>
        
        <!-- Confirm Password Field -->
        <div class="form-group">
          <label for="confirmPassword">Confirmar Senha</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="@/assets/icons/Lock.svg" alt="Confirm Password">
            </div>
            <input 
              :type="showConfirmPassword ? 'text' : 'password'" 
              id="confirmPassword" 
              class="input-field" 
              placeholder="••••••••" 
              v-model="registerForm.confirmPassword">
            <div class="icon-right password-toggle" @click="toggleConfirmPassword">
              <svg v-if="showConfirmPassword" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
            </div>
          </div>
          <p v-if="passwordMismatch" class="error-message">As senhas não coincidem</p>
        </div>
        
        <!-- Terms and Conditions -->
        <div class="remember-forgot">
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
export default {
  name: 'AuthComponent',
  data() {
    return {
      activeTab: 'login',
      showPassword: false,
      showConfirmPassword: false,
      loginForm: {
        email: '',
        password: '',
        remember: false
      },
      registerForm: {
        fullName: '',
        email: '',
        cpf: '',
        birthdate: '',
        password: '',
        confirmPassword: '',
        acceptTerms: false
      },
      formattedCPF: '',
      displayBirthdate: ''
    }
  },
  computed: {
    passwordMismatch() {
      if (!this.registerForm.confirmPassword) return false;
      return this.registerForm.password !== this.registerForm.confirmPassword;
    }
  },
  methods: {
    togglePassword() {
      this.showPassword = !this.showPassword;
    },
    toggleConfirmPassword() {
      this.showConfirmPassword = !this.showConfirmPassword;
    },
    login() {
      console.log('Login attempt', this.loginForm);
    },
    register() {
      console.log('Register attempt', this.registerForm);
    },
    handleCPFInput(event) {
      const rawValue = event.target.value.replace(/\D/g, '');
      this.registerForm.cpf = rawValue.substring(0, 11);
      
      if (rawValue.length <= 3) {
        this.formattedCPF = rawValue;
      } else if (rawValue.length <= 6) {
        this.formattedCPF = rawValue.substring(0, 3) + '.' + rawValue.substring(3);
      } else if (rawValue.length <= 9) {
        this.formattedCPF = rawValue.substring(0, 3) + '.' + rawValue.substring(3, 6) + '.' + rawValue.substring(6);
      } else {
        this.formattedCPF = rawValue.substring(0, 3) + '.' + rawValue.substring(3, 6) + '.' + 
                           rawValue.substring(6, 9) + '-' + rawValue.substring(9, 11);
      }
    },
    openDatePicker() {
      // Abre o seletor de data nativo
      if (this.$refs.birthdateInput) {
        this.$refs.birthdateInput.click();
      }
    },
    formatDate() {
      if (this.registerForm.birthdate) {
        const [year, month, day] = this.registerForm.birthdate.split('-');
        this.displayBirthdate = `${day}/${month}/${year}`;
      }
    }
  },
  mounted() {
    // Define a data máxima como hoje (para impedir datas futuras)
    if (this.$refs.birthdateInput) {
      this.$refs.birthdateInput.max = new Date().toISOString().split('T')[0];
    }
  }
}
</script>

<style>
/* ========== MAIN CONTAINER ========== */
.main {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: var(--bg-main);
    padding: 40px 0;
    /* Removed min-height and justify-content to prevent stretching */
}
  
/* ========== CONTENT CONTAINER ========== */
.auth-container {
    width: 100%;
    max-width: 400px;
    padding: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
    background: #151515; /* Matching the exact color in your DevTools screenshot */
    border: 1px solid var(--border-main);
    border-radius: 10px;
    box-sizing: border-box; /* Ensure padding doesn't add to the width */
}
  
/* ========== TYPOGRAPHY ========== */
.auth-container h1 {
    font-size: 2rem;
    color: #E64C6F; /* Crimson color from your screenshot */
    font-weight: 600;
    margin-bottom: 10px;
    text-align: center;
}
  
p {
    font-size: 1rem;
    color: var(--text-secondary);
    margin-bottom: 20px;
    text-align: center;
}
  
/* ========== FORM CONTAINERS ========== */
.login, .register {
    width: 100%;
}
  
/* ========== TAB BUTTONS ========== */
.buttons {
    display: flex;
    width: 100%;
    margin-bottom: 20px;
    border-radius: 5px;
    overflow: hidden;
    padding: 10px;
    gap: 10px;
    background-color: var(--btn-secondary);
}
  
.button {
    color: var(--text-secondary);
    padding: 8px 0; /* Slightly increased padding for better height */
    text-align: center;
    text-decoration: none;
    width: 50%;
    transition: all 0.3s ease;
    font-weight: 500;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
}
  
.button-active {
    background-color: #E64C6F; /* Matching the active tab color in screenshot */
    color: var(--text-primary);
}
  
/* ========== FORM GROUPS ========== */
.form-group {
    margin-bottom: 15px;
    width: 100%;
}
  
label {
    font-size: 0.95rem;
    color: var(--text-primary);
    display: block;
    font-weight: 500;
}
  
/* ========== INPUT CONTAINERS ========== */
.input-container {
    width: 100%;
    display: flex;
    align-items: center;
    background: var(--bg-element);
    border: 1px solid var(--border-main);
    border-radius: 5px;
    transition: border 0.2s;
    position: relative; /* Posicionamento para o datepicker */
}
  
.input-container:focus-within {
    border: 1px solid var(--primary);
}

/* Icon containers */
.icon-left, .icon-right {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 10px;
}

.icon-left img{
    width: 20px;
}
  
.icon-right {
    cursor: pointer;
}

/* ========== ICON STYLES ========== */
.eye-icon {
    color: var(--text-secondary);
    transition: color 0.2s;
}
  
.eye-icon:hover {
    color: var(--text-primary);
}

/* Estilo para o ícone de calendário */
.calendar-icon {
    color: var(--text-secondary);
    transition: color 0.2s;
}

.calendar-icon:hover {
    color: var(--text-primary);
}
  
/* ========== INPUTS ========== */
.input-field {
    flex: 1;
    padding: 12px 0;
    background: transparent;
    border: none;
    font-size: 0.95rem;
    color: var(--text-primary);
}
  
.input-field:focus {
    outline: none;
}
  
.input-field::placeholder {
    color: var(--text-secondary);
}

/* ========== DATA PICKER OCULTO E CONTAINER ========== */

/* Estilos específicos para o contêiner de data */
.date-container {
    cursor: pointer;
    position: relative;
    transition: background-color 0.2s;
}

.date-container:hover {
    background-color: var(--bg-hover, rgba(255, 255, 255, 0.05));
}

.date-container:active {
    background-color: var(--bg-active, rgba(255, 255, 255, 0.1));
}

/* Estilos para o campo de exibição da data */
.date-display {
    cursor: pointer;
}

/* Garante que o input de data fique realmente escondido em todos os navegadores */
.hidden-date-input {
    position: absolute;
    opacity: 0;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    cursor: pointer;
    z-index: 1; /* Garante que esteja acima para capturar cliques */
}

/* Permite que os eventos de clique ainda sejam tratados pelo script */
.hidden-date-input::-webkit-calendar-picker-indicator {
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 0;
    cursor: pointer;
}

/* Feedback visual ao passar o mouse */
.date-container::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: transparent;
    pointer-events: none;
    transition: background-color 0.2s;
}

.date-container:hover::after {
    background-color: rgba(230, 76, 111, 0.05); /* Efeito sutil na cor principal */
}

/* ========== CHECKBOX & LINKS ROW ========== */
.remember-forgot {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    width: 100%;
}
  
.remember-me {
    display: flex;
    align-items: center;
}
  
.remember-me input[type="checkbox"] {
    width: 16px;
    height: 16px;
    margin-right: 8px;
    accent-color: var(--btn-primary);
}
  
.remember-me label {
    margin-bottom: 0;
    font-size: 0.85rem;
    color: var(--text-secondary);
    font-weight: normal;
}
  
.forgot-password {
    font-size: 0.85rem;
    color: #E64C6F; /* Matching the link color in screenshot */
    text-decoration: none;
}
  
/* Terms section */
.terms-container {
    justify-content: flex-start;
}

.accept-terms {
    display: flex;
    align-items: center;
}

.accept-terms input[type="checkbox"] {
    width: 16px;
    height: 16px;
    margin-right: 8px;
    accent-color: var(--btn-primary);
}

.accept-terms label {
    margin-bottom: 0;
    font-size: 0.85rem;
    color: var(--text-secondary);
    font-weight: normal;
}
  
.terms-link {
    color: #E64C6F; /* Matching the link color in screenshot */
    text-decoration: none;
}
  
/* ========== BUTTONS ========== */
.submit-button {
    background: #E64C6F; /* Matching the button color in screenshot */
    color: var(--text-primary);
    border: none;
    padding: 12px;
    border-radius: 5px;
    cursor: pointer;
    font-weight: 500;
    width: 100%;
    font-size: 1rem;
}

/* Mensagens de erro */
.error-message {
    color: #FF6B6B;
    font-size: 0.8rem;
    margin-top: 5px;
    text-align: left;
}

@media screen and (max-width: 768px) {
    .auth-container {
        width: 90%;
        max-width: 400px;
    }
}
</style>