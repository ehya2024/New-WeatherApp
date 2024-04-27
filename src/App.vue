<template>
    <main>
        <input type="text" class="input" v-model.trim="query" @keypress="weatherData">
        <div v-if="typeof weather.main != 'undefined'">
            <span>{{ weather.name }}</span>
            <span>{{ weatherCountry.country }}</span>
            <span>{{ Math.floor(weatherMain.temp) - 273 }} C</span>
            <span>{{ date.toLocaleDateString() }}</span>
        </div>
        <div v-else>
            <span>City</span>
            <span>Country</span>
            <span>Temp C</span>
            <span>{{ date.toLocaleDateString() }}</span>
        </div>
    </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const query = ref("");
const api = ref("https://api.openweathermap.org/data/2.5/");
const api_key = ref("7f9f88ae5fa257adca4f2e57807ef557");
const weather = ref("");
const weatherCountry = ref("");
const weatherMain = ref("");
const date = new Date();

function weatherData(e) {
    if (e.key === "Enter") {
        axios.get(`${api.value}weather?q=${query.value}&appid=${api_key.value}`)
            .then(function (response) {
                weather.value = response.data;
                weatherCountry.value = response.data.sys;
                weatherMain.value = response.data.main;
                console.log(response.data);
            })
            .catch(function (error) {
                console.log(error);
            })
    }
}
</script>

<style>
main {
    width: 20rem;
    height: 15rem;
    background: linear-gradient(45deg, skyblue, rgb(88, 171, 204));
    border-radius: 2px;
    margin: 7rem auto;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

div {
    display: flex;
    flex-direction: column;
    gap: 0.5rem 0;
    justify-content: center;
    align-items: center;
}

.input {
    border: none;
    background-color: rgb(87, 87, 87);
    color: gainsboro;
    outline: none;
    padding: 8px;
    box-sizing: border-box;
    border-radius: 2px;
    margin-bottom: 0.75rem;
}

span {
    font-size: 17px;
    color: rgb(66, 66, 66);
    cursor: default;
}

span:nth-of-type(3) {
    font-weight: bold;
    background-color: rgb(87, 87, 87);
    color: gainsboro;
    padding: 4px 6px;
    box-sizing: border-box;
    border-radius: 2px;
}
</style>