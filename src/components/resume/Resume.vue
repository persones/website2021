<template>
  <div class="resume">
    <div class="page">
      <div class="button-container">
        <button @click="print" class="no-print">PRINT</button>
      </div>
      <div class="contact">
        <div class="my-name">Eyal Shahar</div>
        <div>http://eyalshahar.com</div>
        <div id="email">{{ email }}</div>  
        <div>(617) 386-3368</div>
      </div>   
      <Education :educations="educations"  />
      <div class="columns-container">
        <div class="column">
          <WorkCatagory :catagory="workCatagories[0]" />
          <Misc :misc="misc" /> 
        </div>
        <div class="column">
          <WorkCatagory :catagory="workCatagories[1]" />
          <WorkCatagory :catagory="workCatagories[2]" />
          <Skills :skillCatagories="skillCatagories" />
        </div>    
      </div>  
      
    </div>   
    <div class="">
      <Publications class="avoid-break" sectionTitle="Publications" :publications="publications" />
      <Talks :talks="talks" />
      <Publications class="avoid-break" sectionTitle="Conference Posters" :publications="posters" />
      <Publications class="avoid-break" sectionTitle="Patents" :publications="patents" />
      <Projects :projects="projects" />
      <Discography :diskcography="discography" />
    </div>
    
  </div>
</template>

<script>
import YAML from 'yaml'
import WorkCatagory from './WorkCatagory.vue';
import Skills from './Skills.vue'
import Publications from './Publications.vue'
import Talks from './Talks.vue'
import Projects from './Projects.vue'
import Education from './Education.vue'
import Misc from './Misc.vue'
import Discography from './Discography.vue';


export default {
  name: 'Resume',
  components: { 
    WorkCatagory,
    Skills,
    Publications,
    Talks,
    Projects,
    Education,
    Misc,
    Discography
  },
  props: {
    projects: Array
  },
  data: function() {
    return {  
      email: 'ee',
      workCatagories: [],
      skillCatagories: [],
      publications: [],
      posters: [],
      patents: [],
      talks: [],
      educations: [],
      misc: [],
      discography: []
    }
  },
  created: async function() {
    let files = [ 
      'positions', 
      'skills',
      'publications', 
      'posters', 
      'patents', 
      'talks',
      'education',
      'misc',
      'discography'
      ]; 
    for (let f of files) { 
      let t = await fetch(`./db/${f}.yaml`);
      t = await t.text();
      let j = YAML.parse(t);
      for (let key in j) {
        this[key] = this[key].concat(j[key]);
      }
    }
  },
  methods: {
    print: function() {
      this.email ='eyal' + '@' + 'eyalshahar' + '.com';
      setTimeout(window.print, 500);
    }
  }
}
</script>

<style>
  .resume {
    font-size: 11px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    text-align: left;
    width: 70em;
    margin: auto;
    overflow: auto;
  }


  
  .contact {
    display: none;
  }
  
  @media print {
    .page  {
      page-break-inside: auto;
      page-break-after: always;    
      
      /*height: 28cm;*/
      width: 100%;
    }
    
    .contact {
      display: initial;
    }

    .avoid-break, .project {
      page-break-inside: avoid;
    }


  }
  
  .my-name {
    font-size: 2em;
  }

  .contact {
    text-align: center;
    font-size: 1.2em;
  }

  .section-title {
    text-decoration: underline;
    text-decoration-color: rgb(153 182 218);
    text-decoration-thickness: 3px;
    text-underline-position: under;
    text-underline-offset: -3px;
    font-size: 1.1em;
    text-transform: uppercase;
    padding-bottom: 0.5em;
    padding-top: 1em;
  }

  .publication {
    display: flex;
    flex-direction: row;
    text-align: left;
    margin: 0.3em 0;
  }

  .publication .date {
    width: 5em;
  }

  .publication .text {
    width: 50%;
    flex-grow: 1;
  }

  .date {
    color: #9b6e00;
  }

  .columns-container {
    display: flex;
    flex-direction: row;
    align-content: space-between;
    justify-content: space-between;
    /*gap: 6em;*/
    margin: 2em 0;
  }

  .columns-container > * + * { margin-left: 6em;}

  .column {
    width: 20%;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  ul {
    list-style: square;
    padding-inline-start: 1.5em;
    margin: 0px;  

  }

  ul li::before {
    content: none;
  }

  .academic-title {
    font-style: italic;
  }

  .text {
    text-align: justify;
  }

  .button-container {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
  }
</style>
