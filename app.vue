<script setup lang="ts">
useHead({
  bodyAttrs: {
    class: 'w-max',
  },
})

function getTimeRange(start: string, end: string, step: number = 15) {
  const [startHour, startMinute] = start.split(':').map(Number)
  const [endHour, endMinute] = end.split(':').map(Number)

  let startTime = new Date(0, 0, 0, startHour, startMinute)
  const endTime = new Date(0, 0, 0, endHour, endMinute)
  const timeRange = []

  while (startTime <= endTime) {
    timeRange.push(startTime.toLocaleTimeString('pl-PL', { hour: 'numeric', minute: '2-digit' }))
    startTime = new Date(startTime.setMinutes(startTime.getMinutes() + step))
  }

  return timeRange
}

interface LessonPlan {
  day: string
  start: string
  end: string
  name: string
}

const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
const lessons: LessonPlan[] = [
  { start: '13:00', end: '13:45', name: 'Ergonomia i BHP', day: 'Monday' },
  { start: '14:00', end: '15:30', name: 'Rozumowanie algorytmiczne', day: 'Monday' },
  { start: '15:45', end: '17:15', name: 'Matematyka dyskretna', day: 'Monday' },
  { start: '17:30', end: '19:00', name: 'Wstęp do programowania', day: 'Monday' },

  { start: '9:00', end: '10:30', name: 'Podstawy matematyki wyższej', day: 'Tuesday' },
]
</script>

<template>
  <table class="border-separate text-left w-screen">
    <thead>
      <tr>
        <th class="text-center">
          Time
        </th>
        <th class="text-center">
          Monday
        </th>
        <th class="text-center">
          Tuesday
        </th>
        <th class="text-center">
          Wednesday
        </th>
        <th class="text-center">
          Thursday
        </th>
        <th class="text-center">
          Friday
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="time in getTimeRange('8:00', '20:00')" :id="time" :key="time" class="snap-center border py-2">
        <td class="sticky left-0 bg-blue-300">
          {{ time }}
        </td>
        <template v-for="day in days" :key="day">
          <template v-for="lesson in lessons" :key="lesson.name">
            <template v-if="lesson.day === day && lesson.start === time">
              <td v-for="d in days.findIndex(d => d === day)" :key="d" />
              <td :rowspan="getTimeRange(lesson.start, lesson.end).length" class="bg-blue-200 text-center">
                {{ lesson.name }}
              </td>
              <td v-for="d in days.length - days.findIndex(d => d === day) - 1" :key="d" />
            </template>
          </template>
        </template>
      </tr>
    </tbody>
  </table>
</template>
