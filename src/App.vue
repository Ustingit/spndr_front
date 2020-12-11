<template>
  <div id="app">
    <h1>$ SPNDR APP $</h1>
    <hr />
    <AddSpend 
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
      spends: []
    }
  },
  mounted() {
    fetch("https://localhost:44378/Spend/GetItems")
    .then(response => {
        if (response.status === 200) {
          response.json().then(json => { console.log(json.Data); this.spends = json.Data;})
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
      this.spends = this.spends.filter(x => x.id !== id);
    },
    addSpend(newItem) {
      const maxId = this.spends.sort((a,b) => b['id'] - a['id'])[0]['id'] + 1;
      newItem.id = maxId;

      this.spends.push(newItem);
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
