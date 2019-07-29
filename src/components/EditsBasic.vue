<template>
  <div id="editingArea">
    <div id="staticData">
      <h3>Form:
        <span class="label_1" v-if="this.selectedProps.form">
          {{ this.selectedProps.form }}
        </span>
      </h3>
      <h3>Field:
        <span class="label_1" v-if="this.selectedProps.field">
          {{ this.titleCased(this.selectedProps.field || '') }}
        </span>
      </h3>
    </div>

    <div id="dynamicData">
      <div class="basic">
        <h3>The above field is</h3>
        <select v-model="selectedEdits">
          <option v-for="(value, key, index) in editsBasicSelections" :key="index" :value="key">
            {{ value }}
          </option>
        </select>
      </div>
      
      <div class="basic">
        <h3 class="condition-operator">If</h3>
      </div>

      <div class="group">
        <div class="basic">
          <h3>This form:</h3>
          <select v-model="selectedForm" @change="onSelectForm">
            <option v-for="(form, index) in editsFormSelections" :key="index" :value="form">
              {{ form }}
            </option>
          </select>
        </div>

        <div class="basic">
          <h3>the field(s)</h3>
          <select v-model="selectedField">
            <option v-for="(field, index) in editsFieldSelections" :key="index" :value="field">
              {{ titleCased(field) }}
            </option>
          </select>
          <button class="btn-basic" @click="addField">
            + Add another field
          </button>
        </div>

        <div class="basic render-selection">
          <h4>
            ⭑ Field(s) selected:
            <span
              class="selected-field-preview"
              v-for="(field, index) in selectedFields"
              :key="index"
              @click="removeField(field)"
            >
              {{ titleCased(field) }}
            </span>
          </h4>
        </div>

        <div class="basic">
          <select v-model="conditions">
            <option v-for="(value, key, index) in editsConditions" :key="index" :value="key">
              {{ value }}
            </option>
          </select>
        </div>
      </div>

    </div>

    <div id="controlArea">
      <button class="btn-basic">
        Update
      </button>
    </div>
  </div>

</template>
<script>
/* eslint-disable */
import { editsBasic, editsConditions } from '../data/editsData';
import formList from '../data/formList';

export default {
  name: 'EditsBasic',
  props: ['formFieldList', 'selectedProps', 'selectedEditsProps'],
  data() {
    return {
      selectedEdits: '',
      selectedForm: '',
      selectedField: '',
      conditions: '',
      selectedFields: [],
      editsBasicSelections: {},
      editsFormSelections: [],
      editsFieldSelections: [],
      editsConditions: {},
    };
  },
  created() {
    this.editsBasicSelections = editsBasic;
    this.editsFormSelections = formList.map(formObj => formObj.name);
    this.editsConditions = editsConditions;
  },
  updated() {
    // initial values of the dropdowns
    this.selectedEdits = this.selectedEditsProps.edits;
    this.selectedForm = this.selectedProps.form;
    this.editsFieldSelections = this.selectedProps.fieldsOfSelectedForm;
  },
  mounted() {
    // debugger
  },
  methods: {
    onSelectForm() {
      this.editsFieldSelections = this.getFieldNameArr(this.selectedForm);
    },
    getFieldNameArr(formName) {
      const fieldObj = this.formFieldList[formName];

      return Object.keys(fieldObj);
    },
    titleCased(nameString) {
      if (nameString === undefined) {
        return '';
      }

      return nameString.split('_').map((string) => {
        const lowerCasedString = string.toLowerCase();

        return (lowerCasedString[0] || '').toUpperCase() + lowerCasedString.slice(1);
      }).join(' ');
    },
    addField() {
      if (!this.selectedFields.includes(this.selectedField)) {
        this.selectedFields.push(this.selectedField);
      }
    },
    removeField(fieldToRemove) {
      this.selectedFields = this.selectedFields.filter(field => field !== fieldToRemove);
    },
  },
};
</script>
​
<style lang="scss" scoped>
 $basic: red;
  $main-blue: rgba(0, 87, 142, 1);
  $main-white: #F2F4F4;

  h3 {
    margin-right: 1rem;
    font-weight: 100;
    text-transform: uppercase;
    letter-spacing: 1px;
  }

  h4 {
    font-weight: 100;
    letter-spacing: 1px;
  }

  #editingArea {
    border: 1px solid $basic;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  #staticData {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin: 0 0 2rem 0;

    h3 {
      margin-top: 0.5rem;
    }
    .label_1 {
      font-size: 1rem;
      color: $main-blue;
      // border-bottom: 1px solid $main-blue;
      padding: 2px 5px;
      letter-spacing: 0.5px;
      text-transform: capitalize;
    }
  }
  ​
  #dynamicData {
    width: 100%;
    display: flex;
    flex-direction: column;
  }

  .group {
    border: 1px solid;
    margin-left: 1rem;
  }
  .basic {
    margin: 0.5rem;
    text-align: left;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    align-content: flex-start;
    // background-color: pink;
    padding: 0.5rem;
  }
  .render-selection {
    margin-left: 1rem;
  }
  .selected-field-preview {
    margin: 0 5px 0 0;
    background-color: $main-blue;
    color: $main-white;
    font-size: 0.8rem;
    padding: 2px 5px;
  }
  .selected-field-preview:hover {
    cursor: pointer;
  }
  select {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    letter-spacing: 1px;
    border: none;
    border-bottom: 1px solid $main-blue;
    outline: none;
    min-width: 300px;
  }

  .btn-basic {
    margin-left: 10px;
    margin-right: 0;
  }
</style>
