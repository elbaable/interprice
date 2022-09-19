<template>
  <div class="main">
    <div class="max-w-screen-xl m-auto">
      <h2 class="text-3xl font-bold py-10">InterPrice Technologies Developer Test</h2>
      <div class="flex items-center">
        <div class="inline-flex shadow-md hover:shadow-lg focus:shadow-lg mr-4" role="group">
          <button @click="currency='USD'" class="rounded-l border-2 border-blue-600 inline-block px-4 py-2.5 text-blue-600 font-medium text-xs leading-tight uppercase active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: currency == 'USD'}">USD</button>
          <button @click="currency='EUR'" class="inline-block px-4 border-t-2 border-b-2 border-blue-600 py-2.5 text-blue-600 font-medium text-xs leading-tight uppercase active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: currency == 'EUR'}">EUR</button>
          <button @click="currency='CAD'" class="rounded-r inline-block px-4 border-2 border-blue-600 py-2.5 text-blue-600 font-medium text-xs leading-tight uppercase active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: currency == 'CAD'}">CAD</button>
        </div>
        <div class="inline-flex shadow-md hover:shadow-lg focus:shadow-lg mr-4" role="group">
          <button @click="yearFilter(5)" class="rounded-l border-2 border-blue-600 inline-block px-4 py-2.5 text-blue-600 font-medium text-xs leading-tight uppercase active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: years.includes(5)}">5 YRS</button>
          <button @click="yearFilter(10)" class="inline-block px-4 border-t-2 border-b-2 border-blue-600 py-2.5 text-blue-600 font-medium text-xs leading-tight uppercase active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: years.includes(10)}">10 YRS</button>
          <button @click="yearFilter(40)" class="rounded-r inline-block px-4 border-2 border-blue-600 py-2.5 text-blue-600 font-medium text-xs leading-tight uppercase active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: years.includes(40)}">40 YRS</button>
        </div>
        <div class="inline-flex shadow-md hover:shadow-lg focus:shadow-lg" role="group">
          <button @click="field='Spread'" class="rounded-l border-2 border-blue-600 inline-block px-4 py-2.5 text-blue-600 font-medium text-xs leading-tight active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: field == 'Spread'}">Spread</button>
          <button @click="field='Yield'" class="inline-block px-4 border-t-2 border-b-2 border-blue-600 py-2.5 text-blue-600 font-medium text-xs leading-tight active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: field == 'Yield'}">Yield</button>
          <button @click="field='3MLSpread'" class="rounded-r inline-block px-4 border-2 border-blue-600 py-2.5 text-blue-600 font-medium text-xs leading-tight active:bg-blue-800 active:text-white transition duration-150 ease-in-out" :class="{active: field == '3MLSpread'}">3MLSpread</button>
        </div>
      </div>
      <input v-model="filter" placeholder="Filter by company name" class="my-5 px-2 py-1 border-2 border-black-600 rounded-l">
      <Table :data=[...filteredData]
            :opened=[...opened] 
            :years=[...years] 
            :currency=currency 
            :toggle=toggle
            :field=field />
    </div>
  </div>
</template>

<script>
import date from "../assets/data.json";
import '@mdi/font/css/materialdesignicons.css'
import Table from "./Table.vue";

export default {
  name: "DevTest",
  props: {
    msg: String
  },
  created() {
      var filterData = date["Items"].filter(item => item.Company.toLowerCase().includes(this.filter.toLowerCase()))
        .map((item, index) => {
          var data = {
            id: index,
            name: item.Company,
            date: item.DateSent,
            preferred: item.Preferred,
            quote: item.Quote?.filter(e => e.Currency == this.currency)
          };
          return data;
        });
        console.log(123)
      this.filteredData = filterData;
  },
  data() {
    return {
      filter: "",
      currency: "USD",
      years: [5, 10, 40],
      opened: [],
      field: "Spread",
      filteredData: []
    };
  },
  watch: {
    filter: function() {
      var filterData = date["Items"].filter(item => item.Company.toLowerCase().includes(this.filter.toLowerCase()))
        .map((item, index) => {
          var data = {
            id: index,
            name: item.Company,
            date: item.DateSent,
            preferred: item.Preferred,
            quote: item.Quote?.filter(e => e.Currency == this.currency)
          };
          return data;
        });
        console.log(filterData)
      this.filteredData = filterData;
    }
  },
  methods: {
    toggle(id) {
      const index = this.opened.indexOf(id);
      if (index > -1) {
        this.opened.splice(index, 1);
      }
      else {
        this.opened.push(id);
      }
    },
    yearFilter(year) {
      const index = this.years.indexOf(year);
      if (index > -1) {
        this.years.splice(index, 1);
      }
      else {
        this.years.push(year);
      }
    }
  },
  components: { Table }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.opened {
  cursor: pointer;
}

.active {
  color: white;
  background-color: rgb(29 78 216);
}
</style>
