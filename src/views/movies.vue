<template>
  <section class="py-16">
    <div class="container p-20">
      <div class="flex gap-16  text-xl text-gray-400 transition-all ">
        <swiper :modules="modules" :breakpoints="{
          320: {
            slidesPerView: 2,
            spaceBetween: 10
          },
          425: {
            slidesPerView: 3,
            spaceBetween: 10
          },
          650: {
            slidesPerView: 4
          },
          1000: {
            slidesPerView: 5
          },
          1255: { slidesPerView: 6 },
          1300: { slidesPerView: 9 }
        }" :navigation="{
  nextEl: '.next',
  prevEl: '.prev',
}" :loop="true" :slidesPerView="9" class="swiper-wrap my-5 flex">
          <swiper-slide class="flex mx-3 pb-5 hover:text-white cursor-pointer" v-for="item in menuArray" :key="item"
            @click="selectCategory(item.title)"
            :class="selectedCategory === item.title ? 'text-white underline' : 'text-gray-400'">
            <li class="text-center">
              {{ item.title }}
            </li>
          </swiper-slide>
        </swiper>
        <div class="arrows flex text-white gap-6 text-2xl">
          <button class="btn prev absolute left-8 bottom-[546px]"><i class="fas fa-chevron-left"></i></button>
          <button class="btn next"><i class="fas fa-chevron-right"></i></button>
        </div>
      </div>
      <div class="carousel">
        <swiper :slidesPerView="1" :spaceBetween="30" :effect="'fade'" :modules="modules" class="swiper-wrap" :autoplay="{
          delay: 3000,
          disableOnInteraction: false,

        }" :navigation="true" :style="{
  '--swiper-navigation-color': '#fff',
  '--swiper-pagination-color': '#fff',
}">
          <swiper-slide :listMovie="filteredMovies" class="swiper" v-for="(movie, index) in listMovie" :key="index"
            :style="{ backgroundImage: `url(${movie.published_bg.url})` }">
            <div class="container flex flex-col gap-2">
              <h1 class="title text-4xl">{{ movie.title }}</h1>
              <h3 class="subtitle w-[630px] text-xl text-gray-400 border-b border-gray-600 pb-4">
                allplay 8.7 • 7.8 • IMDb 7.6 • 2022, боевик, приключения • США • 12+
              </h3>
              <p class="text py-4 text-lg w-[400px]">{{ truncateDescription(movie.description, 200) }}</p>
              <div class="btns flex gap-2 pt-10">
                <button class="btn bg-brand-color px-10 text-xl py-4"> <a :href="movie.url">► Смотреть фильм</a>
                </button>
                <button class="btn px-10 text-xl py-4 bg-stone-700"> <a :href="movie.url">О фильме</a>
                </button>
              </div>
            </div>
          </swiper-slide>
        </swiper>
      </div>
      <filmsList :listMovie="filteredMovies" />
    </div>
  </section>
</template>

<script>
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/navigation';
import { Navigation } from 'swiper/modules';

import filmsList from '../components/filmsList.vue';
export default {
  components: {
    Swiper,
    SwiperSlide,
    filmsList
  },
  data() {
    return {
      modules: [Navigation],
      apiList: [
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=1',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=2',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=3',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=4',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=5',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=6',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=7',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=8',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=9',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=10',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=11',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=12',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=13',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=14',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=15',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=16',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=17',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=18',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=19',
        'https://api.allplay.uz/api/v1/movies?per_page=12&category=movies&page=20',
      ],
      listMovie: [],
      menuArray: [
        {
          title: 'Фильмы',
          route: '#'
        },
        {
          title: 'Сериалы',

        },
        {
          title: 'Мультики',

        },
        {
          title: 'Аниме',

        },
        {
          title: 'AMEDIATEKA',

        },
        {
          title: 'MORE.TV',

        },
        {
          title: 'START',

        },
        {
          title: '►PREMIER',

        },
        {
          title: 'MEGOGO',

        },
        {
          title: 'Фильмы',

        },
        {
          title: 'Сериалы',

        },
        {
          title: 'Мультики',

        },
        {
          title: 'Аниме',

        },
        {
          title: 'AMEDIATEKA',

        },
        {
          title: 'more.tv',

        },
        {
          title: 'START',

        },
        {
          title: '►PREMIER',

        },
        {
          title: 'MEGOGO',

        },
        {
          title: 'Фильмы',

        },
        {
          title: 'Сериалы',

        },
        {
          title: 'Мультики',

        },
        {
          title: 'Аниме',

        },
        {
          title: 'AMEDIATEKA',

        },
        {
          title: 'more.tv',

        },
        {
          title: 'START',

        },
        {
          title: '►PREMIER',

        },
        {
          title: 'MEGOGO',

        },
        {
          title: 'Фильмы',

        },
        {
          title: 'Сериалы',

        },
        {
          title: 'Мультики',

        },
        {
          title: 'Аниме',

        },
        {
          title: 'AMEDIATEKA',

        },
        {
          title: 'more.tv',

        },
        {
          title: 'START',

        },
        {
          title: '►PREMIER',

        },
        {
          title: 'MEGOGO',

        },
        {
          title: 'Фильмы',

        },
        {
          title: 'Сериалы',

        },
        {
          title: 'Мультики',

        },
        {
          title: 'Аниме',

        },
        {
          title: 'AMEDIATEKA',

        },
        {
          title: 'more.tv',

        },
        {
          title: 'START',

        },
        {
          title: '►PREMIER',

        },
        {
          title: 'MEGOGO',

        },
      ],
      currentPage: 1,
      totalPages: 20,
      selectedCategory: 'Фильмы',
      categories: ['https://api.allplay.uz/api/v1/categories'],
      filteredMovies: [],
    }
  },
  methods: {
    async fetchFeaturedMovies() {
      try {
        const response = await fetch('https://api.allplay.uz/api/v1/featured?category=movies');
        const data = await response.json();
        return data;
      } catch (error) {
        console.error('Error fetching featured movies:', error);
        return null;
      }
    },

    async movieList() {
      try {
        while (this.currentPage <= this.totalPages) {
          const response = await fetch(this.apiList[this.currentPage - 1]);
          const data = await response.json();
          this.listMovie = this.listMovie.concat(data.data);
          this.currentPage++;
        }
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    truncateDescription(description, maxLength) {
      if (description.length > maxLength) {
        return description.substring(0, maxLength) + '...';
      } else {
        return description;
      }
    },
    filterMoviesByCategory() {
      if (this.selectedCategory === 'Фильмы') {
        return this.listMovie;
      } else {
        return this.listMovie.filter((movie) => {
          const categoryMatches =
            movie.categories &&
            Array.isArray(movie.categories) &&
            movie.categories.some((cat) => cat.name === this.selectedCategory);
          return categoryMatches;
        });
      }
    },
    selectCategory(category) {
      this.selectedCategory = category;
      this.filteredMovies = this.filterMoviesByCategory();
    },
  },
  computed: {


  },
  created() {
    this.movieList().then(() => {
      this.selectedCategory = this.menuArray[0].title;
      this.filteredMovies = this.filterMoviesByCategory();
    });
  },
  mounted() {
    this.fetchFeaturedMovies().then((data) => {
      if (data && Array.isArray(data.data) && data.data.length > 0) {
        this.listMovie = [...this.listMovie, ...data.data];
      }
    });
    this.movieList();
  },
}
</script>

<style>
.swiper-button-lock {
  display: block;
}

.swiper-wrap .swiper {
  display: flex;
  align-items: flex-start;
  justify-content: center;
  flex-direction: column;
  color: white;
  padding: 50px;
  height: 70vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>