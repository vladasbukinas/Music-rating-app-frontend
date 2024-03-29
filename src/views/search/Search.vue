<template>
  <app-layout :searchActive="true">
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">
        Search for releases
      </h2>
    </template>
    <div class="w-full lg:max-w-7xl rounded-sm mx-auto p-2 lg:py-4 lg:px-8 h-full">
      <custom-table :hasRecords="(getAlbums != null)  ? (getAlbums.items.length > 0) ? true : false : false"
                    :isLoading="(this.getAlbums == null)  ? true : false"
                    divClass="container mx-auto relative h-full"
      >
        <template v-slot:search_input>
                    <span class="h-full absolute inset-y-0 left-0 flex items-center pl-2">
                        <svg viewBox="0 0 24 24" class="h-4 w-4 fill-current text-gray-500">
                            <path
                                d="M10 4a6 6 0 100 12 6 6 0 000-12zm-8 6a8 8 0 1114.32 4.906l5.387 5.387a1 1 0 01-1.414 1.414l-5.387-5.387A8 8 0 012 10z">
                            </path>
                        </svg>
                    </span>
          <input placeholder="Search"
                 class="appearance-none rounded-r rounded-l sm:rounded-l-none border border-gray-400 border-b block pl-8 pr-6 py-2 w-full bg-white text-sm placeholder-gray-400 text-gray-700 focus:bg-white focus:placeholder-gray-600 focus:text-gray-700 focus:outline-none"
                 type="text"
                 v-model="params.term"
                 @input="search"
                 id="search"
          />
        </template>
        <template v-slot:selection>
          <select
              v-model.number="params.perPage"
              @change=changePerPage()
              class="appearance-none h-full rounded-l border block w-full bg-white border-gray-400 text-gray-700 py-2 px-4 pr-8 leading-tight focus:outline-none focus:bg-white focus:border-gray-500">
            <option>5</option>
            <option>10</option>
            <option>15</option>
            <option>20</option>
          </select>
        </template>

        <template v-slot:head>
          <th
              class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase w-1/5 tracking-wider">
            Cover
          </th>
          <th
              class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase w-1/3 tracking-wider">
            Release
          </th>
          <th
              class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase w-1/3 tracking-wider">
            Release date
          </th>
          <th
              class="px-5 py-3 hidden sm:table-cell border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase w-1/6 tracking-wider">
            Type
          </th>
        </template>
        <template v-slot:content>
          <release-table-content v-for="album in (this.releases != null) ? releases.albums.items : []" :key="album.id"
                                 :data="album"></release-table-content>
        </template>
        <template v-if="(this.releases.albums != null)  ? (releases.albums.total > params.perPage) ? true : false : false" v-slot:pagination>
          <pagination :paginationNext="nextPage" :paginationPrevious="previousPage">
            <template v-slot:entrycount>
              {{ parseEntryCount }}
            </template>
          </pagination>
        </template>
      </custom-table>
    </div>
  </app-layout>
</template>
<script>
import AppLayout from '../../AppLayout'
import Pagination from '../../components/customcomponents/Pagination'
import CustomTable from '../../components/customcomponents/Table'
import ReleaseTableContent from '../../components/customcomponents/SearchReleaseTableContent'
import NotFoundError from '../../components/customcomponents/NotFoundError'
import {throttle} from "@/helpers";
import {mapActions, mapGetters, mapState} from 'vuex';
import _ from 'lodash'

export default {
  created() {
    this.params.term = "";
    this.params.perPage = 15;
    this.params.offset = 0;
    this.retrieveReleases(this.params);
    this.resetErrors();
    this.resetSuccess();
  },
  data() {
    return {
      params: {
        term: null,
        perPage: null,
        offset: null,
      }
    }
  },
  components: {
    Pagination,
    CustomTable,
    ReleaseTableContent,
    NotFoundError,
    AppLayout,
  },
  methods: {
    ...mapActions(['retrieveReleases', 'resetErrors', 'resetSuccess']),
    search:_.throttle(function(){
      this.params.offset = 0
      this.retrieveReleases(this.params);
    },500),
    changePerPage() {
      this.params.offset = 0
      this.retrieveReleases(this.params);
    },
    nextPage() {
      if (this.releases.albums.total > (this.params.offset + this.params.perPage)) {
        this.params.offset = this.params.perPage + this.params.offset;
        this.retrieveReleases(this.params);
      }
    },
    previousPage() {
      if (this.params.offset >= this.params.perPage) {
        this.params.offset = this.params.offset - this.params.perPage;
        this.retrieveReleases(this.params);
      } else {
        this.params.offset = 0
        this.retrieveReleases(this.params);
      }
    },
  },
  computed: {
    parseEntryCount() {
      let entryLimit = this.params.offset + this.params.perPage + 1;
      entryLimit = (entryLimit > this.releases.albums.total) ? this.releases.albums.total : entryLimit;

      return "Showing " + parseInt(this.params.offset + 1) + " to " + entryLimit + " of " + this.releases.albums.total;
    },
    ...mapState({
      releases: state => state.release.releases,
    }),
    ...mapGetters(['getAlbums']),
  },
}
</script>

