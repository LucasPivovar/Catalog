<template>
  <!-- Google Fonts import -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
  
  <!-- Main container -->
  <div class="main">
    <div class="content">
      <!-- Header section -->
      <h1>Crimson Auth</h1>
      <p v-if="active_tab === 'login'">Entre com sua conta para continuar</p>
      <p v-else>Crie sua conta para começar</p>
      
      <!-- Tab navigation -->
      <div class="buttons">
        <a href="#"
           @click.prevent="activate_button('login')"
           :class="['button', button_activated('login') ? 'button-active' : '']">
          Login
        </a>
        <a href="#"
           @click.prevent="activate_button('register')"
           :class="['button', button_activated('register') ? 'button-active' : '']">
          Cadastro
        </a>
      </div>
      
      <!-- Login Form -->
      <div class="login" v-show="button_activated('login')">
        <!-- Email field -->
        <div class="form-group">
          <label for="email">Email</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="../assets/icons/Mail.svg" alt="Email icon">
            </div>
            <input type="text" placeholder="seu@email.com" id="email" class="input-field">
          </div>
        </div>
        
        <!-- Password field -->
        <div class="form-group">
          <label for="password">Senha</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="../assets/icons/Lock.svg" alt="Lock icon">
            </div>
            <input :type="showPassword ? 'text' : 'password'" placeholder="••••••••" id="password" class="input-field">
            <div class="icon-right" @click="togglePasswordVisibility">
              <!-- Eye icon when password is hidden -->
              <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
              <!-- Eye-off icon when password is visible -->
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
            </div>
          </div>
        </div>
        
        <!-- Remember me and Forgot password row -->
        <div class="remember-forgot">
          <div class="remember-me">
            <input type="checkbox" id="remember-me">
            <label for="remember-me">Lembrar de mim</label>
          </div>
          <a href="#" class="forgot-password">Esqueceu a senha?</a>
        </div>
        
        <!-- Submit button -->
        <button type="submit" class="submit-button">Entrar</button>
      </div>
      
      <!-- Register Form -->
      <div class="register" v-show="button_activated('register')">
        <!-- Full name field -->
        <div class="form-group">
          <label for="fullname">Nome Completo</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="../assets/icons/User.svg" alt="User icon">
            </div>
            <input type="text" placeholder="Seu nome completo" id="fullname" class="input-field">
          </div>
        </div>
        
        <!-- Email field -->
        <div class="form-group">
          <label for="register-email">Email</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="../assets/icons/Mail.svg" alt="Email icon">
            </div>
            <input type="text" placeholder="seu@email.com" id="register-email" class="input-field">
          </div>
        </div>
        
        <!-- CPF field -->
        <div class="form-group">
          <label for="cpf">CPF</label>
          <div class="input-container">
            <div class="icon-left">
              <!-- No icon for CPF -->
            </div>
            <input type="text" placeholder="000.000.000-00" id="cpf" v-model="cpf" @input="formatCPF" class="input-field">
          </div>
        </div>
        
        <!-- Birthdate field -->
        <div class="form-group">
          <label for="birthdate">Data de Nascimento</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="../assets/icons/Calendar.svg" alt="Calendar icon">
            </div>
            <input type="date" placeholder="dd/mm/aaaa" id="birthdate" class="input-field no-calendar-icon">
          </div>
        </div>
        
        <!-- Password field -->
        <div class="form-group">
          <label for="register-password">Senha</label>
          <div class="input-container">
            <div class="icon-left">
              <img src="../assets/icons/Lock.svg" alt="Lock icon">
            </div>
            <input :type="showRegisterPassword ? 'text' : 'password'" placeholder="••••••••" id="register-password" class="input-field">
            <div class="icon-right" @click="toggleRegisterPasswordVisibility">
              <svg v-if="!showRegisterPassword" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
              <!-- Eye-off icon when password is visible -->
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
            </div>
          </div>
        </div>
        
        <!-- Terms and conditions checkbox -->
        <div class="remember-forgot terms-container">
          <div class="accept-terms">
            <input type="checkbox" id="terms">
            <label for="terms">Eu aceito os <a href="#" class="terms-link">Termos de Uso</a> e <a href="#" class="terms-link">Política de Privacidade</a></label>
          </div>
        </div>
      
        <!-- Submit button -->
        <button type="submit" class="submit-button">Criar Conta</button>
      </div>
    </div>
  </div>
</template>

<script>
import '@/assets/css/acess_view.css';
import '@/assets/css/global.css';
export default {
  data() {
    return {
      active_tab: 'login',
      showPassword: false,
      showRegisterPassword: false,
      cpf: ''
    }
  },
  
  methods: {
    activate_button(val) {
      this.active_tab = val;
    },
    
    button_activated(val) {
      return this.active_tab === val;
    },
    
    togglePasswordVisibility() {
      this.showPassword = !this.showPassword;
    },
    
    toggleRegisterPasswordVisibility() {
      this.showRegisterPassword = !this.showRegisterPassword;
    },
    
    formatCPF() {
      let value = this.cpf.replace(/\D/g, '');
      
      value = value.substring(0, 11);
      
      if (value.length > 9) {
        this.cpf = value.replace(/(\d{3})(\d{3})(\d{3})(\d{2})/, '$1.$2.$3-$4');
      } else if (value.length > 6) {
        this.cpf = value.replace(/(\d{3})(\d{3})(\d{1,3})/, '$1.$2.$3');
      } else if (value.length > 3) {
        this.cpf = value.replace(/(\d{3})(\d{1,3})/, '$1.$2');
      } else {
        this.cpf = value;
      }
    }
  },
}
</script>