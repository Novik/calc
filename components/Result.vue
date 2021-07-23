<template>
  <div v-if="konst">
    <md-table v-if="results.length > 0" v-model="results">
      <md-table-row v-if="duration > 24" class="bottom-border">
        <md-table-head colspan="6" class="right-border text-center"><span class="md-title">Льготный</span></md-table-head>
        <md-table-head colspan="3" class="text-center"><span class="md-title">Стандартный</span></md-table-head>
      </md-table-row>
      <md-table-row v-else class="bottom-border">
        <md-table-head colspan="9" class="text-center"><span class="md-title">Льготный</span></md-table-head>
      </md-table-row>
      <md-table-row>
        <md-table-head>Срок, мес</md-table-head>
        <md-table-head>Дисконт, %</md-table-head>
        <md-table-head>Комиссия, %</md-table-head>
        <md-table-head>Комиссия, руб</md-table-head>
        <md-table-head>Ставка, %</md-table-head>
        <template v-if="duration > 24">
          <md-table-head class="right-border">Платеж, руб/мес</md-table-head>
          <md-table-head>Осталось, мес</md-table-head>
          <md-table-head>Ставка, %</md-table-head>
          <md-table-head>Платеж, руб/мес</md-table-head>
        </template>
        <md-table-head v-else>Платеж, руб/мес</md-table-head>
      </md-table-row>
      <md-table-row v-for="(item,index) in results" :key="index" :class="item.selected ? 'good' : 'bad'">
        <md-table-cell>{{ item.duration }}</md-table-cell>
        <md-table-cell>{{ item.discount }}</md-table-cell>
        <md-table-cell>{{ item.commission_percent }}</md-table-cell>
        <md-table-cell>{{ item.commission }}</md-table-cell>
        <md-table-cell>{{ item.rate }}</md-table-cell>
        <md-table-cell>{{ item.monthly_price }}</md-table-cell>
        <template v-if="duration > 24">
          <md-table-cell>{{ item.duration_priv }}</md-table-cell>
          <md-table-cell>{{ item.rate_priv }}</md-table-cell>
          <md-table-cell>{{ item.monthly_price_priv }}</md-table-cell>
        </template>
      </md-table-row>
    </md-table>
    <span v-else class="md-caption">Ничего не найдено</span>
  </div>
</template>

<script>
module.exports = {
  props: {
    cost: {
      type: Number,
      required: true
    },
    initial: {
      type: Number,
      required: true
    },
    duration: {
      type: Number,
      required: true
    },
    programm: {
      type: String,
      required: true
    },
    max_discount: {
      type: Number,
      required: true
    }
  },

  data: function() {
    return({
      konst: null
    });
  },

  created() {
    fetch('static/data.json')
      .then((response) => response.json())
      .then((response) => {
        if( this.duration <= 24 ) {
          delete response[this.programm].discounts[0];
        }
        if( this.duration <= 12 ) {
          delete response[this.programm].discounts["24"];
        }
        this.konst = response;
      });
  },

  computed: {
    results() {
      let result = [];
      if( this.cost > 0 ) {

        const rate = this.konst[this.programm].rate;
        const sum = this.cost - this.initial;
        let border = (this.max_discount * this.cost)/sum;

        for( let [index, list] of Object.entries(this.konst[this.programm].discounts) ) {

          index = parseInt(index) || 0;
          const has_priv = ( index && (this.duration >= index) );
          list.forEach( (d) => {
            let t = (rate - d.value)/1200;

            let entry = {
              duration: index ? index : this.duration,
              discount: d.value.toFixed(2),
              commission: (d.comission * sum).toFixed(2),
              commission_percent: d.comission.toFixed(2),
              rate: (rate - d.value).toFixed(2),
              monthly_price: (sum * (t + t/( Math.pow( 1 + t, this.duration ) - 1 ))).toFixed(2),
              selected: d.comission <= border,
              duration_priv: '',
              rate_priv: '',
              monthly_price_priv: ''
            };
            if( has_priv ) {
              let rest = sum;
              let monthly_price = sum * (t + t/( Math.pow( 1 + t, this.duration ) - 1 ));
              for( let i = 0; i < index; i++ ) {
                const percents = rest * (rate - d.value) / 1200;
                rest = rest - monthly_price + percents;
              }
              entry.duration_priv = this.duration - index;
              t = rate / 1200;
              entry.rate_priv = rate.toFixed(2);
              entry.monthly_price_priv = (rest * (t + t/( Math.pow( 1 + t, entry.duration_priv ) - 1 ))).toFixed(2);
            }
            result.push(entry);
          });
        }
      }
      
      result.sort( (a,b) => {
        if( a.duration < b.duration ) {
          return(-1);
        }
        if( a.duration > b.duration ) {
          return(1);
        }
        if( a.discount < b.discount ) {
          return(-1);
        }
        if( a.discount > b.discount ) {
          return(1);
        }
        return(0);
      });

      return(result);
    }
  },

  methods: {
  }
}
</script>

<style scoped>
.right-border {
  border-right: 1px solid rgba(0, 0, 0, 0.12);
}

.bottom-border {
  border-bottom: 1px solid rgba(0, 0, 0, 0.12);
}

.text-center {
  text-align: center;
}

.good {
  background-color: #d3e0b4;
}

.bad {
  background-color: #e2afae;
}
</style>
