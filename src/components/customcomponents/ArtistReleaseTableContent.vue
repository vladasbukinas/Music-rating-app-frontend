<template>
  <tr>
    <td class="px-3 py-2 border-b border-gray-200 bg-white text-sm">
      <div class="flex items-center">
        <router-link :to="{ name: 'release', params: { id: data.id }}" class="flex items-center overflow-ellipsis">
          <div class="flex-shrink-0 lg:w-18 lg:h-18 w-10 h-10">
            <img class="w-full h-full rounded-md"
                 :src="data.images[2]['url']"
                 :alt="data.name" />
          </div>
          <div>
            <p class="text-gray-900 whitespace-no-wrap px-4">{{ shortenArtistName }}</p>
            <p class="text-gray-500 whitespace-no-wrap px-4"><small> {{data.release_date}} </small></p>
          </div>
        </router-link>
      </div>
    </td>
    <td class="px-2 py-2 border-b hidden sm:table-cell border-gray-200 bg-white sm:text-sm text-xs">
      <small class="text-gray-500" >({{ data.album_type }})</small>
    </td>
    <td class="px-2 py-2 border-b hidden sm:table-cell border-gray-200 bg-white sm:text-sm text-xs">
      <small class="text-gray-500">{{ data.rating_count }}</small>
    </td>
    <td class="px-2 py-2 border-b border-gray-200 bg-white sm:text-sm text-xs">
      <p class="capitalize text-gray-500 whitespace-no-wrap">
        {{ ratingAverage }}  <i class="fa fa-star text-yellow-300" aria-hidden="true"></i>
      </p>
    </td>
    <td class="px-2 py-2 border-b border-gray-200 bg-white sm:text-sm text-sm">
      <a rel="noreferrer" :href="data.external_urls['spotify']" target="_blank" class="text-green-500">
        <i class="fab fa-spotify" aria-hidden="true"></i>
      </a>
    </td>
  </tr>
</template>
<script>

export default {
  props:{
    data:Object,
  },
  computed:{
    ratingAverage(){
      return Number((this.data.rating_average).toFixed(2));
    },
    shortenArtistName(){
      return parseInt(this.data.name.length) > 20 ?      this.data.name.substring(0,20)+"..." :
          this.data.name
    }
  }
}
</script>
