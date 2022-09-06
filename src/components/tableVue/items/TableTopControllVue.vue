<template>
    <div class="top-controll z-10 mt-24 sm:mt-32 w-full flex flex-row justify-between items-start flex-wrap sm:flex-nowrap">
      <div class="search-box light-theme relative h-12 sm:h-16 w-[30rem] rounded-md overflow-hidden">
        <input type="text" name="search-input" id="search-input" v-model="searchValue" @change="tableSearch" class="light-theme w-full h-full top-0 left-0 right-0 bottom-0 outline-none pl-14 sm:pl-20 pr-4 text-sm sm:text-base" style="background: transparent;" placeholder="Search for a country...">
        <i class="bi bi-search absolute left-0 ml-6 sm:ml-[2.2rem] text-base sm:text-xl top-[50%]"></i>
      </div>
      <div class="filter-box light-theme relative h-12 sm:h-16 w-48 sm:w-64 rounded-md mt-10 sm:mt-0">
        <div class="select light-theme cursor-pointer h-12 sm:h-16 w-full flex justify-between items-center">
          <p class="light-theme ml-6 text-sm sm:text-base">Filter by Region</p>
          <i class="bi bi-chevron-down mr-6 text-sm sm:text-base"></i>
        </div>
        <div v-show="filterBoxIsOpen" class="options light-theme absolute top-[3.4rem] sm:top-[4.3rem] py-2 w-full flex flex-col justify-start items-center rounded-md shadow-md">
          <button v-for:="(region, index) in regions" @click="toggleRegionSelection(index)" class="region-selector-option light-theme pl-6 w-full text-left py-1 transition duration-200 text-sm sm:text-base" :style="[region.selected ? 'color: rgb(34 213 44);' : '']">{{ region.name }}</button>
        </div>
      </div>
    </div>
</template>

<script>

export default {
  name: 'TableTopControllVue',
  data() {
    return ({
      filterBoxIsOpen: 0,
      searchValue: '',
      regions: [ { name: 'Africa', selected: false }, { name: 'Americas', selected: false }, { name: 'Asia', selected: false }, { name: 'Europe', selected: false }, { name: 'Oceania', selected: false } ],
    });
  },
  methods: {
    filterBoxOptionsToggleHandler() {
      document.querySelector('body').addEventListener('click', (event) => {
        if(document.querySelector('.filter-box > .select').contains(event.target))
          this.filterBoxIsOpen = 1 - this.filterBoxIsOpen;
        else
          this.filterBoxIsOpen = 0;
      });
    },
    tableSearch() { this.$emit('on-search', this.searchValue, this.regions); },
    toggleRegionSelection(index) {
      this.regions[index]['selected'] = 1 - this.regions[index]['selected'];
      this.$emit('on-search', this.searchValue, this.regions);
    }
  },
  emits: ['on-search'],
  mounted() { this.filterBoxOptionsToggleHandler() }
}

</script>

<style scoped>

.search-box { background: var(--element-color); box-shadow: 0 0 15px rgba(0, 0, 0, 0.08); }

.search-box > i { color: var(--input-color); transform: translate(0, -50%); }

.filter-box { background: var(--element-color); box-shadow: 0 0 15px rgba(0, 0, 0, 0.08); }

.filter-box > .options { background: var(--element-color); }

#search-input::-webkit-input-placeholder { color: var(--input-color); font-family: 'Nunito Sans', sans-serif; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }

.region-selector-option.dark-theme:hover { color: rgb(0, 222, 85); }

.region-selector-option.light-theme:hover { background: rgb(236, 236, 236); }

</style>