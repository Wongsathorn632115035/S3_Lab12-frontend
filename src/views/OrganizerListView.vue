<template>
  <h1>Organizer List</h1>
  <div class="organizers">
    <OrganizerCard
      v-for="organizer in organizers"
      :key="organizer.id"
      :organizer="organizer"
    ></OrganizerCard>

    <div class="pagination">
      <router-link
        id="page-prev"
        :to="{ name: 'OrganizerList', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
      >
        Prev Page
      </router-link>

      <router-link
        id="page-next"
        :to="{ name: 'OrganizerList', query: { page: page + 1 } }"
        rel="next"
        v-if="hasNextPageOrganizer"
      >
        Next Page
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import OrganizerCard from '@/components/OrganizerCard.vue'
import OrganizerService from '@/services/OrganizerService.js'
export default {
  name: 'OrganizerListView',
  props: {
    page: {
      type: Number,
      required: true
    }
  },
  components: {
    OrganizerCard
  },
  data() {
    return {
      organizers: null,
      totalOrganizers: 0
    }
  },
  // eslint-disable-next-line no-unused-vars
  beforeRouteEnter(routeTo, routeFrom, next) {
    OrganizerService.getOrganizers(3, parseInt(routeTo.query.page) || 1)
      .then((response) => {
        next((comp) => {
          comp.organizers = response.data
          comp.totalOrganizers = response.headers['x-total-count']
        })
      })
      .catch(() => {
        next({ name: 'NetworkError' })
      })
  },
  beforeRouteUpdate(routeTo) {
    return OrganizerService.getOrganizers(3, parseInt(routeTo.query.page) || 1)
      .then((response) => {
        this.organizers = response.data // <---
        this.totalOrganizers = response.headers['x-total-count'] // <---
      })
      .catch(() => {
        return { name: 'NetworkError' } // <---
      })
  },
  computed: {
    hasNextPageOrganizer() {
      let totalPages = Math.ceil(this.totalOrganizers / 3)
      return this.page < totalPages
    }
  }
}
</script>
<style scoped>
.organizers {
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
