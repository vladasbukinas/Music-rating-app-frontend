<template>
    <tr>
        <td class="px-4 py-4 border-b border-gray-200 bg-white sm:text-3xl text-xl">
            <small class="text-gray-500">{{ tableNumber }}</small>
        </td>
        <td class="px-3 py-2 border-b border-gray-200 bg-white">
            <div class="flex items-center sm:text-base text-sm">
                <router-link :to="{ name: 'release', params: { id: data.release.spotify_id }}" class="flex items-center overflow-ellipsis">
                    <div class="flex-shrink-0 sm:w-32 sm:h-32 w-14 h-14 p-2">
                            <img class="w-full h-full rounded-md"
                            :src="data.release.release_image"
                            :alt="data.release.name" />
                    </div>
                </router-link>
                <div class="sm:pl-8 pl-3 whitespace-no-wrap text-gray-500">
                    <p class="text-sm sm:text-base ">
                      <router-link :to="{ name: 'artist', params: { id: data.release.artist.spotify_id }}">
                        {{ data.release.artist.name }}&nbsp;
                      </router-link>
                    </p>
                    <router-link :to="{ name: 'release', params: { id: data.release.spotify_id }}" class="overflow-ellipsis">
                        <p class="text-sm">{{ shortenReleaseName }}</p>
                        <p class="text-xs"><small> {{data.release.release_date}} </small></p>
                    </router-link>
                </div>
            </div>
        </td>
        <td class="px-4 py-4 border-b hidden sm:table-cell border-gray-200 bg-white sm:text-xl text-base">
            <small class="text-gray-500">{{ data.count }}</small>
        </td>
        <td class="px-4 py-4 border-b border-gray-200 bg-white sm:text-base text-sm">
            <p class="capitalize text-gray-500 whitespace-no-wrap">
                {{ ratingAverage }}  <i class="fa fa-star text-yellow-300" aria-hidden="true"></i>
            </p>
        </td>
        <td class="px-4 py-4 border-b border-gray-200 bg-white sm:text-xl text-base">
            <a rel="noreferrer" :href="'https://open.spotify.com/album/'+data.release.spotify_id" target="_blank" class="text-green-500">
                <i class="fab fa-spotify" aria-hidden="true"></i>
            </a>
        </td>
    </tr>
</template>
<script>

export default {
    props:{
        data:Object,
        index:Number,
    },
    computed:{
        ratingAverage(){
            return Number((this.data.average).toFixed(2));
        },
        tableNumber(){
            return (this.index + 1)
        },
        shortenReleaseName(){
            return  parseInt(this.data.release.name.length) > 15 ?      this.data.release.name.substring(0,15)+"..." :
                                                                        this.data.release.name
        }
    }
}
</script>
