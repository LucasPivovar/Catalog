<template>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
  
  <div class="main">
    <div class="content">
      <h1>Crimson Auth</h1>
      <p v-if="mostrar === 'login'">Entre com sua conta para continuar</p>
      <p v-else>Crie sua conta para começar</p>
      
      <div class="buttons">
        <a href="#"
           @click.prevent="deixarAtivo('login')"
           :class="['botoes', estaAtivo('login') ? 'botao-ativo' : '']">
          Login
        </a>
        <a href="#"
           @click.prevent="deixarAtivo('register')"
           :class="['botoes', estaAtivo('register') ? 'botao-ativo' : '']">
          Cadastro
        </a>
      </div>
      
      <div class="login" v-show="estaAtivo('login')">
        <div class="form-group">
          <label for="email">Email</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input type="text" placeholder="seu@email.com" id="email">
          </div>
        </div>
        
        <div class="form-group">
          <label for="password">Senha</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input :type="showPassword ? 'text' : 'password'" placeholder="••••••••" id="password">
            <span class="icon-right" @click="togglePasswordVisibility">
              <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
            </span>
          </div>
        </div>
        
        <div class="remember-forgot">
          <div class="remember">
            <input type="checkbox" id="remember">
            <label for="remember">Lembrar de mim</label>
          </div>
          <a href="#" class="forgot">Esqueceu a senha?</a>
        </div>
        
        <button type="submit" class="btn-submit">Entrar</button>
      </div>
      
      <div class="register" v-show="estaAtivo('register')">
        <div class="form-group">
          <label for="fullname">Nome Completo</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input type="text" placeholder="Seu nome completo" id="fullname">
          </div>
        </div>
        
        <div class="form-group">
          <label for="register-email">Email</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input type="text" placeholder="seu@email.com" id="register-email">
          </div>
        </div>
        
        <div class="form-group">
          <label for="cpf">CPF</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input type="text" placeholder="000.000.000-00" id="cpf" v-model="cpf" @input="formatarCPF">
          </div>
        </div>
        
        <div class="form-group">
          <label for="birthdate">Data de Nascimento</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input type="date" placeholder="dd/mm/aaaa" id="birthdate" class="no-calendar-icon">
          </div>
        </div>
        
        <div class="form-group">
          <label for="register-password">Senha</label>
          <div class="input-icon">
            <span class="icon"></span>
            <input :type="showRegisterPassword ? 'text' : 'password'" placeholder="••••••••" id="register-password">
            <span class="icon-right" @click="toggleRegisterPasswordVisibility">
              <svg v-if="!showRegisterPassword" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"></path>
                <circle cx="12" cy="12" r="3"></circle>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="eye-icon">
                <path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19m-6.72-1.07a3 3 0 1 1-4.24-4.24"></path>
                <line x1="1" y1="1" x2="23" y2="23"></line>
              </svg>
            </span>
          </div>
        </div>
        
        <div class="remember-forgot terms">
          <div class="remember">
            <input type="checkbox" id="terms">
            <label for="terms">Eu aceito os <a href="#" class="terms-link">Termos de Uso</a> e <a href="#" class="terms-link">Política de Privacidade</a></label>
          </div>
        </div>
        
        <button type="submit" class="btn-submit">Criar Conta</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AcessView',
  data() {
    return {
      mostrar: 'login',
      showPassword: false,
      showRegisterPassword: false,
      cpf: ''
    }
  },
  
  methods: {
    deixarAtivo(val) {
      this.mostrar = val;
    },
    
    estaAtivo(val) {
      return this.mostrar === val;
    },
    
    togglePasswordVisibility() {
      this.showPassword = !this.showPassword;
    },
    
    toggleRegisterPasswordVisibility() {
      this.showRegisterPassword = !this.showRegisterPassword;
    },
    
    formatarCPF() {
      // Remove todos os caracteres não numéricos
      let valor = this.cpf.replace(/\D/g, '');
      
      // Limita a 11 dígitos
      valor = valor.substring(0, 11);
      
      // Aplica a máscara 000.000.000-00
      if (valor.length > 9) {
        this.cpf = valor.replace(/(\d{3})(\d{3})(\d{3})(\d{2})/, '$1.$2.$3-$4');
      } else if (valor.length > 6) {
        this.cpf = valor.replace(/(\d{3})(\d{3})(\d{1,3})/, '$1.$2.$3');
      } else if (valor.length > 3) {
        this.cpf = valor.replace(/(\d{3})(\d{1,3})/, '$1.$2');
      } else {
        this.cpf = valor;
      }
    }
  },
}
</script>

<style scoped>
.main {
  --background-primary: rgb(5, 5, 5);
  --background-content: #151515;
  --content-border: #333333;
  --button-active: #d32f4b;
  --button-inactive: #333333;
  --title-content: #eb405d;
  --subtitle-content: #a6a6a6;
  --text-color: white;
  --text-inactive: #a6a69b;
  --forgot-color: #d32f4b;
  --input-bg: #1c1c1c;
  --input-border: #333333;
  
  font-family: "Inter", sans-serif;
}

.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100vh;
  background: var(--background-primary);
}

.content {
  width: 100%;
  max-width: 400px;
  padding: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--background-content);
  border: 1px solid var(--content-border);
  border-radius: 10px;
}

.content h1 {
  font-size: 2rem;
  color: var(--title-content);
  font-weight: 600;

}

.login, .register {
  width: 100%;
}

.form-group {
  margin-bottom: 15px;
  width: 100%;
}

label {
  font-size: 0.95rem;
  color: var(--text-color);
  margin-bottom: 8px;
  display: block;
  font-weight: 500;
}

.input-icon {
  position: relative;
  width: 100%;
}

.icon {
  position: absolute;
  left: 10px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--text-inactive);
  font-size: 0.9rem;
}

.icon-right {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--text-inactive);
  font-size: 0.9rem;
  cursor: pointer;
}

.eye-icon {
  color: var(--text-inactive);
  transition: color 0.2s;
}

.eye-icon:hover {
  color: var(--text-color);
}

input {
  width: 100%;
  padding: 12px 35px;
  background: var(--input-bg);
  border: 1px solid var(--input-border);
  border-radius: 5px;
  font-size: 0.95rem;
  color: var(--text-color);
  box-sizing: border-box;
}

input:focus {
  outline: none;
  border: 1px solid var(--button-active);
}


input.no-calendar-icon {
  padding-left: 12px; 
}

input[type="date"]::-webkit-calendar-picker-indicator {
  display: none;
}

/* Ajuste para firefox */
input[type="date"] {
  appearance: textfield;
  -moz-appearance: textfield;
}

input::placeholder {
  color: var(--text-inactive);
}

.remember-forgot {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  width: 100%;
}

.remember {
  display: flex;
  align-items: center;
}

.remember input[type="checkbox"] {
  width: 16px;
  height: 16px;
  margin-right: 8px;
  accent-color: var(--button-active);
}

.remember label {
  margin-bottom: 0;
  font-size: 0.85rem;
  color: var(--text-inactive);
  font-weight: normal;
}

.forgot {
  font-size: 0.85rem;
  color: var(--forgot-color);
  text-decoration: none;
}

.btn-submit {
  background: var(--button-active);
  color: var(--text-color);
  border: none;
  padding: 12px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 500;
  width: 100%;
  font-size: 1rem;
}

p {
  font-size: 1rem;
  color: var(--subtitle-content);
  margin-bottom: 20px;
}

.buttons {
  display: flex;
  width: 100%;
  margin-bottom: 20px;
  border-radius: 5px;
  overflow: hidden;
  padding: 10px;
  gap: 10px;
  background-color: var(--button-inactive);
}

.botoes {
  color: var(--text-inactive);
  padding: 5px;
  text-align: center;
  text-decoration: none;
  width: 50%;
  transition: all 0.3s ease;
  font-weight: 500;
  border-radius: 5px;
}

.botao-ativo {
  background-color: var(--button-active);
  color: var(--text-color);
}

.terms {
  justify-content: flex-start;
}

.terms-link {
  color: var(--forgot-color);
  text-decoration: none;
}
</style>