<template>
  <div>
    <h3>Form</h3>
    <select v-model="selectedVal">
      <option v-for="(obj, index) in toChildProp" :key="index" :value="obj.name" >
        {{ obj.name }}
      </option>
    </select>
    <h3>Selected: {{ selectedVal }}</h3>
  </div>  
</template>
<script>
export default {
  name: 'Form',
  props: ['toChildProp', 'favFruit'],
  data() {
    return {
      dropdownVals: [],
      selectedVal: '',
    };
  },
  created() {
    this.selectedVal = this.favFruit;
  },
};
</script>
<style scoped>

</style>

