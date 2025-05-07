<template>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Roboto:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
  <div class="main">
    <div class="dashboard-header">
      <h1><strong>Aura</strong>Pulse</h1>
      <div class="buttons">
        <a href="#"
            @click.prevent="activate_button('profile')"
            :class="['button', button_activated('profile') ? 'button-active' : '']">
          <p><img src="../assets/icons/white_people.svg" alt=""> Perfil</p>
        </a>

        <a href="#"
            @click.prevent="activate_button('dashboard')"
            :class="['button', button_activated('dashboard') ? 'button-active' : '']">
          dashboard
        </a>
      </div>
    </div>
    <div class="content">

      
      <!-- Componente dinâmico baseado na aba ativa -->
      <ProfileView v-if="active_tab === 'profile'" />
      <DashboardContent v-else />
    </div>
  </div>
</template>

<script>
// Importe o CSS global no arquivo main.js em vez de aqui
// import '@/assets/css/global.css';

import ProfileView from '@/components/ProfileContent.vue';
import DashboardContent from '@/components/DashboardContent.vue';

export default {
  name: 'DashboardView',
  components: {
    ProfileView,
    DashboardContent
  },
  data() {
    return {
      active_tab: 'profile',
    }
  },
  methods: {
    activate_button(val) {
      this.active_tab = val;
    },
    button_activated(val) {
      return this.active_tab === val;
    }
  }
}
</script>

<style>
  .main{
    display: flex;
    justify-content: center;
    background: var(--bg-main);
    width: 100%;
    min-height: 100vh;
  }

  .content{
    width: 100%;
    min-height: 100vh;
    max-width: 1400px;
    background: var(--bg-main);
  }

  .dashboard-header{
    display: flex;
    color: var(--text-primary);
    align-items: center;
    justify-content: space-between;
    margin-top: 1rem;
    padding: 0 35px;
    width: 100%;
  }

  .dashboard-header h1{
    font-weight: 400;
    font-size: 1.6rem;
  }

  .dashboard-header strong{
    font-weight: 400;
    color: var(--primary);
  }

  .buttons {
    display: flex;
    margin-bottom: 20px;
    border-radius: 5px;
    overflow: hidden;
    padding: 8px;
    gap: 10px;
    background-color: var(--btn-secondary);
    max-width: 300px;
  }

  .button {
    color: var(--text-secondary);
    height: 40px;
    text-align: center;
    text-decoration: none;
    transition: all 0.3s ease;
    font-weight: 500;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
  }

  .button-active {
    background-color: var(--btn-primary);
    color: var(--text-primary);
  }

  .button img {
    width: 10px;
  }
</style>