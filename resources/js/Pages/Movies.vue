<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head } from "@inertiajs/vue3";
import { onMounted, ref, computed } from "vue";
const API_KEY = import.meta.env.VITE_API_KEY;
const activeTab = ref("daily");
const showAll = ref(false);

const movieData = {
    daily: ref([]),
    weekly: ref([]),
    most: ref([]),
    top: ref([]),
    upcoming: ref([]),
    discover: ref([]),
};

onMounted(async () => {
    const endpoints = {
        daily: `https://api.themoviedb.org/3/trending/movie/day?api_key=${API_KEY}`,
        weekly: `https://api.themoviedb.org/3/trending/movie/week?api_key=${API_KEY}`,
        most: `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}`,
        top: `https://api.themoviedb.org/3/movie/top_rated?api_key=${API_KEY}`,
        upcoming: `https://api.themoviedb.org/3/movie/upcoming?api_key=${API_KEY}`,
        discover: `https://api.themoviedb.org/3/discover/movie?api_key=${API_KEY}`,
    };

    for (const [key, url] of Object.entries(endpoints)) {
        const res = await fetch(url);
        const data = await res.json();
        movieData[key].value = data.results;
    }
});

const displayedMovies = computed(() => {
    if (showAll.value) {
        const combined = Object.values(movieData).flatMap(
            (refVal) => refVal.value
        );
        const unique = Array.from(
            new Map(combined.map((movie) => [movie.id, movie])).values()
        );
        return unique;
    }
    return movieData[activeTab.value]?.value ?? [];
});
</script>

<template>
    <AuthenticatedLayout>
        <!-- Tab & See All Control -->
        <div
            class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex items-center justify-between"
        >
            <div class="flex gap-3 flex-wrap">
                <button
                    v-for="tab in Object.keys(movieData)"
                    :key="tab"
                    @click="
                        () => {
                            activeTab = tab;
                            showAll = false;
                        }
                    "
                    :class="[
                        'px-4 py-1 text-sm md:text-base rounded-full font-semibold transition',
                        activeTab === tab && !showAll
                            ? 'text-white bg-emerald-700 p-2'
                            : 'text-white hover:bg-gray-600',
                    ]"
                >
                    {{ tab.charAt(0).toUpperCase() + tab.slice(1) }}
                </button>
            </div>
            <button
                @click="showAll = true"
                :class="[
                    'px-4 py-1 text-sm md:text-base rounded-full font-semibold transition',
                    showAll
                        ? 'text-white bg-emerald-700 p-2'
                        : 'text-white hover:bg-gray-600',
                ]"
            >
                See All
            </button>
        </div>

        <!-- Movie Grid -->
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div
                class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-4"
            >
                <div
                    v-for="movie in displayedMovies"
                    :key="movie.id"
                    class="shadow text-center flex flex-col justify-between"
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
