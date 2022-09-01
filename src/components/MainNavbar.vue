<template>
    <nav class="light-theme fixed top-0 w-full h-20 shadow-md flex items-center justify-between">
        <p class="light-theme text-2xl font-extrabold ml-20">Where in the world?</p>
        <button class="light-theme text-base font-semibold mr-20">
            <i class="bi bi-moon mr-0.5"></i>
            Dark Mode
        </button>
    </nav>
</template>

<script>

export default {
    name: 'MainNavbar'
}

</script>

<style scoped>

nav {
    background: var(--element-color);
}

</style>