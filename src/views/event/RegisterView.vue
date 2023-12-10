<script setup>
import { useRouter } from 'vue-router';

const props = defineProps({
    event: { required: true}
})
const router = useRouter()
const register = () => {
    router.push({
        name: 'event-details'
    })
}
</script>

<template>
    <p>Register event here</p>
    <button @click="register">Register</button>
</template>
