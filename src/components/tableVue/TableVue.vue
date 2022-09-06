<template>
    <TableTopControllVue @on-search="tableSearch" />
    <TableBodyVue @on-country-select="countrySelect" :countries="countries" />
    <TablePageSwitcherVue @on-page-update="updateCountriesPage" :PagesCount="allCountries" :nowPageNumber="nowTablePage" />
</template>

<script>

import TableTopControllVue from './items/TableTopControllVue.vue';
import TableBodyVue from './items/TableBodyVue.vue';
import TablePageSwitcherVue from './items/TablePageSwitcherVue.vue';

export default {
    name: 'TableVue',
    components: {
        TableTopControllVue,
        TableBodyVue,
        TablePageSwitcherVue,
    },
    data() {
        return ({
            elementCountLimit: 32,
            savedAllCountries: [],
            allCountries: [],
            countries: [],
            nowTablePage: 1,
        });
    },
    methods: {
        getCountries() {
            if(typeof(window.localStorage['savedAllCountries']) === 'string') {
                this.allCountries = JSON.parse(window.localStorage['savedAllCountries']);
                this.allCountries.sort();
                this.savedAllCountries = this.allCountries; 
                this.loadCountries();
            }

            fetch('https://restcountries.com/v3.1/all').then((res) => res.json()).then((data) => {
                this.allCountries = data;
                for(let i = 0;i < this.allCountries.length;i++)
                    this.allCountries[i].population = this.makeBetterNumber(this.allCountries[i].population);
                this.allCountries.sort();
                this.savedAllCountries = this.allCountries;
                window.localStorage['savedAllCountries'] = JSON.stringify(this.savedAllCountries);
                this.loadCountries();
            });

            this.countries = this.allCountries[0];
        },
        loadCountries() {
            let allCountries = this.allCountries;
            this.allCountries = [];
            let countries = [];
            let st = 0;
            while(st < allCountries.length) {
                countries = [];
                for(let i = st;i < Math.min(st + this.elementCountLimit, allCountries.length);i++)
                    countries.push(allCountries[i]);
                this.allCountries.push(countries);
                st = st + this.elementCountLimit;
            }
        },
        makeBetterNumber(num) {
            let strNum = num.toString().split('').reverse();
            num = '';
            for(let i = 0;i < strNum.length;i++) {
                num += strNum[i];
                if(!((i + 1) % 3) && i + 1 < strNum.length)
                    num += ',';
            }
            num = num.split('').reverse().join('');
            return num;
        },
        tableSearch(searchValue, searchRegions) {
            let pointer = 0, selectedRegions = [];
            this.allCountries = [];
            for(let i = 0;i < searchRegions.length;i++)
                if(searchRegions[i].selected)
                    selectedRegions.push(searchRegions[i].name);
            if(!selectedRegions.length)
                for(let i = 0;i < searchRegions.length;i++)
                    selectedRegions.push(searchRegions[i].name);
            while(pointer < this.savedAllCountries.length) {
                let countries = [];
                while(countries.length < this.elementCountLimit && pointer < this.savedAllCountries.length) {
                    if(selectedRegions.includes(this.savedAllCountries[pointer].region) && (searchValue === '' || this.savedAllCountries[pointer].name.common.toUpperCase().includes(searchValue.toUpperCase())))
                        countries.push(this.savedAllCountries[pointer]);
                    pointer++;
                }

                if(countries.length)
                    this.allCountries = [...this.allCountries, countries];
            }

            this.updateCountriesPage(0);
        },
        updateCountriesPage(index) {
            this.countries = this.allCountries[index];
            this.nowTablePage = index + 1;

            window.scrollTo(0, 0);
        },
        countrySelect(country) { this.$emit('on-country-select', country); },
    },
    emits: ['on-country-select'],
    mounted() { this.getCountries(); },
}

</script>