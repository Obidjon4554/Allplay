<template>
    <div class="wrapper flex flex-wrap gap-2 items-center transition-all">
      <div  v-for="channel in channels" :key="channel.id" class="card w-[210px] text-center justify-center pb-3 hover:-translate-y-2 hover:shadow-md hover:shadow-gray-500">
        <a href="https://allplay.uz/c">
          <div class="image-block w-[210px] h-[120px] bg-white">
            <img :src="channel.icon.url_250x250" alt="" class="w-full h-full object-contain">
          </div>
          <p class="flex gap-4 m-4 text-gray-400 text-base">10:35<li class="list-disc"> Новости</li></p>
        </a>
        </div>
      </div>
</template>

<script>
export default {
  props: {
    channels: {
      type: Array,
      required: true,
    },

  },
};
</script>