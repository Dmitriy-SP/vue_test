<script>
export default {
  data() {
    return {
      city: "",
      error: "",
    };
  },
  computed: {
    cityName() {
      return `"${this.city}"`;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более 1 символа";
        return false;
      }
      this.error = "";
      return true;
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>{{ city ? `Узнать погоду в ${cityName}` : "Введите город" }}</p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <button v-if="city" v-on:click="getWeather()">Узнать погоду</button>
    <button disabled v-else>Введите город</button>
    <p class="error">{{ error }}</p>
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1F1F1F;
  text-align: center;
  color: #FD850A;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #FD850A;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  background: #5b2915;
  color: #F7EFED;
  border-radius: 10px;
  border: 2px solid #DB6B3D;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
}

.wrapper button:hover:enabled {
  transition: transform 500ms ease;
  transform: scale(1.1) translateY(-5px);
}

.wrapper button:disabled {
  background: #434343;
  cursor: not-allowed;
}
</style>
