<script setup lang="ts">
import { cdate } from "cdate"

const now = cdate().tz("Asia/Tokyo")
const start = now.startOf("month").startOf("week")
const end = now.endOf("month").endOf("week")

const weeks:string[][] = [];
for (let day = start; +day < +end;) {
  const week = []
  for (let i = 0; i < 7; i++) {
    week.push(day.format("YYYY/MM/DD"))
    day = day.next("day")
  }
  weeks.push(week)
}
</script>

<template>
  <h1>{{ now.format("MMMM YYYY") }}</h1>

  <table>
    <tr
      v-for="week in weeks">
      <td
        v-for="day in week"
        :class="{ 'text-red': day === now.format('YYYY/MM/DD') }">
        {{ cdate(day).format("DD") }}
      </td>
    </tr>
  </table>
</template>

<style scoped>
.text-red {
  color: red;
}
</style>
