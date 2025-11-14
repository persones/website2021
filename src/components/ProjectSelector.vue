<template>
  <div class="project-selector" >
    <div class="desktop">
      <div class="border-box project-list-container">
        <div class="item-button" 
          v-for="project in projects"
          :key=project.title
          @click="$emit('selectProject', project)">
            <span v-html="project.title" class="selectable" :class="{ selected : project == selectedProject}"></span>
        </div>
      </div>
    </div>
    <div class="mobile">
      <div class="collapse-projects" @click=toggleCollapse :class="{collapsed : collapse}">&gt;</div>
      <div class="project-thumbnails-container" :class="{collapsed : collapse}">
        <div class="project-thumbnail" 
          v-for="project in projects"
          :key=project.title
          @click="$emit('selectProject', project)">
            <span v-html="project.title" class="selectable" :class="{ selected : project == selectedProject}"></span>
        </div>
      </div>
    </div>
  </div>
</template>
<style>

  .project-selector {
    width: 25%;
    flex-grow: 0;
    flex-shrink: 0;
  }

  @media only screen and (max-width: 1000px) {
    .project-selector {
      width: 100%;
      background-color: white;
    }
  }

  .project-list-container {
    padding-right: 1em;
    display: flex;
    flex-direction: column;
  }

  .project-selector .mobile  .project-thumbnails-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-content: flex-start
  }

  .project-thumbnail {
    border: 1px solid black;
    padding: 0.2em;
    margin: 0.1em;
    height: 1.5em;
  }
  
  .project-thumbnails-container {
    height: 1000px;
    background-color: white;
    transition: all 1s;
    position: absolute;
    z-index: 100;
    overflow-y: hidden;
  }
  .project-thumbnails-container.collapsed {
    height: 0%;
  }

  .collapse-projects {
    text-align: center;
    font-size: 2em;
    transform: rotate(450deg);
    transition: all 1.5s;
  }

  .collapse-projects.collapsed {
    transform: rotate(0deg);
  }

</style>
<script>                                                                                                           
export default {
  name: 'Project',
  emits: ['selectProject'],
  props: {
    projects: Array,
    selectedProject: Object
  },
  data: function() {
    return {
      collapse: false
    }
  },
  methods: {
    toggleCollapse() {
      this.collapse = !this.collapse;
    }
  }, 
  watch: {
    selectedProject: function() {
      this.collapse = true;
    }
  }
}
</script>