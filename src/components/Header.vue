<template>
  <div id="header">
    <div id="selectionArea">

      <div id="formSearchArea">
        <h3>Form: </h3>
        <select @change="onSelectForm" v-model="selectedForm">
          <option v-for="form in formList" :key="form" :value="form">
            {{ form }}
          </option>
        </select>
      </div>

      <div id="fieldSearchArea">
        <h3>Fields: </h3>
        <select
          @change="onSelectField"
          v-model="selectedField"
          ref="search-field">
          <option v-for="field in fieldsToChoose" :key="field" :value="field">
            {{ titleCased(field) }}
          </option>
        </select>
      </div>
    </div>
    
    <div id="controlArea">
      <button class="btn-basic" @click="search">
        Select
      </button>
      <button class="btn-basic btn-danger" @click="resetForm">Reset</button>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: 'Header',
  props: ['formFieldList'],
  data() {
    return {
      formList: [],
      selectedForm: '',
      selectedField: '',
      fieldsToChoose: [],
    };
  },
  mounted() {
    this.createFormList(this.formFieldList);
  },
  methods: {
    createFormList(formFieldObj) {
      this.formList = Object.keys(formFieldObj);
    },
    onSelectForm() {
      // console.log('selected form: ', this.selectedForm);
      this.fieldsToChoose = this.getFieldNameArr(this.selectedForm);
      this.autoFocus();
      this.selectedField = '';
    },
    autoFocus() {
      this.$refs['search-field'].focus();
    },
    getFieldNameArr(formName) {
      const fieldObj = this.formFieldList[formName];

      return Object.keys(fieldObj);
    },
    onSelectField() {
      // console.log('selected field: ', this.selectedField);
    },
    resetForm() {
      this.selectedForm = '';
      this.selectedField = '';
    },
    search() {
      const props = {
        form: this.selectedForm,
        field: this.selectedField,
      };
      
      if (this.selectedForm && this.selectedField ) {
        this.$emit('search-fields', props);
      }
    },
    titleCased(nameString) {
      return nameString.split('_').map((string) => {
        const lowerCasedString = string.toLowerCase();

        return lowerCasedString[0].toUpperCase() + lowerCasedString.slice(1);
      }).join(' ');
    },
  },
};
</script>

<style lang="scss" scoped>
  $main-dark: #333333;
  $main-blue: rgba(0, 87, 142, 1);
  $main-blue-light: #CCDDE8;
  $main-blue-dark: #002C47;
  $main-white: #F2F4F4;
  $main-yellow: #FCDB24;

  #header {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    background-color: $main-blue-light;
    color: $main-blue-dark;
    padding: 1rem 2rem;
  }
  #selectionArea {
    display: flex;
    flex-direction: column;
  }
  #formSearchArea, #fieldSearchArea {
    flex: 5;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    margin-bottom: 0.5rem;

    h3 {
      margin-right: 1rem;
      font-weight: 100;
      letter-spacing: 0.5px;
    }
  }
  select {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    letter-spacing: 1px;
    border: none;
    outline: none;
    min-width: 300px;
  }
  select:focus {
    background-color: $main-white;
  }
  #controlArea {
    flex: 2;
  }

  /* Text Selection Effects */

  ::-moz-selection { /* Code for Firefox */
    color: $main-dark;
    background: $main-yellow;
  }

  ::selection {
    color: $main-dark;
    background: $main-yellow;
  }
</style>