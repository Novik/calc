<template>
  <md-app md-waterfall md-mode="fixed">
    <md-app-toolbar class="md-primary">
      <span class="md-title">Кредитный калькулятор</span>
    </md-app-toolbar>
    <md-app-content>
      <md-steppers :md-active-step.sync="active" md-vertical>
        <md-step id="first" md-label="Данные клиента">
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-xsmall-size-100 md-small-size-66 md-medium-size-50 md-large-size-33 md-xlarge-size-33">
              <md-field>
                <label>Стоимость квартиры</label>
                <md-input v-model="cost" type="number" min="0" required></md-input>
                <span class="md-suffix">&nbsp;&nbsp;руб</span>
                <span class="md-error">There is an error</span>
              </md-field>
              
              <md-field>
                <label>Первоначальный взнос</label>
                <md-input v-model="initial" type="number" min="0" required></md-input>
                <span class="md-suffix">&nbsp;&nbsp;руб</span>
                <span class="md-error">There is an error</span>
              </md-field>

              <md-field>
                <label>Программа кредитования</label>
                <md-select v-model="programm" md-dense required>
                  <md-option value="standard">Стандартная программа</md-option>
                  <md-option value="family">Семейная ипотека</md-option>
                </md-select>
                <span class="md-suffix">мес</span>
              </md-field>
             
              <md-field>
                <label>Срок кредитования</label>
                <md-input v-model="duration" type="number" min="1" required></md-input>
                <span class="md-suffix">лет</span>
                <span class="md-error">There is an error</span>
              </md-field>

            </div>
            <div class="md-layout-item md-xsmall-size-100 md-small-size-66 md-medium-size-50 md-large-size-33 md-xlarge-size-33">

              <md-field>
                <label>Сумма кредита</label>
                <md-input v-model="sum" readonly></md-input>
                <span class="md-suffix">&nbsp;&nbsp;руб</span>
              </md-field>

              <md-field>
                <label>Компенсационная выплата</label>
                <md-input v-model="pay" readonly></md-input>
                <span class="md-suffix">&nbsp;&nbsp;руб</span>
              </md-field>
                
              <md-field>
                <label>Компенсационная выплата</label>
                <md-input v-model="pay_percent" readonly></md-input>
                <span class="md-suffix">&nbsp;&nbsp;%</span>
              </md-field>

              <md-field>
                <label>Срок кредитования</label>
                <md-input v-model="duration_month" readonly></md-input>
                <span class="md-suffix">мес</span>
              </md-field>
            </div>
          </div>
        </md-step>
        <md-step id="second" md-label="Рассчитать">
          <result v-if="active == 'second'"
            :cost="parseInt(cost) || 0"
            :initial="parseInt(initial) || 0"
            :duration="parseInt(duration_month) || 0"
            :programm="programm"
            :max_discount="max_discount"
          />
        </md-step>
      </md-steppers>
    </md-app-content>
  </md-app>
</template>

<script>
module.exports = {
  components: {
    'result': httpVueLoader('components/Result.vue')
  },

  data: function() {
    return {
      cost: 17000000,
      initial: 6000000,
      duration: 25,
      programm: 'standard',
      max_discount: 4.0,
      active: 'first',
    }
  },

  computed: {
    sum: {
      set(v) {
      },

      get() {
        const sum = (parseInt(this.cost) - parseInt(this.initial)) || 0;
        return(sum < 0 ? 0 : sum);
      }
    },

    duration_month: {
      set(v) {
      },

      get() {
        return(this.duration * 12);
      }
    },

    pay: {
      set(v) {
      },

      get() {
        return( (this.max_discount * this.cost) / 100 );
      }
    },

    pay_percent: {
      set(v) {
      },

      get() {
        return( this.sum > 0 ? (this.pay / this.sum * 100).toFixed(2) : 0 );
      }
    },
  }
  
}
</script>

<style>
</style>