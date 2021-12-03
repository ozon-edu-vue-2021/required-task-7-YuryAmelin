<template>
  <div class="person" @click="$emit('personClick', id)">
    <slot/>
    <h2>{{name}}</h2>
  </div>
</template>

<script>
export default {
  name: "Person.vue",
  props: {
    name: {
      type: String,
      default: ''
    },
    id: {
      default: null
    }
  },
}
</script>

<style scoped>

h2 {
  margin: 30px;
}

.person {
  display: flex;
  flex-direction: row;
  justify-content: left;
  width: 400px;
  align-items: center;
  cursor: pointer;
  padding: 10px;
  border-top: 1px solid #2c3e50;
  border-bottom: 1px solid #2c3e50;
}

.person:hover {
  background-color: pink;
}

</style>