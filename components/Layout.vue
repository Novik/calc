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
                                <label>Срок кредитования</label>
                                <md-select v-model="duration" md-dense multiple>
                                    <md-option value="12">12</md-option>
                                    <md-option value="24">24</md-option>
                                    <md-option value="242">242</md-option>
                                </md-select>
                                <span class="md-suffix">мес</span>
                            </md-field>
                        
                            <md-field>
                                <label>Сумма кредита</label>
                                <md-input v-model="sum" readonly></md-input>
                                <span class="md-suffix">&nbsp;&nbsp;руб</span>
                            </md-field>
                        </div>
                    </div>
                </md-step>
                <md-step id="second" md-label="Рассчитать">
                    <result v-if="active == 'second'"
                        :cost="parseInt(cost) || 0"
                        :initial="parseInt(initial) || 0"
                        :duration="duration"
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
            cost: 0,
            initial: 0,
            duration: [ '12' ],
            sum: 0,
            active: 'first',
        }
    },

    watch: {
        cost() {
            this.calcSum();
        },

        initial() {
            this.calcSum();
        },
    },

    computed: {

    },

    methods: {
        calcSum() {
            const sum = (parseInt(this.cost) - parseInt(this.initial)) || 0;
            this.sum = sum < 0 ? 0 : sum;
        }
    }
}
</script>

<style>
</style>