<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head } from "@inertiajs/vue3";
import { onMounted, ref } from "vue";
const trendingMovies = ref([]);
const mostWatchedMovies = ref([]);
const API_KEY = import.meta.env.VITE_API_KEY;

onMounted(async () => {
    const res1 = await fetch(
        `https://api.themoviedb.org/3/trending/movie/day?api_key=${API_KEY}`
    );
    const data1 = await res1.json();
    trendingMovies.value = data1.results;

    const res2 = await fetch(
        `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`
    );
    const data2 = await res2.json();
    mostWatchedMovies.value = data2.results;
});
</script>

<template>
    <AuthenticatedLayout>
        <!-- Section: Daily Trending -->
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="py-8">
                <h2 class="text-2xl font-bold text-white">Daily Trending</h2>
            </div>
            <div
                class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-4 items-stretch"
            >
                <div
                    v-for="movie in trendingMovies"
                    :key="movie.id"
                    class="flex flex-col justify-between h-full text-center bg-gray-800 rounded shadow p-3"
                >
                    <div>
                        <img
                            :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
                            :alt="movie.title"
                            class="mx-auto h-60 object-cover rounded"
                        />
                        <p class="mt-3 font-bold text-white">
                            {{ movie.title }}
                        </p>
                    </div>

                    <div>
                        <p class="mt-auto text-sm text-white opacity-70">
                            {{ movie.release_date }}
                        </p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Section: Most Watch -->
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="py-8">
                <h2 class="text-2xl font-bold text-white">Most Watched</h2>
            </div>
            <div
                class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-4 items-stretch"
            >
                <div
                    v-for="movie in mostWatchedMovies"
                    :key="movie.id"
                    class="flex flex-col justify-between h-full text-center bg-gray-800 rounded shadow p-3"
                >
                    <div>
                        <img
                            :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
                            :alt="movie.title"
                            class="mx-auto h-60 object-cover rounded"
                        />
                        <p class="mt-3 font-bold text-white">
                            {{ movie.title }}
                        </p>
                    </div>

                    <div>
                        <p class="mt-auto text-sm text-white opacity-70">
                            {{ movie.release_date }}
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
