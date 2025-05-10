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
