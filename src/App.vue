<template>
  <div>
    <List></List>
  </div>
</template>

<script>
import List from './components/list.vue'

export default {
  name: 'app',
  components: {
    List
  }
}
</script>

<style>
body{
    margin: 0;
}
*{
  padding: 0;
  margin: 0;
}
</style>
