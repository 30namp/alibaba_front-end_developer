<template>
  <div class="dark-theme-background fixed w-0 h-full" style="top: 50%;left: 50%;transform:translate(-50%, -50%);background: hsl(207, 26%, 17%);transition: 500ms;"></div>
  <MainNavbar class="z-20" />
  <div class="page-container light-theme px-6 sm:px-20 w-full min-h-screen">
    <div v-show="infoPageIsOpen === false" class="top-controll z-10 mt-24 sm:mt-32 w-full flex flex-row justify-between items-start flex-wrap sm:flex-nowrap">
      <div class="search-box light-theme relative h-12 sm:h-16 w-[30rem] rounded-md overflow-hidden">
        <input type="text" name="search-input" id="search-input" v-model="searchValue" @change="updateResultBySearch" class="light-theme w-full h-full top-0 left-0 right-0 bottom-0 outline-none pl-14 sm:pl-20 pr-4 text-sm sm:text-base" style="background: transparent;" placeholder="Search for a country...">
        <i class="bi bi-search absolute left-0 ml-6 sm:ml-[2.2rem] text-base sm:text-xl top-[50%]"></i>
      </div>
      <div class="filter-box light-theme relative h-12 sm:h-16 w-48 sm:w-64 rounded-md mt-10 sm:mt-0">
        <div class="select light-theme cursor-pointer h-12 sm:h-16 w-full flex justify-between items-center">
          <p class="light-theme ml-6 text-sm sm:text-base">Filter by Region</p>
          <i class="bi bi-chevron-down mr-6 text-sm sm:text-base"></i>
        </div>
        <div v-show="filterBoxIsOpen" class="options light-theme absolute top-[3.4rem] sm:top-[4.3rem] py-2 w-full flex flex-col justify-start items-center rounded-md shadow-md">
          <button v-for:="(region, index) in regions" @click="toggleSelectRegion(index)" class="region-selector-option light-theme pl-6 w-full text-left py-1 transition duration-200 text-sm sm:text-base" :style="[region.selected ? 'color: rgb(34 213 44);' : '']">{{ region.name }}</button>
        </div>
      </div>
    </div>
    <div v-show="infoPageIsOpen === false" class="countries mt-12 w-full grid grid-cols-1 sm:grid-cols-4 gap-16">
      <div v-for:="country in countries" class="country-card mx-8 sm:mx-0 cursor-pointer light-theme rounded-lg overflow-hidden" @click="openCountryInfo(country)">
        <img :src="country.flags.png" alt="mt" loading="lazy" class="w-full h-[190px]">
        <p class="country-name text-xl font-semibold sm:font-extrabold w-full px-6 mt-6">{{ country.name.common }}</p>
        <p class="country-population text-base font-semibold sm:font-extrabold w-full px-6 mt-2">Population: <span class="font-light sm:font-semibold"> {{ country.population }}</span></p>
        <p class="country-region text-base font-semibold sm:font-extrabold w-full px-6 mt-1">Region: <span class="font-light sm:font-semibold"> {{ country.region }}</span></p>
        <p class="country-capital text-base font-semibold sm:font-extrabold w-full px-6 mt-1 mb-12">Capital: <span class="font-light sm:font-semibold"> {{ country.capital[0] }}</span></p>
      </div>
    </div>
    <div v-show="infoPageIsOpen === false && countries.length" class="country-stack-pages mt-12 w-full grid-cols-3 gap-16 mb-12">
      <div class="selectors col-span-1 flex justify-center items-center gap-2">
        <button @click="changeCountryStackPage(-1)" class="next light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md"><i class="bi bi-chevron-left"></i></button>
        <input type="text" class="selector light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md outline-none text-center" disabled @change="changeCountryStackPage" v-model="nowCountryStackPage" />
        <button @click="changeCountryStackPage(1)" class="prev light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md"><i class="bi bi-chevron-right"></i></button>
      </div>
    </div>

    <div v-show="infoPageIsOpen === true" class="back-btn-container mt-32 sm:mt-44 w-full flex flex-row justify-start items-start">
      <button @click="closeInfoPage" class="back-btn light-theme relative h-8 sm:h-12 w-28 sm:w-44 rounded-sm sm:rounded-md overflow-hidden shadow-md flex justify-center items-center gap-3" style="background: var(--element-color);">
        <i class="bi bi-arrow-left text-lg sm:text-2xl"></i>
        <p class="light-theme text-sm sm:text-xl font-semibold">Back</p>
      </button>
    </div>
    <div v-show="infoPageIsOpen === true" class="country-contents mt-20 w-full flex flex-col sm:flex-row justify-start sm:justify-between items-center sm:items-start gap-8 sm:gap-0">
      <div class="img w-full sm:w-[36.5rem]">
        <img :src="selectedCountry.flags.png" :alt="selectedCountry.name.common + ' flag'" class="w-full">
      </div>
      <div class="country-content w-full sm:w-[36.5rem] flex flex-col justify-start items-start">
        <p class="text-3xl mt-12 font-extrabold">{{ selectedCountry.name.common }}</p>
        <div class="country-info w-full mt-8 grid grid-cols-2 gap-y-2 sm:gap-x-4">
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Native Name: <span class="font-semibold">{{ selectedCountry.name.nativeName[Object.keys(selectedCountry.name.nativeName)[0]].common }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible">Top Level Domain: <span class="font-semibold">{{ selectedCountry.tld[0] }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Population: <span class="font-semibold">{{ selectedCountry.population }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible">Currencies: <span class="font-semibold">{{ selectedCountry.currencies }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Region: <span class="font-semibold">{{ selectedCountry.region }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible">Languages: <span class="font-semibold">{{ selectedCountry.languages }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Sub Region: <span class="font-semibold">{{ selectedCountry.subregion }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible"> <span class="font-semibold"></span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Capital: <span class="font-semibold">{{ selectedCountry.capital.join(', ') }}</span></p>
          
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>

          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 visible sm:hidden">Top Level Domain: <span class="font-semibold">{{ selectedCountry.tld[0] }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 visible sm:hidden">Currencies: <span class="font-semibold">{{ selectedCountry.currencies }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 visible sm:hidden">Languages: <span class="font-semibold">{{ selectedCountry.languages }}</span></p>
          
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>

          <div class="light-theme col-span-2 flex flex-row justify-start items-start gap-2 flex-wrap mb-8 sm:mb-0">
            <p class="light-theme text-left text-base font-extrabold h-8 flex justify-start sm:justify-center items-center w-full sm:w-auto">Border Countries:</p> 
            <span v-for:="borderCountry in selectedCountry.borders" @click="selectCountryByShortName(borderCountry)" class="font-semibold cursor-pointer px-8 h-8 shadow-md flex justify-center items-center rounded-sm" style="background: var(--element-color);">
              {{ borderCountry }}
            </span>
          </div>
        </div>
      </div>
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
      infoPageIsOpen: false,
      countryStackSize: 32,
      nowCountryStackPage: 1,
      filterBoxIsOpen: 0,
      searchValue: '',
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
      savedAllCountries: [],
      allCountries: [],
      countries: [],
      selectedCountry: {
        name: {
          common: "Loading...",
          nativeName: {
            fin: {
              common: "Loading..."
            }
          }
        },
        tld: [
          'loading...',
        ],
        flags: {
          png: "Loading...",
          svg: "Loading...",
        },
        population: "Loading...",
        currencies: {
          EUR: {
            name: "Loading...",
            symbol: "€",
          },
        },
        region: 'Loading...',
        subregion: 'Loading...',
        languages: "Loading...",
        capital: [
          "Loading..."
        ],
        borders: [
          "Loading...",
        ],
      },
    }
  },
  methods: {
    updateResultBySearch()
    {
      let allCountries = this.savedAllCountries;
      let pointer = 0;
      this.allCountries = [];
      let countries = [];
      let selectedRegions = [];
      for(let i = 0;i < this.regions.length;i++)
      {
        if(this.regions[i].selected)
        {
          selectedRegions.push(this.regions[i].name);
        }
      }
      if(!selectedRegions.length)
      {
        for(let i = 0;i < this.regions.length;i++)
        {
          selectedRegions.push(this.regions[i].name);
        }
      }
      while(pointer < allCountries.length)
      {
        countries = [];
        while(countries.length < this.countryStackSize && pointer < allCountries.length)
        {
          if(selectedRegions.includes(allCountries[pointer].region) && (this.searchValue === '' || allCountries[pointer].name.common.includes(this.searchValue)))
          {
            countries.push(allCountries[pointer]);
          }
          pointer++;
        }

        if(countries.length)
        {
          this.allCountries = [...this.allCountries, countries];
        }
      }

      this.nowCountryStackPage = 1;
      this.countries = this.allCountries[this.nowCountryStackPage - 1];
    },
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
      this.updateResultBySearch();
    },
    getCountries()
    {

      fetch('https://restcountries.com/v3.1/all').then((res) => res.json()).then((data) => {
        localStorage['countries'] = JSON.stringify(data);
        this.allCountries = data;
        for(let i = 0;i < this.allCountries.length;i++)
        {
          this.allCountries[i].population = this.makeBetterNumber(this.allCountries[i].population);
        }
        this.allCountries.sort();
        this.savedAllCountries = this.allCountries;
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

      this.scrollToTop();
    },
    openCountryInfo(country = this.countries[0])
    {
      this.selectCountry(country);
    },
    selectCountry(country)
    {
      country.languages = this.makeBetterLanguageObject(country.languages);
      country.currencies = this.makeBetterCurrenciesObject(country.currencies);
      this.selectedCountry = country;
      
      this.infoPageIsOpen = true;

      this.scrollToTop();
    },
    makeBetterNumber(num)
    {
      let strNum = num.toString();
      strNum = strNum.split('').reverse();
      num = '';
      for(let i = 0;i < strNum.length;i++)
      {
        num += strNum[i];
        if(!((i + 1) % 3) && i + 1 < strNum.length)
        {
          num += ',';
        }
      }
      num = num.split('').reverse().join('');
      return num;
    },
    makeBetterLanguageObject(languagesObject)
    {
      if(typeof(languagesObject) === 'string')
      {
        return languagesObject;
      }

      let languages = [];
      let objKeys = Object.keys(languagesObject);
      for(let i = 0;i < objKeys.length;i++)
      {
        languages.push(languagesObject[objKeys[i]]);
      }
      
      return languages.join(', ');
    },
    makeBetterCurrenciesObject(currenciesObject)
    {
      if(typeof(currenciesObject) === 'string')
      {
        return currenciesObject;
      }

      let currencies = [];
      let keys = Object.keys(currenciesObject);
      for(let i = 0;i < keys.length;i++)
      {
        currencies.push(currenciesObject[keys[i]].name);
      }

      return currencies.join(", ");
    },
    selectCountryByShortName(shortName)
    {
      for(let i = 0;i < this.savedAllCountries.length;i++)
      {
        if(this.savedAllCountries[i].fifa === shortName)
        {
          this.openCountryInfo(this.savedAllCountries[i]);
        }
      }
    },
    closeInfoPage()
    {
      this.infoPageIsOpen = false;
    },
    scrollToTop() {
      window.scrollTo(0,0);
    },
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
 
.region-selector-option.dark-theme:hover {
  color: rgb(0, 222, 85);
}

.region-selector-option.light-theme:hover {
  background: rgb(236, 236, 236);
}

</style>
