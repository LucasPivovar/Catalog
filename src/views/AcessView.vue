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
            <div class="icon-right" @click="togglePassword">
              <img :src="showPassword ? '@/assets/icons/EyeOff.svg' : '@/assets/icons/Eye.svg'" alt="Toggle">
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
        
        <!-- Data de Nascimento Field -->
        <div class="form-group">
          <label for="birthdate">Data de Nascimento</label>
          <div class="input-container" @click="$refs.birthdateInput.click()">
            <div class="icon-left">
              <img src="@/assets/icons/Calendar.svg" alt="Calendar">
            </div>
            <input 
              type="text" 
              class="input-field" 
              placeholder="DD/MM/AAAA" 
              :value="displayBirthdate"
              readonly>
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
            <div class="icon-right" @click="togglePassword">
              <img :src="showPassword ? '@/assets/icons/hide.png' : '@/assets/icons/Eye.svg'" alt="Toggle">
            </div>
          </div>
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
        cpf: '',
        birthdate: '',
        password: '',
        acceptTerms: false
      },
      formattedCPF: '',
      displayBirthdate: ''
    }
  },
  methods: {
    togglePassword() {
      this.showPassword = !this.showPassword;
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
    formatDate() {
      if (this.registerForm.birthdate) {
        const [year, month, day] = this.registerForm.birthdate.split('-');
        this.displayBirthdate = `${day}/${month}/${year}`;
      }
    }
  },
  mounted() {
    if (this.$refs.birthdateInput) {
      this.$refs.birthdateInput.max = new Date().toISOString().split('T')[0];
    }
  }
}
</script>