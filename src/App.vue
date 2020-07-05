<template>
  <div id="app">
   <top></top>
   <index></index>
   <bottom></bottom>
  </div>
</template>

<script>
import top from './components/01.top'
import index from './components/02.index'
import bottom from './components/03.bottom'

export default {
  name: 'App',
  components: {
    top,
    index,
    bottom,
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
li {
  list-style: none;
}
a {
  text-decoration: none;
  -webkit-tap-highlight-color:rgba(0,0,0,0);
}
#app {
  width: 7.5rem;
  margin: 0 auto;
  padding: 0;
}
</style>
