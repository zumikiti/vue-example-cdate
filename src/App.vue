<script setup lang="ts">
import { cdate } from "cdate"
import { onMounted, reactive } from "vue";

const now = cdate().tz("Asia/Tokyo")
const state = reactive({
  current: cdate(),
  weeks: [] as string[][],
  isLoaded: false as boolean,
})

const setWeeks = () => {
  const start = state.current.startOf("month").startOf("week")
  const end = state.current.endOf("month").endOf("week")

  state.weeks = []

  for (let day = start; +day < +end;) {
    const week = []
    for (let i = 0; i < 7; i++) {
      week.push(day.format("YYYY/MM/DD"))
      day = day.next("day")
    }
    state.weeks.push(week)
  }

  state.isLoaded = true
}

const preMonth = () => {
  state.isLoaded = false

  state.current = state.current.prev("month")
  setWeeks()
}

const nextMonth = () => {
  state.isLoaded = false

  state.current = state.current.next("month")
  setWeeks()
}

const resetMonth = () => {
  state.isLoaded = false

  state.current = now
  setWeeks()
}

onMounted(() => resetMonth())
</script>

<template>
  <template 
    v-if="state.isLoaded">
    <h1>{{ state.current.format("MMMM YYYY") }}</h1>

    <div>
      <div class="flex">
        <button
          @click="preMonth()">
          &lt;
        </button>
        <button
          @click="resetMonth()">
          now
        </button>
        <button
          @click="nextMonth()">
          &gt;
        </button>
      </div>

      <table>
        <tr
          v-for="week in state.weeks">
          <td
            v-for="day in week"
            :class="{ 'text-red': day === now.format('YYYY/MM/DD') }">
            {{ cdate(day).format("DD") }}
          </td>
        </tr>
      </table>
    </div>
  </template>
</template>

<style scoped>
.text-red {
  color: red;
}
.flex {
  display: flex;
  place-items: center;
}
</style>
