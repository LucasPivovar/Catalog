<template>
    <main>
        <div class="content">
            <div class="dashboard-header">
                <h1><strong>Aura</strong>Pulse</h1>
                <div class="buttons">
                    <a href="#"
                    @click.prevent="activate_button('login')"
                    :class="['button', button_activated('login') ? 'button-active' : '']">
                    Login
                    </a>
                </div>
            </div>
        </div>
    </main>
</template>
<script>
import '@/assets/css/dashboard_view.css';
import '@/assets/css/global.css';
export default {
    name: 'DashboardView',
    data() {
        return {
            active_tab: '',
        }
    },

    methods: {

    }
}

</script>