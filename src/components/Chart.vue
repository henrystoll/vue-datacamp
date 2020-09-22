<script>
import {Line} from 'vue-chartjs'

export default {
    name: 'Chart',
    extends: Line,
    props: {
        labels: Array,
        datasets: Array
    },
    mounted() {
        this.renderChart({
            labels: this.labels,
            datasets: this.datasets,
        }, {
            legend: {
                position: 'right',
                align: 'start'
            },
            tooltips: {
                mode: "nearest",
                intersect: false
            }
        })
    }
}
</script>

<style scoped>
</style>