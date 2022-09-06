<template>
    <div v-show="PagesCount" class="country-stack-pages mt-12 w-full grid-cols-3 gap-16 mb-12">
      <div class="selectors col-span-1 flex justify-center items-center gap-2">
        <button @click="changeTableBodyPage(-1)" class="next light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md"><i class="bi bi-chevron-left"></i></button>
        <input type="text" class="selector light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md outline-none text-center" disabled @change="changeTableBodyPage" :value="nowPageNumber" />
        <button @click="changeTableBodyPage(1)" class="prev light-theme h-12 w-12 flex justify-center items-center rounded-lg shadow-md"><i class="bi bi-chevron-right"></i></button>
      </div>
    </div>
</template>

<script>

export default {
  name: 'TablePageSwitcherVue',
  props: ['PagesCount', 'nowPageNumber'],
  methods: {
    changeTableBodyPage(change = 0) {
      if(this.nowPageNumber + change >= 1 && this.nowPageNumber + change <= this.PagesCount.length)
        this.$emit('on-page-update', this.nowPageNumber + change - 1);
      else
        alert('requested page not exist!');
    },
  },
}

</script>

<style scoped>

.country-stack-pages > .selectors > button { background: var(--element-color); }

</style>