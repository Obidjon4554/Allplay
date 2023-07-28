<template>
    <header class="fixed w-full  min-height-[100vh] flex items-center justify-center px-20 py-8 bg-black z-50">
        <div class="container flex items-center gap-72  justify-between">
            <div class="logo flex gap-8 mx-auto items-center text-2xl">

                <i class="fas fa-bars cursor-pointer"></i>

                <img src="../assets/images/logo.svg" alt="" class="cursor-pointer">
            </div>
            <ul class="flex gap-8 text-xl text-gray-400 ">
                <li class="hover:text-white cursor-pointer" v-for="(item, idx) in menuArray" :key="item"
                    @click="selectCategory(item.title)"
                    :class="selectedCategory === item.title ? 'text-white' : 'text-gray-400'">
                    <router-link :to="item.route">
                        {{ item.title }}
                    </router-link>
                </li>
                <li class="hover:text-white cursor-pointer">
                    <a href="https://allplay.uz/search">
                        <i class="fas fa-search"></i>
                    </a>
                </li>
            </ul>
            <div class="right flex  gap-4 items-center">
                <ul class="flex gap-2 text-sm text-gray-400 uppercase ">
                    <li class="text-brand-color hover:underline cursor-pointer"> Ру </li>
                    <li class="hover:underline cursor-pointer"><a href="https://allplay.uz/uz/tv">O'z</a></li>
                    <li class="hover:underline cursor-pointer"><a href="https://allplay.uz/en/tv">En</a></li>
                </ul>
                <h1 class="text-lg text-gray-400 hover:text-white cursor-pointer">
                    <a href="https://allplay.uz/login/">
                        Войти
                    </a>
                </h1>
            </div>
        </div>
    </header>
</template>

<script>
export default {
    data() {
        return {
            selectedCategory: 'all',
            menuArray: [
                {
                    title: 'ТВ',
                    route: '/'
                },
                {
                    title: 'Кинотеатр',
                    route: 'movies'
                },
                {
                    title: 'GО',
                    route: '#'
                },
                {
                    title: 'Радио',
                    route: '#'
                },
                {
                    title: 'Я смотрю',
                    route: '#'
                }
            ]
        }
    },
    methods: {
        selectCategory(category) {
            this.selectedCategory = category;
        },
    }
}
</script>