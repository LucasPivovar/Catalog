<template>
    <div class="main">
        <h1 class="section-title">Impulsionamento</h1>
        
        <div class="boost-container">
            <!-- Credits Section -->
            <div class="section-header">
                <h2>Seus Créditos</h2>
                <p class="subtitle">Gerencie seus créditos de impulsionamento</p>
            </div>
            
            <div class="credits-display">
                <div class="credit-amount">{{ totalCredits }}</div>
                <div class="credit-label">créditos disponíveis</div>
            </div>
            
            <div class="section-header">
                <h2>Comprar Mais Créditos</h2>
            </div>
            
            <div class="credit-options">
                <div 
                    class="credit-option" 
                    :class="{ 'selected': selectedOption === 100 }"
                    @click="selectCreditOption(100, 19.90)"
                >
                    <div class="option-amount">100</div>
                    <div class="option-label">créditos</div>
                    <div class="option-price">R$ 19.90</div>
                </div>
                
                <div 
                    class="credit-option" 
                    :class="{ 'selected': selectedOption === 500 }"
                    @click="selectCreditOption(500, 89.90)"
                >
                    <div class="popular-tag">Popular</div>
                    <div class="option-amount">500</div>
                    <div class="option-label">créditos</div>
                    <div class="option-price">R$ 89.90</div>
                </div>
                
                <div 
                    class="credit-option" 
                    :class="{ 'selected': selectedOption === 1000 }"
                    @click="selectCreditOption(1000, 159.90)"
                >
                    <div class="option-amount">1000</div>
                    <div class="option-label">créditos</div>
                    <div class="option-price">R$ 159.90</div>
                </div>
            </div>
            
            <button class="action-button" @click="buyCredits">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="button-icon">
                    <rect x="1" y="4" width="22" height="16" rx="2" ry="2"></rect>
                    <line x1="1" y1="10" x2="23" y2="10"></line>
                </svg>
                Comprar Créditos
            </button>
        </div>
        
        <!-- Boost Profile Section -->
        <div class="boost-container">
            <div class="section-header">
                <h2>Impulsionar Perfil</h2>
                <p class="subtitle">Escolha como deseja impulsionar seu perfil</p>
            </div>
            
            <div class="tab-buttons">
                <button 
                    class="tab-button"
                    :class="{ 'active': activeTab === 'now' }"
                    @click="activeTab = 'now'"
                >
                    Impulsionar Agora
                </button>
                <button 
                    class="tab-button"
                    :class="{ 'active': activeTab === 'schedule' }"
                    @click="activeTab = 'schedule'"
                >
                    Agendar Impulsão
                </button>
            </div>
            
            <!-- Schedule Boost Content -->
            <div v-if="activeTab === 'schedule'" class="tab-content">
                <div class="schedule-fields">
                    <div class="field">
                        <label>Data de Início</label>
                        <div class="date-picker">
                            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="date-picker-icon">
                                <rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect>
                                <line x1="16" y1="2" x2="16" y2="6"></line>
                                <line x1="8" y1="2" x2="8" y2="6"></line>
                                <line x1="3" y1="10" x2="21" y2="10"></line>
                            </svg>
                            <input 
                                type="text" 
                                placeholder="Selecione uma data" 
                                v-model="scheduleDate" 
                                readonly 
                                @click="toggleDatePicker"
                            />
                        </div>
                        <!-- Date Picker Calendar -->
                        <div v-if="showDatePicker" class="calendar-container">
                            <div class="calendar-header">
                                <button class="calendar-nav" @click="previousMonth">&lt;</button>
                                <div class="calendar-title">{{ currentMonthName }} {{ currentYear }}</div>
                                <button class="calendar-nav" @click="nextMonth">&gt;</button>
                            </div>
                            <div class="calendar-weekdays">
                                <div class="weekday" v-for="day in weekdays" :key="day">{{ day }}</div>
                            </div>
                            <div class="calendar-days">
                                <div 
                                    v-for="day in calendarDays" 
                                    :key="day.date" 
                                    class="calendar-day" 
                                    :class="{
                                        'other-month': !day.currentMonth,
                                        'selected': isSelectedDate(day.date),
                                        'today': isToday(day.date)
                                    }"
                                    @click="selectDate(day.date)"
                                >
                                    {{ day.dayNumber }}
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="field">
                        <label>Horário</label>
                        <div class="time-select">
                            <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="date-picker-icon">
                                <circle cx="12" cy="12" r="10"></circle>
                                <polyline points="12 6 12 12 16 14"></polyline>
                            </svg>
                            <select v-model="scheduleTime">
                                <option value="00:00">00:00</option>
                                <option value="01:00">01:00</option>
                                <option value="02:00">02:00</option>
                                <option value="03:00">03:00</option>
                                <option value="04:00">04:00</option>
                                <option value="05:00">05:00</option>
                                <option value="06:00">06:00</option>
                                <option value="07:00">07:00</option>
                                <option value="08:00">08:00</option>
                                <option value="09:00">09:00</option>
                                <option value="10:00">10:00</option>
                                <option value="11:00">11:00</option>
                                <option value="12:00">12:00</option>
                                <option value="13:00">13:00</option>
                                <option value="14:00">14:00</option>
                                <option value="15:00">15:00</option>
                                <option value="16:00">16:00</option>
                                <option value="17:00">17:00</option>
                                <option value="18:00">18:00</option>
                                <option value="19:00">19:00</option>
                                <option value="20:00">20:00</option>
                                <option value="21:00">21:00</option>
                                <option value="22:00">22:00</option>
                                <option value="23:00">23:00</option>
                            </select>
                            <span class="select-arrow">▼</span>
                        </div>
                    </div>
                </div>
                
                <div class="cost-info">
                    <span class="cost-label">Custo:</span>
                    <span class="cost-value">100 créditos</span>
                </div>
                
                <p class="schedule-info">Impulsionamento programado por 24 horas na data selecionada.</p>
                
                <button class="action-button" @click="scheduleBoost">
                    <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="button-icon">
                        <circle cx="12" cy="12" r="10"></circle>
                        <polyline points="12 6 12 12 16 14"></polyline>
                    </svg>
                    Agendar Impulsionamento
                </button>
            </div>
        </div>
        
        <!-- Success Alert -->
        <div class="alert-container" v-if="showAlert">
            <div class="alert">
                <div class="alert-icon">✓</div>
                <div class="alert-content">
                    <div class="alert-title">Sucesso!</div>
                    <div class="alert-message">{{ alertMessage }}</div>
                </div>
                <button class="alert-close" @click="showAlert = false">×</button>
            </div>
        </div>
    </div>
</template>

<script>
import '@/assets/css/boost_content.css';
import '@/assets/css/global.css'



export default {
    name: 'BoostContent',
    data() {
        return {
            totalCredits: 250,
            selectedOption: 1000, // Start with 1000 selected to match image
            selectedPrice: 159.90,
            activeTab: 'schedule', // Start with 'schedule' to match image
            scheduleDate: '',
            scheduleTime: '18:00', // Default to 18:00 to match image
            showAlert: false,
            alertMessage: '',
            showDatePicker: false,
            currentDate: new Date(),
            currentMonth: new Date().getMonth(),
            currentYear: new Date().getFullYear(),
            weekdays: ['D', 'S', 'T', 'Q', 'Q', 'S', 'S'],
            months: [
                'Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho', 
                'Julho', 'Agosto', 'Setembro', 'Outubro', 'Novembro', 'Dezembro'
            ]
        }
    },
    computed: {
        currentMonthName() {
            return this.months[this.currentMonth];
        },
        calendarDays() {
            let days = [];
            
            // First day of the month
            const firstDay = new Date(this.currentYear, this.currentMonth, 1);
            // Last day of the month
            const lastDay = new Date(this.currentYear, this.currentMonth + 1, 0);
            
            // Days from previous month
            const daysFromPrevMonth = firstDay.getDay();
            for (let i = daysFromPrevMonth; i > 0; i--) {
                const prevDate = new Date(this.currentYear, this.currentMonth, 1 - i);
                days.push({
                    date: prevDate,
                    dayNumber: prevDate.getDate(),
                    currentMonth: false
                });
            }
            
            // Days from current month
            for (let i = 1; i <= lastDay.getDate(); i++) {
                const date = new Date(this.currentYear, this.currentMonth, i);
                days.push({
                    date: date,
                    dayNumber: i,
                    currentMonth: true
                });
            }
            
            // Fill the rest of the calendar grid
            const remainingDays = 35 - days.length; // 5 rows of 7 days
            for (let i = 1; i <= remainingDays; i++) {
                const nextDate = new Date(this.currentYear, this.currentMonth + 1, i);
                days.push({
                    date: nextDate,
                    dayNumber: i,
                    currentMonth: false
                });
            }
            
            return days;
        }
    },
    methods: {
        selectCreditOption(amount, price) {
            this.selectedOption = amount;
            this.selectedPrice = price;
        },
        buyCredits() {
            if (!this.selectedOption) {
                this.showAlertMessage('Por favor, selecione uma opção de créditos.');
                return;
            }
            
            // Simulate API call
            setTimeout(() => {
                this.totalCredits += this.selectedOption;
                this.showAlertMessage(`${this.selectedOption} créditos adicionados com sucesso!`);
            }, 500);
        },
        scheduleBoost() {
            if (this.totalCredits < 100) {
                this.showAlertMessage('Créditos insuficientes para agendar impulsionamento.');
                return;
            }
            
            if (!this.scheduleDate) {
                this.showAlertMessage('Por favor, selecione uma data para o agendamento.');
                return;
            }
            
            // Simulate API call
            setTimeout(() => {
                this.totalCredits -= 100;
                this.showAlertMessage(`Impulsionamento agendado para ${this.scheduleDate} às ${this.scheduleTime}!`);
                this.showDatePicker = false;
            }, 500);
        },
        toggleDatePicker() {
            this.showDatePicker = !this.showDatePicker;
        },
        previousMonth() {
            if (this.currentMonth === 0) {
                this.currentMonth = 11;
                this.currentYear--;
            } else {
                this.currentMonth--;
            }
        },
        nextMonth() {
            if (this.currentMonth === 11) {
                this.currentMonth = 0;
                this.currentYear++;
            } else {
                this.currentMonth++;
            }
        },
        formatDate(date) {
            const day = date.getDate().toString().padStart(2, '0');
            const month = (date.getMonth() + 1).toString().padStart(2, '0');
            const year = date.getFullYear();
            return `${day}/${month}/${year}`;
        },
        selectDate(date) {
            this.scheduleDate = this.formatDate(date);
            this.showDatePicker = false;
        },
        isSelectedDate(date) {
            if (!this.scheduleDate) return false;
            
            const [day, month, year] = this.scheduleDate.split('/').map(Number);
            return date.getDate() === day && 
                   date.getMonth() === month - 1 && 
                   date.getFullYear() === year;
        },
        isToday(date) {
            const today = new Date();
            return date.getDate() === today.getDate() && 
                   date.getMonth() === today.getMonth() && 
                   date.getFullYear() === today.getFullYear();
        },
        showAlertMessage(message) {
            this.alertMessage = message;
            this.showAlert = true;
            
            // Auto-hide alert after 5 seconds
            setTimeout(() => {
                this.showAlert = false;
            }, 5000);
        }
    }
}
</script>

<style>
/* Importing your CSS variables and using them */
.main {
    width: 100%;
    margin: 0 auto;
    padding: 0 20px;
    background-color: var(--bg-main);
    min-height: 100vh;
    color: var(--text-primary);
    font-family: 'Inter', sans-serif;
}

span{
    background: transparent;
}

.section-title {
    font-size: 1.8rem;
    font-weight: 500;
    color: var(--text-primary);
    margin-bottom: 20px;
}

.boost-container {
    background-color: var(--bg-card);
    border-radius: 8px;
    padding: 24px;
    margin-bottom: 24px;
    max-width: 1400px;
    width: 100%;
}

.section-header h2 {
    font-size: 1.2rem;
    font-weight: 500;
    color: var(--text-primary);
    margin: 0;
}

.subtitle {
    font-size: 0.9rem;
    color: var(--text-secondary);
    margin: 4px 0 20px 0;
}

.credits-display {
    background-color: var(--bg-element);
    border-radius: 8px;
    padding: 30px 20px;
    text-align: center;
    margin-bottom: 20px;
}

.credit-amount {
    font-size: 3.5rem;
    font-weight: 700;
    color: var(--primary);
    margin-bottom: 5px;
}

.credit-label {
    font-size: 0.9rem;
    color: var(--text-secondary);
}

.credit-options {
    display: flex;
    gap: 16px;
    margin-bottom: 20px;
}

.credit-option {
    flex: 1;
    background-color: var(--bg-element);
    border-radius: 6px;
    padding: 20px;
    text-align: center;
    position: relative;
    cursor: pointer;
    border: 2px solid transparent;
    transition: border-color 0.2s ease;
}

.credit-option.selected {
    border-color: var(--primary);
}

.popular-tag {
    position: absolute;
    top: -10px;
    right: -10px;
    background-color: var(--primary);
    color: white;
    font-size: 0.75rem;
    padding: 2px 8px;
    border-radius: 4px;
}

.option-amount {
    font-size: 2rem;
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 4px;
}

.option-label {
    font-size: 0.85rem;
    color: var(--text-secondary);
    margin-bottom: 15px;
}

.option-price {
    font-size: 1.1rem;
    font-weight: 500;
    color: var(--text-primary);
}

.action-button {
    width: 100%;
    background-color: var(--primary);
    color: var(--text-primary);
    border: none;
    border-radius: 4px;
    padding: 14px 0;
    font-size: 1rem;
    font-weight: 500;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    transition: background-color 0.2s ease;
}

.action-button:hover {
    background-color: var(--primary-hover);
}

.button-icon {
    color: var(--text-primary);
}

.tab-buttons {
    display: flex;
    margin-bottom: 20px;
    border-bottom: 1px solid var(--bg-element);
}

.tab-button {
    background: none;
    border: none;
    color: var(--text-secondary);
    font-size: 1rem;
    padding: 10px 0;
    margin-right: 20px;
    cursor: pointer;
    position: relative;
}

.tab-button.active {
    color: var(--primary);
}

.tab-button.active::after {
    content: '';
    position: absolute;
    bottom: -1px;
    left: 0;
    width: 100%;
    height: 2px;
    background-color: var(--primary);
}

.tab-content {
    background-color: var(--bg-element);
    border-radius: 6px;
    padding: 20px;
}

.cost-info {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
}

.cost-label {
    color: var(--text-secondary);
    margin-right: 8px;
}

.cost-value {
    color: var(--primary);
    font-weight: 500;
}

.schedule-info {
    color: var(--text-secondary);
    font-size: 0.9rem;
    margin-bottom: 20px;
}

.schedule-fields {
    display: flex;
    gap: 20px;
    margin-bottom: 20px;
}

.field {
    flex: 1;
    position: relative;
}

.field label {
    display: block;
    color: var(--text-primary);
    margin-bottom: 8px;
}

.date-picker, .time-select {
    background-color: var(--bg-card);
    border-radius: 4px;
    padding: 12px;
    display: flex;
    align-items: center;
    position: relative;
}

.date-picker-icon {
    margin-right: 8px;
    color: var(--text-secondary);
}

.date-picker input {
    background: none;
    border: none;
    color: var(--text-primary);
    width: 100%;
    outline: none;
    font-size: 0.9rem;
}

.time-select select {
    background: none;
    border: none;
    color: var(--text-primary);
    width: 100%;
    outline: none;
    appearance: none;
    font-size: 0.9rem;
}

.select-arrow {
    position: absolute;
    right: 12px;
    top: 50%;
    transform: translateY(-50%);
    color: var(--text-secondary);
    font-size: 0.7rem;
}

/* Calendar Styles */
.calendar-container {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 10;
    width: 200px;
    height: 220px;
    background-color: var(--bg-element);
    border-radius: 4px;
    padding: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    margin-top: 4px;
    border: 1px solid var(--border-primary);
}

.calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
}

.calendar-title {
    color: var(--text-primary);
    font-weight: 500;
}

.calendar-nav {
    background: none;
    border: none;
    color: var(--primary);
    cursor: pointer;
    font-size: 1rem;
    width: 24px;
    height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
}

.calendar-nav:hover {
    background-color: rgba(240, 62, 62, 0.1);
}

.calendar-weekdays {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    margin-bottom: 8px;
}

.weekday {
    color: var(--text-secondary);
    font-size: 0.8rem;
    text-align: center;
    padding: 4px;
}

.calendar-days {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 4px;
}

.calendar-day {
    width: 100%;
    aspect-ratio: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--text-primary);
    font-size: 0.9rem;
    cursor: pointer;
    border-radius: 50%;
    transition: background-color 0.2s ease;
}

.calendar-day:hover {
    background-color: rgba(240, 62, 62, 0.1);
}

.calendar-day.other-month {
    color: #666;
}

.calendar-day.selected {
    background-color: var(--primary);
    color: var(--text-primary);
}

.calendar-day.today {
    border: 2px solid var(--primary);
}

/* Alert Styles */
.alert-container {
    position: fixed;
    bottom: 20px;
    left: 20px;
    z-index: 1000;
}

.alert {
    display: flex;
    align-items: center;
    background-color: var(--bg-element);
    border-left: 4px solid #2ecc71;
    border-radius: 4px;
    padding: 16px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    width: 300px;
    animation: slideIn 0.3s ease-out;
}

.alert-icon {
    background-color: rgba(46, 204, 113, 0.1);
    color: #2ecc71;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 12px;
}

.alert-content {
    flex: 1;
}

.alert-title {
    font-weight: 600;
    color: var(--text-primary);
    margin-bottom: 4px;
}

.alert-message {
    font-size: 0.9rem;
    color: var(--text-secondary);
}

.alert-close {
    background: none;
    border: none;
    color: var(--text-secondary);
    font-size: 1.2rem;
    cursor: pointer;
    padding: 0;
}

@keyframes slideIn {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}
</style>