<template>
    <nav class="light-theme fixed top-0 w-full h-20 shadow-md flex items-center justify-between">
        <p class="light-theme text-md sm:text-2xl font-extrabold ml-6 sm:ml-20">Where in the world?</p>
        <button @click="switchTheme" class="switch-theme-btn light-theme text-sm sm:text-base font-semibold mr-6 sm:mr-20">
            <i class="bi bi-moon mr-0.5"></i>
            Dark Mode
        </button>
    </nav>
</template>

<script>

export default {
    name: 'MainNavbar',
    data() {
        return ({
            theme: 'light',
        });
    },
    methods: {
        switchTheme()
        {
            let themeSwitchIcon = document.querySelector('.switch-theme-btn i');
            themeSwitchIcon.classList.toggle('bi-moon');
            themeSwitchIcon.classList.toggle('bi-moon-fill');

            let darkThemeBackground = document.querySelector('.dark-theme-background');
            darkThemeBackground.classList.toggle('w-0');
            darkThemeBackground.classList.toggle('w-full');

            let themedElements;

            if(this.theme === 'light')
            {
                themedElements = document.querySelectorAll('.light-theme');
                this.theme = 'dark';
            }
            else if(this.theme === 'dark')
            {
                themedElements = document.querySelectorAll('.dark-theme');
                this.theme = 'light';
            }

            window.localStorage['theme'] = this.theme;

            themedElements.forEach((elem) => {
                this.elementSwitchTheme(elem);
            });
        },
        elementSwitchTheme(elem) {
            elem.style.transition = '300ms';
            elem.classList.toggle('light-theme');
            elem.classList.toggle('dark-theme');
        }
    },
    mounted() {
        document.querySelector('body').addEventListener('onchange', () => {
            let selector = '';
            if(this.theme === 'dark')
                selector = '.light';
            else
                selector = '.dark';
            selector += '-theme';
            let elems = document.querySelectorAll(selector);
            elems.forEach((elem) => {
                this.elementSwitchTheme(elem);
            });
        });
    }
}

</script>

<style scoped>

nav {
    background: var(--element-color);
}

</style>