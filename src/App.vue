<template>
  <div id="app">
    <Header />
    <div class="container">
      <Search v-on:pc-location="getCrimes"/>
      <template v-if="isLoading">
        <Loading />
      </template>
      <transition name="fade">
      <template v-if="crimesFound">
        <table>
          <tr>
            <th>Crime</th>
            <th>Location</th>
            <th>Outcome</th>
            <th>Date</th>
          </tr>
          <tr v-for="(crime, i) in crimes" :key="i">
            <td>{{crime.category | formatCat}}</td>
            <td>{{crime.location.street.name}}</td>
            <td>{{crime.outcome_status ? crime.outcome_status.category : 'Unknown'}}</td>
            <td>{{crime.month | formatDate}}</td>
          </tr>
        </table>
      </template>
      </transition>
      <template v-if="noData">
        <h3>No data found</h3>
      </template>
    </div>
  </div>
</template>

<script>
import Header from './static/Header.vue';
import Loading from './static/Loading.vue';

import Search from './components/Search.vue';

export default {
  name: 'App',
  components: {
    Header,
    Loading,
    Search,
  },
  data() {
    return {
      isLoading: false,
      noData: false,
      crimesFound: false,
      crimes: [],
    };
  },
  methods: {
    getCrimes(location) {
      this.noData = false;
      this.crimesFound = false;
      const {
        lat,
        lng,
        month,
        year,
      } = location;
      this.isLoading = true;
      fetch(`https://data.police.uk/api/crimes-at-location?date=${year}-${month}&lat=${lat}&lng=${lng}`)
        .then((res) => res.json())
        .then((data) => {
          setTimeout(() => {
            if (data.length > 0) {
              this.crimesFound = true;
              this.crimes = data;
            } else {
              this.noData = true;
            }
            this.isLoading = false;
          }, 2000);
        });
    },
  },
  filters: {
    formatCat(val) {
      return val.split('-').join(' ');
    },
    formatDate(val) {
      return val.split('-').reverse().join('/');
    },
  },
};
</script>

<style lang="scss">
$primary: #323D69;


@import url('https://fonts.googleapis.com/css?family=Poppins&display=swap');
* {
  box-sizing: border-box;
  margin:0;
  padding: 0;
}
body {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  font-family: 'Poppins', sans-serif;
  background: #f4f4f4;
}
#app {
  width:100vw;
  display: flex;
  flex-direction: column;
  align-items: center
}
.container  {
  padding:15px;
  margin:30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width:100%;
  max-width: 1280px;
  background: #fff;
  transition: all .5s;
}

table {
  width:100%;
  th {
    background: $primary;
    color:#fff;
    padding: 5px 7px;
  }
  td {
    padding: 5px 7px;
    &:nth-child(1) {
      text-transform: capitalize;
    }
  }
  tr:nth-child(odd) {
    background: #f4f4f4;
  }
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
