<script setup lang="ts">
import { Bar } from "vue-chartjs";
import type { ChartData } from "chart.js"
import { computed, ref } from "vue";



type movie = { title: string; rating: number; }
const movies = ref<movie[]>([]);

const chartData = computed((): ChartData<"bar"> => {
    return {
        labels: movies.value.map(movie => movie.title),
        datasets: [
            {
                label: "IMDB",
                backgroundColor: ["#c82834"], 
                data: movies.value.map(movie => movie.rating),
                stack: "rating"
            },
            {
                label: "Kinopoisk",
                backgroundColor: [ "#244771"], 
                data: movies.value.map(movie => Number(movie.rating) + 1 ),
                stack: "rating"
            },
            {
                label: "Rotten Tomatoes",
                backgroundColor: [ "#fAfA30"], 
                data: movies.value.map(movie => movie.rating - 1 ),
                stack: "rating",
                borderColor: "red",
                borderWidth: 1,
            },

        ]
    }
})

fetch("/api.json")
    .then( async (res) => movies.value = await res.json())
    .catch((error) => alert(error.message));

</script>


<template>
    <Bar 
        v-if="movies.length"
        :data="chartData" 
        :options="{
            indexAxis: 'y'
        }"
    />
</template>

<style>
.body {
    padding: 50px
}
</style>