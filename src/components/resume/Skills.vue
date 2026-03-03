<template>
  <div>
    <div class="section-title">Technical skills</div>
    <ul>
      <li v-for="skillCatagory in skillCatagories" :key="skillCatagory.skillCatagory">
        <span class="skillCatagotyName" v-html="skillCatagory.skillCatagory" />: 
        <span v-html="skillList(skillCatagory.skills)" />
      </li>  
    </ul>
  </div>
</template>

<script>
export default {
  name: "Skills",
  props: {
    "skillCatagories": Array
  }, 
  methods: {
    skillList: function (s) {
      return s.join(', ');
    }
  } 
}
</script>

<style>
  .skillCatagotyName {
    font-weight: bold;
  }

</style>