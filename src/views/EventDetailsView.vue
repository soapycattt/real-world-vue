<script setup>
import { ref, onMounted } from 'vue'
import EventService from '@/services/EventService.js'

const event = ref(null)

const props = defineProps(['id'])

onMounted(() => {
  EventService.getEvent(props.id)
    .then((response) => {
      console.log(response)
      event.value = response.data
      console.log(event)
    })
    .catch((error) => {
      console.log(error)
    })
})
</script>

<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
    <p>{{ event.description }}</p>
  </div>

  <div v-else>
    <h3>Loading...</h3>
  </div>
</template>
