<script setup>
import { useRouter } from 'vue-router';
import EventCard from '@/components/EventCard.vue';
import EventService from '@/services/EventService.js';
import { onMounted, ref, computed, watchEffect } from 'vue';

const props = defineProps(['page'])
const events = ref(null)
const router = useRouter()
const totalEvents = ref(0)
const page = computed(() => props.page)
const hasNextPages = computed(() => {
  const totalPages = Math.ceil(totalEvents.value / 3)
  return page.value < totalPages
})

onMounted(() => {
  watchEffect(() => {
    events.value = null
    EventService.getEvents(3, page.value)
      .then((response) => {
        console.log('response', response.data);
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        if (error.response && error.response.status == 404) {
          console.log('error', error);
          router.push({ name: '404-resource', params: { resource: "events" } }) 
        } else {
          router.push({ name: "network-error" });
        }
      })
  })
})
</script>

<template>
  <div class="events">
    <h1>Event For Good</h1>
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <div class="pagination">
      <RouterLink id="page-prev" :to="{ name: 'event-list', query: { page: page - 1 } }" rel="prev" v-if="page != 1">
        &#60; Previou
      </RouterLink>
      <RouterLink id="page-next" :to="{ name: 'event-list', query: { page: page + 1 } }" rel="next" v-if="hasNextPages">
        Next &#62; 
      </RouterLink>
    </div>
  </div>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pagination {
  display: flex;
  width: 290px;
}

.pagination a {
  flex: 1;
  text-decoration: none;
  color: #2c3e50;
}

#page-prev {
  text-align: left;
}

#page-next {
  text-align: right;
}
</style>
