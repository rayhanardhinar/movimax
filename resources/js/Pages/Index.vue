<script setup>
import { Head, Link } from "@inertiajs/vue3";

defineProps({
    canLogin: {
        type: Boolean,
    },
    canRegister: {
        type: Boolean,
    },
    laravelVersion: {
        type: String,
        required: true,
    },
    phpVersion: {
        type: String,
        required: true,
    },
});
</script>

<template>
    <div class="min-h-screen flex flex-col dark:bg-gray-900">
        <div class="w-full border-b border-blue-800 shadow">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between">
                    <div>
                        <a href="http://www.themoviedb.org">
                            <img
                                src="https://www.themoviedb.org/assets/2/v4/logos/v2/blue_square_1-5bdc75aaebeb75dc7ae79426ddd9be3b2be1e342510f8202baf6bffa71d7f5c4.svg"
                                alt="logo"
                                class="w-20 h-16"
                            />
                        </a>
                    </div>

                    <div
                        v-if="canLogin"
                        class="flex justify-center items-center"
                    >
                        <Link
                            v-if="$page.props.auth.user"
                            :href="route('movies')"
                            class="font-semibold text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white focus:outline focus:outline-2 focus:rounded-sm focus:outline-red-500"
                            >Dashboard</Link
                        >

                        <template v-else>
                            <Link
                                :href="route('login')"
                                class="font-semibold text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white focus:outline focus:outline-2 focus:rounded-sm focus:outline-red-500"
                                >Log in</Link
                            >

                            <Link
                                v-if="canRegister"
                                :href="route('register')"
                                class="ms-4 font-semibold text-gray-600 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white focus:outline focus:outline-2 focus:rounded-sm focus:outline-red-500"
                                >Register</Link
                            >
                        </template>
                    </div>
                </div>
            </div>
        </div>

        <div
            class="flex-1 flex items-center justify-center bg-gradient-to-br from-gray-900 to-blue-900 text-white"
        >
            <div class="max-w-4xl mx-auto text-center px-6 py-20">
                <h1 class="text-7xl font-bold mb-6 animate-fade-in">
                    Welcome to <span class="text-blue-400">Movimax</span>
                </h1>
                <p class="text-xl mb-8 leading-relaxed">
                    Your ultimate gateway to the world of cinema! Explore
                    thousands of movies from The Movie Database (TMDB), discover
                    trending films, and create your personal watchlist. From
                    blockbusters to hidden gems, Movimax brings the magic of
                    movies right to your screen.
                </p>
                <div class="space-x-4">
                    <a
                        href="/movies"
                        class="bg-blue-600 hover:bg-blue-700 px-6 py-3 rounded-lg font-medium transition duration-300 inline-block"
                    >
                        Explore Movies
                    </a>
                </div>
            </div>
        </div>
    </div>
</template>
