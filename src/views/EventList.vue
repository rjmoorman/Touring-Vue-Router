<template>
  <h1>Events for Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
    <div>
      <router-link
        id="page-prev" 
        :to="{ name: 'EventList', query: { page: page -1}}"
        rel="prev"
        v-if="page !=1"
      >Prev Page
      </router-link>
      <router-link
        id="page-next"
        :to="{ name: 'EventList', query: { page: page+1 }}"
        rel="next"
        v-if="hasNextPage"
      >Next Page</router-link>
    </div>
    
  </div>
</template>

<script>
import { watchEffect } from 'vue'
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'

export default {
  name: 'EventList',
  props: ['page'],
  components: {
    EventCard
  },
  data() {
    return {
      events: null
    }
  },
  created() {
      watchEffect(()=>{
        this.event = null
        EventService.getEvents(2, this.page)
          .then(response => {
            this.events = response.data
          })
          .catch(error => {
            console.log(error)
          })
    })
  },
  computed: {
    hasNextPage(){
      var totalPages = Math.ceil(this.totalEvents / 2) // 2 is events per page

      // Then check to see if the current page is less than the total pages.
      return this.page < totalPages
    }
  }
}
</script>

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

.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
