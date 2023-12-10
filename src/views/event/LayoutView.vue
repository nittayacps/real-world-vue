<script setup>
import EventService from '@/services/EventService.js';
import { onMounted, ref } from 'vue';

const event = ref(null)
const props = defineProps({
    id: { required: true}
})

onMounted(() => {
  EventService.getEvent(props.id)
  .then((response) => {
   console.log('response', response.data);
   event.value = response.data
  })
  .catch((error) => {
    console.log('error', error);
  })
})
</script>

<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <div id="nav">
      <router-link :to="{ name: 'event-details' }">Details</router-link>
      |
      <router-link :to="{ name: 'event-register' }">Register</router-link>
      |
      <router-link :to="{ name: 'event-edit' }">Edit</router-link>
    </div>
    <RouterView :event="event" />
  </div>
</template>
