<template>
  <div id="app">
    <div class='roundContainer'>
      <round-rotate :balls='imgArr' :radius='0.85' :duration='2' :count='1'></round-rotate>
    </div>
  </div>
</template>

<script>
import RoundRotate from '../src/index'
export default {
  name: 'app',
  data () {
    return {
      imgArr: [
        require('./assets/1.png'),
        require('./assets/2.png'),
        require('./assets/3.png'),
        require('./assets/4.png'),
        require('./assets/5.png')
      ]
    }
  },
  components: {
    RoundRotate
  },
  mounted: function() {
    console.log(this)
  }
}
</script>

<style>
  .roundContainer {
    width: 500px;
    height: 500px;
  }
</style>
