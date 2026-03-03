<template>
  <div id="vueapp" :class="{ noscroll: showing!='resume'}">
    <link href="https://fonts.googleapis.com/css2?family=Cutive+Mono&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Cutive+Mono&family=Fira+Code:wght@300&display=swap" rel="stylesheet">
    <div id="header" class="border-box">
      <div id="name" class="no-print">Eyal Shahar</div>
      <div id="nav" class="no-print">
        <div @click="showing='projects'" class="selectable" :class="{ selected: showing=='projects' }">Projects</div>
        <div @click="showing='about'" class="selectable" :class="{ selected: showing=='about' }">About</div>
      </div>
    </div>
    <transition name="component-fade" mode="out-in">
      <div id="content-container" :key="showing">
        <ProjectSelector v-if="showing=='projects'" v-bind:projects="projects" v-bind:selectedProject="currentProject" v-on:selectProject="setCurrentProject" />
        <Project v-if="showing=='projects'" id="extended-project" v-bind="currentProject" />
        <About v-if="showing=='about'" v-on:show="show"/>
        <Resume v-if="showing=='resume'" :projects="projects" />
      </div>
    </transition>
  </div>
</template>

<script>

import Project from './components/Project.vue';
import ProjectSelector from './components/ProjectSelector.vue';
import About from './components/About.vue';
import Resume from './components/resume/Resume.vue';
import YAML from 'yaml'

export default {
  name: 'App',
  components: {
   Project,
   ProjectSelector,
   About,
   Resume
  },
  data: function() {
    return {  
      currentProject: {},
      projects: [],
      showing: 'projects'
    }
  },
  props: {
    
  },
  created: async function() {
    if (window.location.hash === '#resume') {
      this.showing = 'resume';
    }

    let files = [ 'explo', 'mit']; 
    for (let f of files) {
      let t = await fetch(`./db/${f}.yaml`);
      t = await t.text();
      let j = YAML.parse(t);
      this.projects = this.projects.concat(j.projects);
    }

    //this.currentProject = this.projects[Math.floor(Math.random() * this.projects.length)];
    this.currentProject = this.projects[0];

  },
  methods: {
    setCurrentProject(p) {
      this.currentProject = p;
    },
    show(mode) {
      console.log(mode);
      this.showing = mode;
    }
  }    
}
</script>

<style>
  @import './assets/styles/clean.css';

  #vueapp {
    box-sizing: border-box;
    padding: 2em 2em 0 2em;
    height: 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
  }

  #app.noscroll { 
    /*overflow: hidden;*/
  }

  #content-container {
    display: flex;
    flex-direction: row;
    width: 100%;
    height: 1px;
    flex-grow: 1;
    position: relative;
  }

  @media only screen and (max-width: 1000px) {
    #content-container {
      flex-direction: column;
    }
  }

.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity 0.5s ease;
}
.component-fade-enter, .component-fade-leave-to {
  opacity: 0; 
  transform: translateY(2px);
}

a, .link {
  cursor:pointer;
  color:blue;
  text-decoration:underline;
}

@media print
{  
  * {
     -webkit-print-color-adjust: exact; /* For Chrome, Safari, Edge (WebKit/Blink) */
    print-color-adjust: exact;
  }
  
  #vueapp  {
    margin-top: 0;
    padding-top: 0;
  }
  #header {
    margin-top: 0;
    padding-top: 0;
  } 
  .no-print, .no-print *
  {
      display: none !important;
  }
}

  
</style>
