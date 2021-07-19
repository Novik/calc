<template>
    <div>
        <md-table v-if="results.length > 0" v-model="results" md-sort="discount" md-sort-order="asc" :md-sort.sync="currentSort" :md-sort-order.sync="currentSortOrder" :md-sort-fn="customSort">
            <md-table-row slot="md-table-row" slot-scope="{ item }">
                <md-table-cell md-label="Срок кредита" md-sort-by="duration" md-numeric>{{ item.duration }}</md-table-cell>
                <md-table-cell md-label="Размер дисконта" md-sort-by="discount" md-numeric>{{ item.discount }}</md-table-cell>
                <md-table-cell md-label="Итоговая ставка" md-sort-by="summary" md-numeric>{{ item.summary }}</md-table-cell>
                <md-table-cell md-label="Общая выплата" md-sort-by="overall" md-numeric>{{ item.overall }}</md-table-cell>
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
            type: Array,
            required: true
        },
    },

    data: function() {
        return({
            currentSort: 'discount',
            currentSortOrder: 'asc',

            standard: 8.3,
            max: 4.00,
            konsts: {
                "242": [
                    {
                        discount: 0.1,
                        commission: 0.37,
                    },{
                        discount: 0.2,
                        commission: 0.75,
                    },{
                        discount: 0.3,
                        commission: 1.12,
                    },{
                        discount: 0.4,
                        commission: 1.49,
                    },{
                        discount: 0.5,
                        commission: 1.86,
                    },{
                        discount: 0.6,
                        commission: 2.23,
                    },{
                        discount: 0.7,
                        commission: 2.60,
                    },{
                        discount: 0.8,
                        commission: 2.97,
                    },{
                        discount: 0.9,
                        commission: 3.33,
                    },{
                        discount: 1.0,
                        commission: 3.70,
                    },{
                        discount: 1.1,
                        commission: 4.07,
                    },{
                        discount: 1.2,
                        commission: 4.43,
                    },{
                        discount: 1.3,
                        commission: 4.79,
                    },{
                        discount: 1.4,
                        commission: 5.16,
                    },{
                        discount: 1.5,
                        commission: 5.52,
                    },{
                        discount: 1.6,
                        commission: 5.88,
                    },{
                        discount: 1.7,
                        commission: 6.24,
                    },{
                        discount: 1.8,
                        commission: 6.60,
                    },{
                        discount: 1.9,
                        commission: 6.96,
                    },{
                        discount: 2.0,
                        commission: 7.32,
                    },{
                        discount: 2.1,
                        commission: 7.67,
                    },{
                        discount: 2.2,
                        commission: 8.03,
                    },{
                        discount: 2.3,
                        commission: 8.38,
                    },{
                        discount: 2.4,
                        commission: 8.74,
                    },{
                        discount: 2.5,
                        commission: 9.09,
                    },{
                        discount: 2.6,
                        commission: 9.44,
                    },{
                        discount: 2.7,
                        commission: 9.80,
                    },{
                        discount: 2.8,
                        commission: 10.15,
                    },{
                        discount: 2.9,
                        commission: 10.50,
                    },{
                        discount: 3.0,
                        commission: 10.84,
                    },{
                        discount: 3.1,
                        commission: 11.19,
                    },{
                        discount: 3.2,
                        commission: 11.54,
                    },{
                        discount: 3.3,
                        commission: 11.89,
                    },{
                        discount: 3.4,
                        commission: 12.23,
                    },{
                        discount: 3.5,
                        commission: 12.57,
                    },{
                        discount: 3.6,
                        commission: 12.92,
                    },{
                        discount: 3.7,
                        commission: 13.26,
                    },{
                        discount: 3.8,
                        commission: 13.60,
                    },{
                        discount: 3.9,
                        commission: 13.94,
                    },{
                        discount: 4.0,
                        commission: 14.28,
                    }
                ],
                "12": [
                    {
                        discount: 3.8,
                        commission: 3.48,
                    },{
                        discount: 3.9,
                        commission: 3.57,
                    },{
                        discount: 4.0,
                        commission: 3.66,
                    },{
                        discount: 4.1,
                        commission: 3.75,
                    },{
                        discount: 4.2,
                        commission: 3.84,
                    },{
                        discount: 4.3,
                        commission: 3.93,
                    },{
                        discount: 4.4,
                        commission: 4.02,
                    },{
                        discount: 4.6,
                        commission: 4.20,
                    },{
                        discount: 4.7,
                        commission: 4.30,
                    },{
                        discount: 4.8,
                        commission: 4.39,
                    },{
                        discount: 4.9,
                        commission: 4.48,
                    },{
                        discount: 5.0,
                        commission: 4.57,
                    },{
                        discount: 5.1,
                        commission: 4.66,
                    },{
                        discount: 5.2,
                        commission: 4.75,
                    },{
                        discount: 5.3,
                        commission: 4.84,
                    },{
                        discount: 5.4,
                        commission: 4.93,
                    },{
                        discount: 5.5,
                        commission: 5.01,
                    },{
                        discount: 5.7,
                        commission: 5.20,
                    },{
                        discount: 5.8,
                        commission: 5.31,
                    },{
                        discount: 5.9,
                        commission: 5.39,
                    },{
                        discount: 6.1,
                        commission: 5.57,
                    },{
                        discount: 6.2,
                        commission: 5.66,
                    },{
                        discount: 6.3,
                        commission: 5.75,
                    },{
                        discount: 6.4,
                        commission: 5.84,
                    },{
                        discount: 6.5,
                        commission: 5.93,
                    },{
                        discount: 6.6,
                        commission: 6.02,
                    },{
                        discount: 6.7,
                        commission: 6.11,
                    },{
                        discount: 6.8,
                        commission: 6.21,
                    },{
                        discount: 6.9,
                        commission: 6.30,
                    },{
                        discount: 7.0,
                        commission: 6.39,
                    },{
                        discount: 7.1,
                        commission: 6.48,
                    },{
                        discount: 7.2,
                        commission: 6.57,
                    },{
                        discount: 7.3,
                        commission: 6.66,
                    },{
                        discount: 7.4,
                        commission: 6.75,
                    },{
                        discount: 7.6,
                        commission: 6.93,
                    },{
                        discount: 7.7,
                        commission: 7.02,
                    },{
                        discount: 7.8,
                        commission: 7.10,
                    },{
                        discount: 7.9,
                        commission: 7.20,
                    },{
                        discount: 8.0,
                        commission: 7.29,
                    },{
                        discount: 8.1,
                        commission: 7.38,
                    },{
                        discount: 8.2,
                        commission: 7.47,
                    },{
                        discount: 8.3,
                        commission: 7.56,
                    },{
                        discount: 8.4,
                        commission: 7.65,
                    },{
                        discount: 8.5,
                        commission: 7.74,
                    },{
                        discount: 8.6,
                        commission: 7.83,
                    },{
                        discount: 8.7,
                        commission: 7.92,
                    },{
                        discount: 8.8,
                        commission: 8.01,
                    },{
                        discount: 8.9,
                        commission: 8.10,
                    },{
                        discount: 9.0,
                        commission: 8.19,
                    },{
                        discount: 9.1,
                        commission: 8.28,
                    },{
                        discount: 9.2,
                        commission: 8.37,
                    },{
                        discount: 9.3,
                        commission: 8.46,
                    },{
                        discount: 9.4,
                        commission: 8.55,
                    },{
                        discount: 9.5,
                        commission: 8.64,
                    },{
                        discount: 9.6,
                        commission: 8.73,
                    },{
                        discount: 9.7,
                        commission: 8.82,
                    },{
                        discount: 9.8,
                        commission: 8.91,
                    },{
                        discount: 9.9,
                        commission: 9.0,
                    },{
                        discount: 10.0,
                        commission: 9.09,
                    }
                ],
                "24": [
                    {
                        discount: 3.8,
                        commission: 6.10,
                    },{
                        discount: 3.9,
                        commission: 6.33,
                    },{
                        discount: 4.0,
                        commission: 6.49,
                    },{
                        discount: 4.1,
                        commission: 6.65,
                    },{
                        discount: 4.2,
                        commission: 6.81,
                    },{
                        discount: 4.3,
                        commission: 6.97,
                    },{
                        discount: 4.4,
                        commission: 7.13,
                    },{
                        discount: 4.5,
                        commission: 7.29,
                    },{
                        discount: 4.6,
                        commission: 7.45,
                    },{
                        discount: 4.7,
                        commission: 7.61,
                    },{
                        discount: 4.8,
                        commission: 7.77,
                    },{
                        discount: 4.9,
                        commission: 7.93,
                    },{
                        discount: 5.0,
                        commission: 8.09,
                    },{
                        discount: 5.1,
                        commission: 8.25,
                    },{
                        discount: 5.2,
                        commission: 8.41,
                    },{
                        discount: 5.3,
                        commission: 8.57,
                    },{
                        discount: 5.4,
                        commission: 8.73,
                    },{
                        discount: 5.5,
                        commission: 8.85,
                    },{
                        discount: 5.6,
                        commission: 9.02,
                    },{
                        discount: 5.7,
                        commission: 9.20,
                    },{
                        discount: 5.8,
                        commission: 9.37,
                    },{
                        discount: 5.9,
                        commission: 9.52,
                    },{
                        discount: 6.0,
                        commission: 9.68,
                    },{
                        discount: 6.1,
                        commission: 9.84,
                    },{
                        discount: 6.2,
                        commission: 10.00,
                    },{
                        discount: 6.3,
                        commission: 10.16,
                    },{
                        discount: 6.4,
                        commission: 10.32,
                    },{
                        discount: 6.5,
                        commission: 10.47,
                    },{
                        discount: 6.6,
                        commission: 10.63,
                    },{
                        discount: 6.7,
                        commission: 10.79,
                    },{
                        discount: 6.8,
                        commission: 10.95,
                    },{
                        discount: 6.9,
                        commission: 11.10,
                    },{
                        discount: 7.0,
                        commission: 11.26,
                    },{
                        discount: 7.1,
                        commission: 11.42,
                    },{
                        discount: 7.2,
                        commission: 11.58,
                    },{
                        discount: 7.3,
                        commission: 11.73,
                    },{
                        discount: 7.4,
                        commission: 11.89,
                    },{
                        discount: 7.5,
                        commission: 12.05,
                    },{
                        discount: 7.6,
                        commission: 12.20,
                    },{
                        discount: 7.7,
                        commission: 12.36,
                    },{
                        discount: 7.8,
                        commission: 12.52,
                    },{
                        discount: 7.9,
                        commission: 12.67,
                    },{
                        discount: 8.0,
                        commission: 12.83,
                    },{
                        discount: 8.1,
                        commission: 12.99,
                    },{
                        discount: 8.2,
                        commission: 13.14,
                    }
                ]
            }
        });
    },

    computed: {
        results: {
            get() {
                let result = []
                if( this.cost > 0 ) {
                    const overall = (parseInt(this.cost) - parseInt(this.initial)) || 0;

                    this.duration.forEach( (v) => {
                        const ret = this.konsts[v].filter( (e) => {
                            return( e.commission * overall / this.cost < this.max );
                        });

                        if(ret.length) {
                            const max = ret.reduce(function(prev, current) {
                                return (prev.discount > current.discount) ? prev : current
                            });
                            const summary = this.standard - max.discount;
                            result.push({
                                duration: v,
                                discount: max.discount.toFixed(2),
                                summary: summary.toFixed(2),
                                overall: (summary * this.cost).toFixed(2)
                            });
                        }
                    });
                }
                return(result);
            },

            set(v) {
            }
        }
    },

    methods: {
        customSort (value) {
            return value.sort((a, b) => {
                const sortBy = this.currentSort
                if (this.currentSortOrder === 'desc') {
                    return(a[sortBy] - b[sortBy]);
                }
                return(b[sortBy] - a[sortBy]);
            });
        }
    }
}
</script>

<style>
</style>