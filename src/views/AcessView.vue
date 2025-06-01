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
              Eu aceito os <a href="#" @click.prevent="openModal('terms')" class="terms-link">Termos de Uso</a> e 
              <a href="#" @click.prevent="openModal('privacy')" class="terms-link">Política de Privacidade</a>
            </label>
          </div>
        </div>
        
        <!-- Submit Button -->
        <button class="submit-button" @click="register">Criar Conta</button>
      </div>
    </div>
    
    <!-- Modal -->
    <div v-if="showModal" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <div class="modal-header">
          <h2>{{ modalTitle }}</h2>
          <button class="modal-close" @click="closeModal">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <line x1="18" y1="6" x2="6" y2="18"></line>
              <line x1="6" y1="6" x2="18" y2="18"></line>
            </svg>
          </button>
        </div>
        <div class="modal-body">
          <div v-html="modalContent"></div>
        </div>
        <div class="modal-footer">
          <button class="modal-button" @click="closeModal">Fechar</button>
        </div>
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
      showConfirmPassword: false,
      showModal: false,
      modalType: '',
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
    },
    modalTitle() {
      return this.modalType === 'terms' ? 'Termos de Uso' : 'Política de Privacidade';
    },
    modalContent() {
      if (this.modalType === 'terms') {
        return `
          <h3>1. Aceitação dos Termos</h3>
          <p>Ao utilizar nossos serviços, você concorda com estes termos de uso.</p>
          
          <h3>2. Uso do Serviço</h3>
          <p>Você concorda em usar nosso serviço apenas para fins legais e de acordo com estes termos.</p>
          
          <h3>3. Conta do Usuário</h3>
          <p>Você é responsável por manter a confidencialidade de sua conta e senha.</p>
          
          <h3>4. Privacidade</h3>
          <p>Respeitamos sua privacidade conforme descrito em nossa Política de Privacidade.</p>
          
          <h3>5. Limitação de Responsabilidade</h3>
          <p>Nosso serviço é fornecido "como está" sem garantias de qualquer tipo.</p>
          
          <h3>6. Modificações</h3>
          <p>Podemos modificar estes termos a qualquer momento. As alterações entrarão em vigor imediatamente.</p>
          
          <h3>7. Contato</h3>
          <p>Para dúvidas sobre estes termos, entre em contato conosco através do e-mail: contato@crimsonauth.com</p>
        `;
      } else {
        return `
          <h3>1. Coleta de Informações</h3>
          <p>Coletamos informações que você nos fornece diretamente, como nome, e-mail e CPF.</p>
          
          <h3>2. Uso das Informações</h3>
          <p>Utilizamos suas informações para fornecer e melhorar nossos serviços.</p>
          
          <h3>3. Compartilhamento de Dados</h3>
          <p>Não vendemos, comercializamos ou transferimos suas informações pessoais para terceiros.</p>
          
          <h3>4. Segurança</h3>
          <p>Implementamos medidas de segurança para proteger suas informações pessoais.</p>
          
          <h3>5. Cookies</h3>
          <p>Utilizamos cookies para melhorar sua experiência em nosso site.</p>
          
          <h3>6. Seus Direitos</h3>
          <p>Você tem o direito de acessar, corrigir ou excluir suas informações pessoais.</p>
          
          <h3>7. Contato</h3>
          <p>Para questões sobre privacidade, entre em contato: privacidade@crimsonauth.com</p>
        `;
      }
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
      if (this.$refs.birthdateInput) {
        this.$refs.birthdateInput.click();
      }
    },
    formatDate() {
      if (this.registerForm.birthdate) {
        const [year, month, day] = this.registerForm.birthdate.split('-');
        this.displayBirthdate = `${day}/${month}/${year}`;
      }
    },
    // Métodos do Modal
    openModal(type) {
      this.modalType = type;
      this.showModal = true;
      document.body.style.overflow = 'hidden';
    },
    closeModal() {
      this.showModal = false;
      document.body.style.overflow = 'auto';
    }
  },
  mounted() {
    if (this.$refs.birthdateInput) {
      this.$refs.birthdateInput.max = new Date().toISOString().split('T')[0];
    }
  }
}
</script>