<script setup>
import { ref, onMounted, computed, watchEffect } from 'vue'
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'

const events = ref(null);
const props = defineProps(['page']);

const page = computed(() => props.page);
const perPage = 2;

onMounted(() => {
  watchEffect(() => {
    events.value = null
    EventService.getEvents(2, page.value)
      .then((response) => {
        console.log(response)
        events.value = response.data
        console.log(events)
      })
      .catch((error) => {
        console.log(error)
      })
  })
})

</script>

<template>
  <h1>Events For Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />

    <router-link :to="{ name: 'event-list', query: { page: page - 1 } }" rel="prev" v-if="page != 1">Prev
      Page</router-link>

    <router-link :to="{ name: 'event-list', query: { page: page + 1 } }" rel="next">Next Page</router-link>

  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
