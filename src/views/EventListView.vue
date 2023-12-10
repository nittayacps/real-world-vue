<script setup>
import EventCard from '@/components/EventCard.vue';
import EventService from '@/services/EventService.js';
import { onMounted, ref } from 'vue';

const events = ref(null)

onMounted(() => {
  EventService.getEvents()
  .then((response) => {
   console.log('response', response.data);
   events.value = response.data
  })
  .catch((error) => {
    console.log('error', error);
  })
})
</script>

<template>
  <div class="events">
    <h1>Event For Good</h1>
    <EventCard v-for="event in events" :key="event.id" :event="event"/>
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
