<template>
  <div>
    <h1 class="title">Average Students Per Session</h1>
    <div class="filters">
      <span>Filter By Site:</span>
      <span v-for="siteId in uniqueSites" :key="siteId">
        <a @click="id = siteId">{{ siteId }}</a>
      </span>
    </div>
    <table>
      <tr>
        <td>Site {{ id }}:</td>
        <td>{{ filteredAverageCount }}</td>
      </tr>
      <tr>
        <td>Overall:</td>
        <td>{{ averageCount }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import { average } from '~/utils/index'
export default {
  props: {
    records: {
      type: Array,
      default: () => []
    },
    id: {
      type: String,
      default: 'a'
    }
  },
  computed: {
    uniqueSites() {
      return new Set(this.records.map(record => record.id))
    },
    averageCount() {
        // "records": [
        //     { "am": 1, "pm": 10, "id": "a" },
        //     { "am": 2, "pm": 4, "id": "b" },
        //     { "am": 3, "pm": 12, "id": "a" },
        //     { "am": 1, "pm": 7, "id": "b" }
        // ]

      // const counts = []
      // this.records.forEach(record => {
      //     counts.push(record.am, record.pm);
      // });
      const counts = this.records.flatMap(record => [record.am, record.pm])
      return average(counts)
    },
    filteredAverageCount() {
      // const counts = []
      // this.records.forEach(record => {
      //   if (this.id === record.id) {
      //     counts.push(record.am, record.pm)
      //   }
      // });
      const counts = this.records.flatMap(record => {
        return this.id === record.id ? [record.am, record.pm] : []
      })
      return average(counts)
    }
  }
}
</script>
