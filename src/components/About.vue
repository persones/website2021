<template>
  <div>
    <div class="text-section">
      <p>
        Hi. I'm Eyal. 
      </p>
      <p>Here is my <a v-on:click="$emit('show', 'resume')">resume</a>.</p>
      <p>I design and build new-media exhibits at the <a href="https://www.exploratorium.edu/people/eyal-shahar" target="_blank">Exploratorium</a> in San Francisco.</p>
      <p>I teach aspiring museum professionals at <a href="https://www.usfca.edu/faculty/eyal-shahar" target="_blank">the University of San Francisco</a> about museums and technology.</p>
      <p>Sometimes I <a href="https://www.apress.com/gp/book/9781484250181" target="blank">write about technology</a>. When I travel, I sometimes <a href="https://person-es.blogspot.com/" target="_blank">write about that too</a>.</p>
      <p>Before doing what I do now, I played music professionally, taught robotics and music production, and worked at a few startups, doing some DSP and software engineering.<p>
      
      <p>Contact me at eyal {at} eyalshahar {dot} com.</p>
    </div>
    <img src="images/eyal.png">
  </div>
</template>
<style scoped>
  img {
    position: absolute;
    bottom: 0;
    left: 60%;
    max-width:25%;
    max-height:75%;
    width:auto;
    height:auto;
    object-fit: contain;
    animation: fadeIn linear 1s;
    opacity: 1.0;
  }

  @keyframes fadeIn {
    0% {opacity:0;}
    100% {opacity:1;}
  }

  .text-section {
    width: 50%;
  }
</style>
<script>
export default {
  name: 'About',
  data: function() {
    return {
      publicPath: process.env.BASE_URL
    }
  },
  methods: {
    p(pp) {
      return (this.publicPath + pp)
    }
  }
}
</script>