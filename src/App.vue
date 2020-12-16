<template>
  <div id="app">
    <h1>$ SPNDR APP $</h1>
    <hr />
    <AddSpend 
      v-bind:incomeSubTypes="incomeSubTypes"
      v-bind:outcomeSubTypes="outcomeSubTypes"
      v-on:add-spend="addSpend"
     />
    <h2>Траты:</h2>
    <select v-model="type" >
      <option value="all" >все</option>
      <option value="HIGH_LEVEL_TYPE_COSTS" >расходы</option>
      <option value="HIGH_LEVEL_TYPE_INCOME" >прибыль</option>
    </select>
    <SpendList 
      v-bind:spends="typeFilteredSpends"
      v-if="typeFilteredSpends.length" 
      v-on:rm-spend="rmSpend"
     />
     <p v-else >No spends yet!</p>
  </div>
</template>

<script>
import SpendList from '@/components/SpendList';
import AddSpend from '@/components/AddSpend';
import arrayHelpers from '@/Helprers/helpers.js';

export default {
  name: 'App',
  components: {
    SpendList, AddSpend
  },
  data() {
    return {
      type: "all",
      spends: [],
      incomeSubTypes: [],
      outcomeSubTypes: [],
    }
  },
  mounted() {
    fetch("https://localhost:44378/Spend/GetStartingData")
    .then(response => {
        if (response.status === 200) {
          response.json().then(json => {
            this.spends = json.data.Data;
            this.incomeSubTypes = json.incomeSubTypes;
            this.outcomeSubTypes = json.outcomeSubTypes;
          })
        }
      })
  },
  computed: {
    groupedSpends() {
      return arrayHelpers.groupBy(spends, "date");
    },
    typeFilteredSpends() {
      if (this.type === "all") {
        return this.spends;
      }
      else if (this.type === "HIGH_LEVEL_TYPE_INCOME") {
        return this.spends.filter(x => x.highType === "HIGH_LEVEL_TYPE_INCOME");
      }
      else if (this.type === "HIGH_LEVEL_TYPE_COSTS") {
        return this.spends.filter(x => x.highType === "HIGH_LEVEL_TYPE_COSTS");
      }

      return this.spends;
    }
  },
  methods: {
    rmSpend(id) {
      fetch("https://localhost:44378/Spend/DeleteSpend/" + id, {
        method: 'DELETE'
      }).then(response => {
        if (response.status === 200) {
          response.json().then(json => {
            if (json.success === true) {
                this.spends = this.spends.filter(x => x.id !== id);
            }
          })
        } 
      })
    },
    addSpend(newItem) {
      console.log(itemToSent)
      fetch("https://localhost:44378/Spend/AddSpend", {
        method: 'POST',
        body: JSON.stringify(newItem),
        headers: {
          "Accept": "application/json, text/plain, */*",
          "Content-Type": "application/json"
        }
      }).then(response => {
        if (response.status === 200) {
          response.json().then(json => {
            if (json.success === true) {
                newItem.id = json.data.id;
                this.spends.push(newItem);
            }
          })
        }
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
