<template>
  <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
  <p>{{ event.description }}</p>
  <img v-for="url in event.imageUrls" :key="url" :src="url" />
  {{ event }}
</template>

<script>
export default {
  props: ['id', 'event']
}
</script>
