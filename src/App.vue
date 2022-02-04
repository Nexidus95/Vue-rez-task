<template>
 <div class="full">
 <h1>Reiz Tech Homework Assigment</h1>
  <div>
  <button v-on:click="SetOceania">Oceania</button>
  <button v-on:click="SetSmaller">Smaller than Lithuania</button>
  <button v-on:click="ResetFilter">Reset filter</button>
  <button class= "sort" v-on:click="Sorting">Sort</button>
   <table class= "stats">
    <tbody>
      <tr class="Block" v-for="country in countries" v-if="country.area != null" v-bind:key="country">
        <table style="table-layout: fixed" v-if=" country.region == 'Oceania' && okeanija && !smaller">
          <tr class="cell" >Name:  {{country.name}}</tr>
          <tr class="cell">Region:  {{country.region}}</tr>
          <tr class="area">Size:  {{country.area}}</tr>
        </table>
        <table style="table-layout: fixed" v-if=" !okeanija && smaller && country.area < lithuaniaS">
          <tr class="cell" >Name:  {{country.name}}</tr>
          <tr class="cell">Region:  {{country.region}}</tr>
          <tr class="area">Size:  {{country.area}}</tr>
        </table>
        <table style="table-layout: fixed" v-if=" !smaller && !okeanija">
          <tr class="cell">Name:  {{country.name}}</tr>
          <tr class="cell">Region:  {{country.region}}</tr>
          <tr class="area">Size:  {{country.area}}</tr>
        </table>
      </tr>
    </tbody>
   </table>
  </div>
 </div>
</template>

<script>
export default {
  data () {
    return {
      countries: [],
      okeanija: false,
      smaller: false,
      lithuaniaS: null
    }
  },
  methods: {
    GetData () {
      fetch('https://restcountries.com/v2/all?fields=name,region,area')
        .then(response => response.json())
        .then((data) => {
          this.countries = data
        })
    },
    GetLithuania () {
      for (var prop in this.countries) {
        if (this.countries[prop].name === 'Lithuania') {
          this.lithuaniaS = this.countries[prop].area
        }
      }
    },
    SetOceania () {
      this.okeanija = true
      this.smaller = false
    },
    SetSmaller () {
      this.GetLithuania()
      this.okeanija = false
      this.smaller = true
    },
    ResetFilter () {
      this.okeanija = false
      this.smaller = false
    },
    Sorting () {
      return this.countries.sort((a, b) => {
        return a.name.localeCompare(b.name)
      })
    }
  },
  mounted () {
    this.GetData()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  color: green;
}
.slot {
  color: white;
}
.stats {
  border-collapse: collapse;
  width: 100%;
  responsive: true;
}
th, td, tr {
  border: 1px solid black;
  width: 100%;
}
.full{
  background-color: PaleGreen;
}
.Block{
  background-color: LawnGreen;
  width: 100%;
}
.cell{
 background-color: LimeGreen;
 width: 100%;
}
.sort{
 float: right;
}
.area{
 width: 100%;
 background-color: LimeGreen;
}
</style>
