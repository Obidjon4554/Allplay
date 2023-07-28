<template>
  <section class="min-height-[100vh]  items-center justify-center py-20 w-full flex">
    <div class="container p-20 flex flex-col gap-8">
      <h1 class="title text-4xl">Список каналов</h1>
      <div class="flex gap-4 items-center ">
        <button @click="selectCategory('all')" :class="selectedCategory === 'all' ? 'bg-brand-color' : 'bg-gray-800'"
          class="link p-3 rounded-2xl">
          Все
        </button>
        <button v-for="(item, idx) in menuArray" :key="idx" @click="selectCategory(item.title)"
          :class="selectedCategory === item.title ? 'bg-brand-color' : 'bg-gray-800'" class="link p-3 rounded-2xl">
          {{ item.title }}
        </button>
      </div>
      <div class="flex gap-3">
        <input type="search" placeholder="Поиск"
          class="lowercase px-3 text-lg rounded-sm w-[400px] h-14 bg-gray-900 hover:bg-gray-800 focus:bg-gray-800"
          v-model="search" @keydown.enter="filteredList" />
      </div>
      <div v-if="isLoading" class="loader-container">
        <Loader />
      </div>
      <Chanels v-else :channels="filteredChannels"></Chanels>
    </div>
  </section>
</template>

<script>
import Loader from '../components/Loader.vue';
import Chanels from '../components/Chanels.vue';
export default {
  components: {
    Chanels,
    Loader
  },
  data() {
    return {
      api: 'https://api.allplay.uz/api/v1/iptv/channels',
      search: '',
      menuArray: [
        {
          title: 'Новостные',
        },
        {
          title: 'Кино',
        },
        {
          title: 'Премиум',
        },
        {
          title: 'Познавательные',
        },
        {
          title: 'Спорт',
        },
        {
          title: 'Музыка',
        },
        {
          title: 'Детские',
        },
        {
          title: 'Узбекские',
        },
      ],
      channels: [],
      selectedCategory: 'all',
      isLoading: true,
    };
  },
  mounted() {
    this.fetchChannels();
  },
  methods: {
    async fetchChannels() {
      try {
        const response = await fetch(this.api);
        const data = await response.json();
        this.channels = data.data;
      } catch (error) {
        console.error("Error fetching channels:", error);
      }
      this.isLoading = false;
    },
    selectCategory(category) {
      this.selectedCategory = category;
    },
  },
  computed: {
    filteredChannels() {
      return this.channels.filter(channel => {
        const nameMatches = channel.name.toLowerCase().includes(this.search.toLowerCase());
        const categoryMatches = this.selectedCategory === 'all' || channel.categories.some(cat => cat.name === this.selectedCategory);
        return nameMatches && categoryMatches;
      });
    },
  }
}
</script>