<template>
  <p>Registration form here</p>
  <button @click="register">Register Me</button>
</template>

<script>
export default {
  props: ['id', 'organizer'],
  inject: ['GStore'],
  methods: {
    register() {
      this.GStore.flashMessage =
        'You are successfully register for ' + this.organizer.name
      setTimeout(() => {
        this.GStore.flashMessage = ''
      }, 3000)
      this.$router.push({
        name: 'EventDetails',
        params: { id: this.organizer.id }
      })
    }
  }
}
</script>
