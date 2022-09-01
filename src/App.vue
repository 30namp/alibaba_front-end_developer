<template>
  <MainNavbar class="z-20" />
  <div class="page-container light-theme px-20 w-full min-h-screen">
    <div v-show="infoPageIsOpen === false" class="top-controll z-10 mt-32 w-full flex flex-row justify-between items-start">
      <div class="search-box light-theme relative h-16 w-[30rem] rounded-md overflow-hidden">
        <input type="text" name="search-input" id="search-input" class="light-theme w-full h-full top-0 left-0 right-0 bottom-0 outline-none pl-20 pr-4" placeholder="Search for a country...">
        <i class="bi bi-search absolute left-0 ml-[2.2rem] text-xl top-[50%]"></i>
      </div>
      <div class="filter-box light-theme relative h-16 w-64 rounded-md">
        <div class="select light-theme cursor-pointer h-16 w-full flex justify-between items-center">
          <p class="light-theme ml-6">Filter by Region</p>
          <i class="bi bi-chevron-down mr-6"></i>
        </div>
        <div v-show="filterBoxIsOpen" class="options light-theme absolute top-[4.3rem] py-2 w-full flex flex-col justify-start items-center rounded-md shadow-md">
          <button v-for:="(region, index) in regions" @click="toggleSelectRegion(index)" class="light-theme pl-6 w-full text-left py-1 hover:bg-gray-100 transition duration-200" :style="[region.selected ? 'color: rgb(34 213 44);' : '']">{{ region.name }}</button>
        </div>
      </div>
    </div>
    <div v-show="infoPageIsOpen === false" class="countries mt-12 w-full grid grid-cols-4 gap-16">
      <div v-for:="country in countries" class="country-card light-theme rounded-lg overflow-hidden" @click="openCountryInfo(country)">
        <img :src="country.flags.png" alt="mt" loading="lazy" class="w-full h-[190px]">
        <p class="country-name text-xl font-extrabold w-full px-6 mt-6">{{ country.name.common }}</p>
        <p class="country-population text-base font-semibold w-full px-6 mt-2">Population: <span class="font-light"> {{ country.population }}</span></p>
        <p class="country-region text-base font-semibold w-full px-6 mt-1">Region: <span class="font-light"> {{ country.region }}</span></p>
        <p class="country-capital text-base font-semibold w-full px-6 mt-1 mb-12">Capital: <span class="font-light"> {{ country.capital[0] }}</span></p>
      </div>
    </div>
    <div v-show="infoPageIsOpen === false" class="country-stack-pages mt-12 w-full grid-cols-3 gap-16 mb-12">
      <div class="selectors col-span-1 flex justify-center items-center gap-2">
        <button @click="changeCountryStackPage(-1)" class="next light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md"><i class="bi bi-chevron-left"></i></button>
        <input type="text" class="selector light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md outline-none text-center" disabled @change="changeCountryStackPage" v-model="nowCountryStackPage" />
        <button @click="changeCountryStackPage(1)" class="prev light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md"><i class="bi bi-chevron-right"></i></button>
      </div>
    </div>

    <div v-show="infoPageIsOpen === true" class="back-btn-container z-10 mt-32 w-full flex flex-row justify-start items-start">
      <button class="back-btn light-theme relative h-12 w-44 rounded-md overflow-hidden shadow-md flex justify-center items-center gap-3" style="background: var(--element-color);">
        <i class="bi bi-arrow-left text-2xl"></i>
        <p class="light-theme text-xl font-semibold">Back</p>
      </button>
    </div>
  </div>
</template>

<script>
import MainNavbar from './components/MainNavbar.vue';

export default {
  name: 'App',
  components: {
    MainNavbar
  },
  data() {
    return {
      infoPageIsOpen: true,
      countryStackSize: 32,
      nowCountryStackPage: 1,
      filterBoxIsOpen: 0,
      regions: [
        {
          name: 'Africa',
          selected: false
        },
        {
          name: 'Americas',
          selected: false
        },
        {
          name: 'Asia',
          selected: false
        },
        {
          name: 'Europe',
          selected: false
        },
        {
          name: 'Oceania',
          selected: false
        },
      ],
      allCountries: [],
      countries: [],
      selectedCountry: [],
    }
  },
  methods: {
    filterBoxOptionsToggleHandler()
    {
      document.querySelector('body').addEventListener('click', (event) => {
        if(document.querySelector('.filter-box > .select').contains(event.target))
        {
          this.filterBoxIsOpen = 1 - this.filterBoxIsOpen;
        }
        else
        {
          this.filterBoxIsOpen = 0;
        }
      });
    },
    toggleSelectRegion(index)
    {
      this.regions[index]['selected'] = 1 - this.regions[index]['selected'];
    },
    getCountries()
    {

      fetch('https://restcountries.com/v3.1/all').then((res) => res.json()).then((data) => {
        localStorage['countries'] = JSON.stringify(data);
        this.allCountries = data;
        this.loadCountries();
      });
      
    },
    loadCountries()
    {
      let allCountries = this.allCountries;
      this.allCountries = [];
      let countries = [];
      let st = 0;
      while(st < allCountries.length)
      {
        countries = [];
        for(let i = st;i < Math.min(st + this.countryStackSize, allCountries.length);i++)
        {
          countries.push(allCountries[i]);
        }
        this.allCountries.push(countries);
        st = st + this.countryStackSize;
      }
      this.countries = this.allCountries[0];
    },
    changeCountryStackPage(change = 0)
    {

      this.nowCountryStackPage = this.nowCountryStackPage + change;
      
      if(this.nowCountryStackPage < 1)
      {
        this.nowCountryStackPage = 1;
      }
      if(this.nowCountryStackPage >= this.allCountries.length)
      {
        this.nowCountryStackPage = this.allCountries.length;
      }
      this.countries = this.allCountries[this.nowCountryStackPage - 1];
    },
    openCountryInfo(country = this.countries[0])
    {
      this.selectedCountry = country;
      this.infoPageIsOpen = true;
    }
  },
  mounted() {
    this.filterBoxOptionsToggleHandler(),
    this.getCountries()
  },
}

</script>

<style scoped>

.search-box {
  background: var(--element-color);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.08);
}

.search-box > i {
  color: var(--input-color);
  transform: translate(0, -50%);
}

.filter-box {
  background: var(--element-color);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.08);
}

.filter-box > .options {
  background: var(--element-color);
}

#search-input::-webkit-input-placeholder {
  color: var(--input-color);
  font-family: 'Nunito Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.country-card {
  background: var(--element-color);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.08);
}

.country-stack-pages > .selectors > button {
  background: var(--element-color);
}

</style>
