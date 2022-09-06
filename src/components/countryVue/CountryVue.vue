<template>
    <div class="back-btn-container mt-32 sm:mt-44 w-full flex flex-row justify-start items-start">
      <button @click="this.$emit('on-close')" class="back-btn light-theme relative h-8 sm:h-12 w-28 sm:w-44 rounded-sm sm:rounded-md overflow-hidden shadow-md flex justify-center items-center gap-3" style="background: var(--element-color);">
        <i class="bi bi-arrow-left text-lg sm:text-2xl"></i>
        <p class="light-theme text-sm sm:text-xl font-semibold">Back</p>
      </button>
    </div>
    <div class="country-contents mt-20 w-full flex flex-col sm:flex-row justify-start sm:justify-between items-center sm:items-start gap-8 sm:gap-0">
      <div class="img w-full sm:w-[36.5rem]">
        <img :src="country.flags.png" :alt="country.name.common + ' flag'" class="w-full">
      </div>
      <div class="country-content w-full sm:w-[36.5rem] flex flex-col justify-start items-start">
        <p class="text-3xl mt-12 font-extrabold">{{ country.name.common }}</p>
        <div class="country-info w-full mt-8 grid grid-cols-2 gap-y-2 sm:gap-x-4">
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Native Name: <span class="font-semibold">{{ country.name.nativeName[Object.keys(country.name.nativeName)[0]].common }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible">Top Level Domain: <span class="font-semibold">{{ country.tld[0] }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Population: <span class="font-semibold">{{ country.population }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible">Currencies: <span class="font-semibold">{{ country.currencies }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Region: <span class="font-semibold">{{ country.region }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible">Languages: <span class="font-semibold">{{ country.languages }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Sub Region: <span class="font-semibold">{{ country.subregion }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 hidden sm:visible"> <span class="font-semibold"></span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1">Capital: <span class="font-semibold">{{ country.capital.join(', ') }}</span></p>
          
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>

          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 visible sm:hidden">Top Level Domain: <span class="font-semibold">{{ country.tld[0] }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 visible sm:hidden">Currencies: <span class="font-semibold">{{ country.currencies }}</span></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2 sm:col-span-1 visible sm:hidden">Languages: <span class="font-semibold">{{ country.languages }}</span></p>
          
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>
          <p class="light-theme text-left text-base font-extrabold col-span-2"></p>

          <div class="light-theme col-span-2 flex flex-row justify-start items-start gap-2 flex-wrap mb-8 sm:mb-0">
            <p class="light-theme text-left text-base font-extrabold h-8 flex justify-start sm:justify-center items-center w-full sm:w-auto">Border Countries:</p> 
            <span v-for:="borderCountry in country.borders" @click="this.$emit('on-select-country-by-shortname', borderCountry)" class="font-semibold cursor-pointer px-8 h-8 shadow-md flex justify-center items-center rounded-sm" style="background: var(--element-color);">
              {{ borderCountry }}
            </span>
          </div>
        </div>
      </div>
    </div>
</template>

<script>

export default {
    name: 'CountryVue',
    props: ['country'],
    emits: ['on-close', 'on-select-country-by-shortname']
}

</script>