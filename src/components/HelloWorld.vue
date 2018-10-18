<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>{{ pose || 'loading..'}}</p>
  </div>
</template>

<script>
import * as posenet from '@tensorflow-models/posenet';

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    net: null,
    pose: null
  },
  mounted: async function() {
    this.net = await posenet.load();
    this.detect();
  },
  methods: {
    detect: async function() {
      let imageScaleFactor = 0.50;
      let flipHorizontal = false;
      let outputStride = 16;

      let imageElement = document.getElementById('target');
      this.pose = await this.net.estimateSinglePose(imageElement, imageScaleFactor, flipHorizontal, outputStride);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
