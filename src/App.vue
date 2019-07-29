<template>
  <div id="app">
    <Header
    @search-fields="onSearch"
    :formFieldList="formFieldList"
    ref="childHeader"
     />
    <edits-basic
    :formFieldList="formFieldList"
    :selectedProps="selectedProps"
    :selectedEditsProps="selectedEditsProps"
    />
  </div>
</template>

<script>
/* eslint-disable */
import EditsBasic from './components/EditsBasic.vue';
import Header from './components/Header.vue';
import formList from '../src/data/formList.js';
import fieldList from '../src/data/fieldList.js';
import configRules from '../src/data/configRules.js';
import editsChainById from '../src/data/editsChainById.js';

export default {
  name: 'app',
  components: {
    EditsBasic,
    Header,
  },
  data() {
    return {
      formFieldList: {},
      selectedProps: {},
      selectedEditsProps: {},
    };
  },
  created() {
    this.formFieldList = this.constructHeaderProps({
      formList, fieldList, configRules, editsChainById,
    });
  },
  methods: {
    onSearch(resultProps) {
      const { form, field } = resultProps;
      const editsProp = this.formFieldList[form][field];
      const fieldsOfSelectedForm = this.$refs.childHeader.getFieldNameArr(form);

      this.selectedProps = { ...resultProps, fieldsOfSelectedForm };
      this.selectedEditsProps = editsProp;
    },
    constructHeaderProps(props) {
      const {
        formList, fieldList, configRules, editsChainById,
      } = props;

      return configRules.reduce((resultObj, curConfigRule) => {
        const { formNameId, fieldNameId, editsChainId } = curConfigRule;
        const formName = formList.filter(formObj => formObj.id === formNameId)[0].name;
        const fieldName = fieldList.filter(fieldObj => fieldObj.id === fieldNameId)[0].name;
        const editsChainArr = editsChainById.filter(editsObj => editsObj.id === editsChainId)[0].nodeChain;

        const currentEdits = editsChainArr[0].nodeId;
        const hasForm = Object.prototype.hasOwnProperty.call(resultObj, formName);

        if (!hasForm) {
          resultObj[formName] = {};
        }

        resultObj[formName][fieldName] = {};
        resultObj[formName][fieldName].edits = currentEdits

      return resultObj;
      }, {});
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
  },
};
</script>

<style lang="scss">
$main-blue: #00578E;
$main-blue-light: #CCDDE8;
$main-blue-dark: #002C47;
$main-white: #F2F4F4;
$main-red: #D0021B;
$main-yellow: #FCDB24;

* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;
  border: 1px solid;
  display: flex;
  flex-direction: column;
}

.btn-basic {
  padding: 0.5rem 0.8rem;
  border: none;
  margin-right: 10px;
  min-width: 80px;
  background-color: whitesmoke;
  color: $main-blue;
  letter-spacing: 1px;
  text-transform: uppercase;
  outline: none;
  transition: 0.3s;
}

.btn-basic:hover {
  cursor: pointer;
  transform: translateY(2px);
}
​
.btn-basic:active {
  box-shadow: 1px 1px 5px white;
}

.btn-danger {
  background-color: $main-blue-dark;
  color: whitesmoke;
}

.btn-large {
  min-width: 200px;
}
</style>