<template>
  <div id="app">
    <h1 class="app__title">Switch City</h1>
    <div v-if="isLoading">{{message}}</div>
    <div v-else-if="isError">{{message}}</div>

      <SwitchCities v-else
        class="app__wrapper"
        :moscow="time.moscow"
        :new_york="time.new_york"
      />

  </div>
</template>

<script>
import SwitchCities from './components/SwitchCities';
import axios from 'axios';
import moment from 'moment';
moment.locale('ru');
export default {
  name: 'App',
  components: {
    SwitchCities
  },
  data() {
    return {
      time: {
        moscow: '',
        new_york: '',
      },
      message: 'Loading cities data...',
      isError: false,
      isLoading: true
    }
  },
  created() {
      const moscow = () => axios.get('http://worldtimeapi.org/api/timezone/Europe/Moscow');
      const newYork = () => axios.get('http://worldtimeapi.org/api/timezone/America/New_York');
      axios.all([moscow(), newYork()])
              .then(([moscow, newYork]) => {

                const moscowDate = new Date(moscow.data.datetime);
                const newYorkDate = new Date(newYork.data.datetime);

                this.time.moscow = moment.utc(moscowDate).utcOffset(moscow.data.utc_offset).format('dddd Do MMMM, HH:mm:ss');
                this.time.new_york = moment.utc(newYorkDate).utcOffset(newYork.data.utc_offset).format('dddd Do MMMM, HH:mm:ss');
                this.isLoading = this.isError = false;
              })
              .catch(error => {
                this.isError = true;
                this.isLoading = false;
                this.message = `Сожалеем, данные не получены :( ${error}`;
                throw new Error('Данные не получены :(');
              })
  },

  mounted() {
  }
}
</script>

<style>
  #app {
    font-family: 'Montserrat', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .app__wrapper {
    text-align: center;
  }
</style>
