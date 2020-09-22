import {isSameDay} from "date-fns";import {format} from "date-fns";import {parseJSON} from "date-fns";
<template>
  <div class="home">
    <Chart
      v-if="datasets"
      :labels="labels"
      :datasets="datasets"
    />
    <RiseLoader
      v-else
      class="center"
      color="#42b983"
      :size="30"
      margin="6px"
    />
  </div>
</template>

<script>
import axios from "axios"
import {parseJSON, isSameDay, format} from 'date-fns'
import randomColor from "randomcolor"

import Chart from "../components/Chart"
import {RiseLoader} from '@saeris/vue-spinners'


export default {
    name: 'Home',
    components: {
        Chart,
        RiseLoader
    },
    data() {
        return {
            labels: null,
            datasets: null
        }
    },
    async mounted() {
        const res = await axios.get('https://cbs2020-datacamp.azurewebsites.net/get_results')
        const data = res.data.Results

        let previousDate = new Date(0)
        const alternateDateFormat = date => {
            const tempDate = new Date(previousDate)
            previousDate = date
            return isSameDay(tempDate, date) ? format(date, 'p') : format(date, 'Pp');
        }
        this.labels = data.filter(e => e.email === "bibe20ac@student.cbs.dk")
            .map(e => e.date |> parseJSON |> alternateDateFormat)

        const uniqueNames = new Set(data.map(e => e.name))
        const names = {}
        for (const name of uniqueNames) {
            names[name] = {
                label: name,
                data: data.filter(e => e.name === name).map(e => e.xp),
                // chaptersCompleted: e.chapters_completed,
                // coursesCompleted: e.courses_completed,
                borderColor: randomColor(),
                fill: false,
            }
        }
        this.datasets = Object.values(names)
        // .filter(e => e.data[0] > 20000)
    },
}
</script>
<style scoped>
  .home {
    height: 90vh;
    width: 100vw;
  }

  .center {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
