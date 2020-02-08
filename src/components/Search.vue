<template>
  <div class="search">
    <transition name="fade">
    <div v-if="error" class="error">
      {{errorMsg}}
    </div>
    </transition>
    <form @submit.prevent="searchPostcode" class="form" >
      <input :class="{invalid: error}"
      type="text" placeholder="Enter a postcode"
      autofocus="autofocus"
      v-model="postcode">
      <select v-model="month">
        <option value="01">January</option>
        <option value="02">February</option>
        <option value="03">March</option>
        <option value="04">April</option>
        <option value="05">May</option>
        <option value="06">June</option>
        <option value="07">July</option>
        <option value="08">August</option>
        <option value="09">October</option>
        <option value="10">September</option>
        <option value="11">November</option>
        <option value="12">December</option>
      </select>
      <select v-model="year">
        <option selected value="2019">2019</option>
        <option value="2018">2018</option>
        <option value="2017">2017</option>
      </select>
      <input type="submit" value="Get Crimes">
    </form>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      error: false,
      errorMsg: '',
      postcode: '',
      year: 2019,
      month: '01',
    };
  },
  methods: {
    searchPostcode() {
      this.error = false;
      this.errorMsg = '';
      fetch(`https://postcodes.io/postcodes/${this.postcode}`)
        .then((res) => res.json())
        .then((data) => {
          if (data.status === 400) {
            this.error = true;
            this.errorMsg = 'Please Enter a Postcode';
          } else if (data.status === 404) {
            this.error = true;
            this.errorMsg = data.error;
          } else if (data.status === 200) {
            this.$emit('pc-location', {
              lat: data.result.latitude,
              lng: data.result.longitude,
              month: this.month,
              year: this.year,
            });
          }
        });
    },
  },
};
</script>

<style lang="scss" scoped>
$primary: #323D69;
.search {
  width:100%;
}
.form {
  display: flex;
  margin: 25px 0;
  width:100%;

  @media(max-width: 600px) {
    flex-wrap: wrap;
  }

  input[type="text"] {
    padding:7px;
    font-size: 1em;
    flex:1 1 auto;
    -webkit-appearance: none;
    @media(max-width: 600px) {
    width:100%;
    margin-bottom: 15px;
    -webkit-appearance: none;
    }
  }
  input[type="submit"] {
    padding:7px;
    font-size: 1em;
    flex:0 1 auto;
    border:none;
    background: $primary;
    color:#fff;
    border-radius: 5px;
    border:1px solid $primary;
    cursor: pointer;
    -webkit-appearance: none;
    @media(max-width: 600px) {
    width:100%;
    margin-bottom: 15px;
    -webkit-appearance: none;
    }
  }
  select {
    padding:7px;
    font-size: 1em;
    border-radius: none;
    background: $primary;
    border:none;
    color:#fff;
    margin: 0 7px;
    border-radius: 5px;
    -webkit-appearance: none;
    @media(max-width: 600px) {
    width:100%;
    margin-bottom: 15px;
    margin-left: 0;
    margin-right: 0;
    -webkit-appearance: none;
    }
  }
}
.error {
  background:#DE4C31;
  color:#fff;
  padding:12px;
  border-radius: 5px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.invalid {
  border-color: #DE4C31;
  border-radius: 5px;
}
</style>
