<template>
  <header class="appHeader fixed-top border-bottom bg-light">
    <div>
      <span class="text-success font-weight-bold">S</span>earch 
      <span class="text-success font-weight-bold">M</span>eeting 
      <span class="text-success font-weight-bold">P</span>lace
    </div>
    <BIconQuestionCircleFill class="cursor" @click="check" />
  </header>
</template>

<script>
import { BIconQuestionCircleFill } from 'bootstrap-icons-vue'

export default {
  components: {
    BIconQuestionCircleFill
  },
  methods: {
    check () {
      console.log("check")
    }
  }
}
</script>

<style scoped>
.appHeader {
  display: flex;
  padding: 0.25rem 1.5rem;
  font-size: 25px;
}

@media screen and (max-width: 400px) {
  .appHeader {
    font-size: 20px;
  }
}

.cursor {
  margin: auto 0 auto auto;
  cursor: pointer;
}
</style>