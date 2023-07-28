<template>
  <section class="p-20 w-full min-height-[100vh] flex items-center justify-center">
    <div class="container">

      <div class="flex gap-8 pt-20 text-xl text-gray-400 transition-all ">
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
          <button class="btn prev text-white gap-6 text-2xl relative  bottom-20"><i class="fas fa-chevron-left"></i></button>
          <swiper-slide class="flex mx-3 pb-5 hover:text-white cursor-pointer" v-for="item in menuArray" :key="item"
            @click="selectCategory(item.title)"
            :class="selectedCategory === item.title ? 'text-white underline' : 'text-gray-400'">
            <li class="text-center">
              {{ item.title }}
            </li>
          </swiper-slide>
        </swiper>
          <button class="btn next text-white gap-6 text-2xl relative  bottom-7"><i class="fas fa-chevron-right"></i></button>
      </div>
      <div v-if="isLoading" class="loader-container">
        <Loader />
      </div>
      <div v-else class="carousel">
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
      <div v-if="isLoading" class="loader-container">
  <Loader />
</div>
<div v-else class="pagination mt-5 justify-center ">
  <ul class="flex items-center relative left-[500px]">
    <li @click="goToPreviousPage(page)"
      class="flex items-center justify-center cursor-pointer px-3 h-8 ml-0 leading-tight bg-brand-color border-r-2 border-black">
      <button class="btn">Previous</button>
    </li>
    <li @click="goToPage(page)"
      class="flex items-center justify-center cursor-pointer px-3 h-8 ml-0 leading-tight border border-brand-color"
      v-for="page in visiblePages" :key="page" :class="{ 'bg-brand-color': isPageActive(page) }">
      <button class="btn">{{ page }}</button>
    </li>
    <li @click="goToNextPage(page)"
      class="flex items-center justify-center cursor-pointer px-3 h-8 ml-0 leading-tight bg-brand-color border-l-2 border-black">
      <button class="btn">Next</button>
    </li>
  </ul>
</div>
    </div>
  </section>
</template>

<script>
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/navigation';
import { Navigation } from 'swiper/modules';
import Loader from '../components/Loader.vue';
import filmsList from '../components/filmsList.vue';
export default {
  components: {
    Swiper,
    SwiperSlide,
    filmsList,
    Loader
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
      itemsPerPage: 12,
      totalPages: 22,
      selectedCategory: 'Фильмы',
      categories: ['https://api.allplay.uz/api/v1/categories'],
      filteredMovies: [],
      maxVisiblePages: 5,
      isLoading: true,
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
    for (const apiURL of this.apiList) {
      const response = await fetch(apiURL);
      const data = await response.json();
      this.listMovie = this.listMovie.concat(data.data);
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
  const startIndex = (this.currentPage - 1) * this.itemsPerPage;
  const endIndex = startIndex + this.itemsPerPage;
  if (this.selectedCategory === 'Фильмы') {
    return this.listMovie.slice(startIndex, endIndex);
  } else {
    return this.listMovie.filter((movie) => {
      const categoryMatches =
        movie.categories &&
        Array.isArray(movie.categories) &&
        movie.categories.some((cat) => cat.name === this.selectedCategory);
      return categoryMatches;
    }).slice(startIndex, endIndex);
  }
},
    isPageActive(page) {
      return this.currentPage === page;
    },
    calculateVisiblePages() {
      const startPage = Math.max(1, this.currentPage - Math.floor(this.maxVisiblePages / 2));
      const endPage = Math.min(this.totalPages, startPage + this.maxVisiblePages - 1);
      return Array.from({ length: endPage - startPage + 1 }, (_, index) => startPage + index);
    },

    goToPreviousPage() {
  if (this.currentPage > 1) {
    this.currentPage--;
    this.filteredMovies = this.filterMoviesByCategory();
  }
},

goToNextPage() {
  if (this.currentPage < this.totalPages) {
    this.currentPage++;
    this.filteredMovies = this.filterMoviesByCategory();
  }
},

goToPage(page) {
  if (page >= 1 && page <= this.totalPages) {
    this.currentPage = page;
    this.filteredMovies = this.filterMoviesByCategory();
  }
},

  },

  computed: {
    visiblePages() {
      return this.calculateVisiblePages();
    },
  },

  created() {
  this.fetchFeaturedMovies().then((data) => {
    if (data && Array.isArray(data.data) && data.data.length > 0) {
      this.listMovie = [...this.listMovie, ...data.data];
    }
    this.isLoading = false;
    this.filteredMovies = this.filterMoviesByCategory(); // Set initial value for filteredMovies
  });

  this.currentPage = 1;
  this.movieList().then(() => {
    this.selectedCategory = this.menuArray[0].title;
    this.isLoading = false;
  });
},
  mounted() {
    this.fetchFeaturedMovies().then((data) => {
      if (data && Array.isArray(data.data) && data.data.length > 0) {
        this.listMovie = [...this.listMovie, ...data.data];
      }
    });

    this.currentPage = 1;

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