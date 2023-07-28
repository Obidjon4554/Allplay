<template>
    <div class="bottom   py-20 w-full flex flex-col gap-2">
        <h1 class="title text-4xl pb-4 ">Новинки </h1>
        <div class="wrapper min-height-[100vh]  items-center justify-center w-full  flex flex-wrap gap-2 transition-all ">
            <div class="card w-[219px] h-[300px]   transition-all hover:-translate-y-2 hover:shadow-md hover:shadow-gray-500 rounded-sm"
                v-for="movie in listMovie" :key="movie.id">
                <a :href="movie.url">
                    <div class="card-image" :style="{ backgroundImage: 'url(' + movie.poster.url_340x450 + ')', }">
                        <div class="content text-white justify-center items-center flex flex-col mx-auto gap-3 py-8 ">
                            <h3 class="subtitle mt-44 text-base font-normal text-amber-300 ">Подписка</h3>
                            <h1 class="title text-xl font-bold text-center mx-auto">{{ movie.title }}</h1>
                        </div>
                    </div>
                </a>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        listMovie: {
            type: Array,
            required: true,
        },
    },

}
</script>

<style>
.card-image {
    @apply bg-cover bg-center w-full h-full rounded-sm;
}



</style>