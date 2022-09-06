<template>
  <div class="countries mt-12 w-full grid grid-cols-1 sm:grid-cols-4 gap-16">
    <div v-for:="country in countries" class="country-card mx-8 sm:mx-0 cursor-pointer rounded-lg overflow-hidden" @click="this.$emit('on-country-select', country)">
      <img :src="country.flags.png" alt="mt" loading="lazy" class="w-full h-[190px]">
      <p class="country-name text-xl font-semibold sm:font-extrabold w-full px-6 mt-6">{{ country.name.common }}</p>
      <p class="country-population text-base font-semibold sm:font-extrabold w-full px-6 mt-2">Population: <span class="font-light sm:font-semibold"> {{ country.population }}</span></p>
      <p class="country-region text-base font-semibold sm:font-extrabold w-full px-6 mt-1">Region: <span class="font-light sm:font-semibold"> {{ country.region }}</span></p>
      <p class="country-capital text-base font-semibold sm:font-extrabold w-full px-6 mt-1 mb-12">Capital: <span class="font-light sm:font-semibold"> {{ country.capital[0] }}</span></p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'TableBodyVue',
  props: ['countries'],
  emits: ['on-country-select'],
}

</script>

<style scoped>

.country-card { background: var(--element-color); box-shadow: 0 0 15px rgba(0, 0, 0, 0.08); }

</style>