<template>
    <div>
        <the-calendar></the-calendar>
    </div>
</template>

<script>
// @ is an alias to /src
import TheCalendar from '@/components/TheCalendar.vue'

export default {
  name: 'Home',
  components: {
      TheCalendar
  }
}
</script>
