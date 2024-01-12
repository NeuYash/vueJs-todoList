<script>
  import todo from './todo.vue'
  import weather from './WeatherWidget.vue'

  export default{
    components: {
      todo,
      weather,
    },
  };
</script>

<template>
  <div id="app">
    <weather />
    <todo />
    
  </div>
</template>