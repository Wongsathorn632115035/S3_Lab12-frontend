<template>
  <div>
    <h1>Create an organizer</h1>
    <form @submit.prevent="saveOrganizer">
      <BaseInput
        v-model="organizer.name"
        type="text"
        label="Name"
        class="field"
      />

      <h3>The image of the organizer</h3>
      <UploadImages @changed="handleImages" />
      <button type="submit">Submit</button>
    </form>

    <pre>{{ event }}</pre>
  </div>
</template>

<script>
import UploadImages from 'vue-upload-drop-images'
import OrganizerService from '@/services/OrganizerService.js'
export default {
  inject: ['GStore'],
  components: {
    UploadImages
  },
  data() {
    return {
      organizer: {
        name: '',
        imageUrls: []
      },
      files: []
    }
  },
  methods: {
    saveOrganizer() {
      // console.log(this.files)
      Promise.all(
        this.files.map((file) => {
          return OrganizerService.uploadFile(file)
        })
      )
        .then((response) => {
          this.organizer.imageUrls = response.map((r) => r.data)
          OrganizerService.saveOrganizer(this.organizer).then((res) => {
            console.log(res)
            this.$router.push({
              name: 'OrganizerDetails',
              params: {
                id: res.data.id
              }
            })
            this.GStore.flashMessage =
              'You are successfully add a new event for ' + res.data.name
            setTimeout(() => {
              this.GStore.flashMessage = ''
            }, 3000)
          })
        })
        .catch(() => {
          this.$router.push('NetworkError')
        })
    },
    handleImages(files) {
      console.log(files)
      this.files = files
    }
  }
}
</script>
<style scoped></style>
