<template>
  <div class="dark-theme-background fixed w-0 h-full" style="top: 50%;left: 50%;transform:translate(-50%, -50%);background: hsl(207, 26%, 17%);transition: 500ms;"></div>
  <MainNavbar class="z-20" />
  <div class="page-container light-theme px-6 sm:px-20 w-full min-h-screen">
    <div v-show="!infoPageIsOpen">
      <TableVue @on-country-select="openCountryInfo" />
    </div>

    <div v-show="infoPageIsOpen">
      <CountryVue @on-close="closeInfoPage" @on-select-country-by-shortname="selectCountryByShortName" :country="selectedCountry" />
    </div>
  </div>
</template>

<script>

import MainNavbar from './components/basic/MainNavbar.vue';
import TableVue from './components/tableVue/TableVue.vue';
import CountryVue from './components/countryVue/CountryVue.vue';

export default {
  name: 'App',
  components: {
    MainNavbar,
    TableVue,
    CountryVue,
  },
  data() {
    return {
      infoPageIsOpen: false,
      selectedCountry: {
        name: {
          common: "Loading...",
          nativeName: { 
            fin: { common: "Loading..." } 
          }
        },
        tld: [ 'loading...', ],
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
        capital: [ "Loading..." ],
        borders: [ "Loading..." ],
      },
    }
  },
  methods: {
    openCountryInfo(country = this.countries[0]) { this.selectCountry(country); },
    selectCountry(country) {
      country.languages = this.makeBetterLanguageObject(country.languages);
      country.currencies = this.makeBetterCurrenciesObject(country.currencies);
      this.selectedCountry = country;
      this.infoPageIsOpen = true;

      window.scrollTo(0, 0);
    },
    makeBetterLanguageObject(languagesObject) {
      if(typeof(languagesObject) === 'string')
        return languagesObject;

      let languages = [];
      let objKeys = Object.keys(languagesObject);
      for(let i = 0;i < objKeys.length;i++)
        languages.push(languagesObject[objKeys[i]]);
      
      return languages.join(', ');
    },
    makeBetterCurrenciesObject(currenciesObject) {
      if(typeof(currenciesObject) === 'string')
        return currenciesObject;

      let currencies = [];
      let keys = Object.keys(currenciesObject);
      for(let i = 0;i < keys.length;i++)
        currencies.push(currenciesObject[keys[i]].name);

      return currencies.join(", ");
    },
    selectCountryByShortName(shortName) {
      let savedAllCountries = JSON.parse(window.localStorage['savedAllCountries']);
      for(let i = 0;i < savedAllCountries.length;i++)
        if(savedAllCountries[i].fifa === shortName)
          this.openCountryInfo(savedAllCountries[i]);
    },
    closeInfoPage() { this.infoPageIsOpen = false; },
  },
}

</script>