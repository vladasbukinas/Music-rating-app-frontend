<template>
    <router-link :to="href" :class="classes">
        <slot></slot>
    </router-link>
</template>

<script>
    export default {
        props: ['href', 'active'],

        computed: {
            classes() {
                return this.active
                            ? 'inline-flex items-center px-1 pt-1 border-b-2 border-white-100 text-sm font-medium leading-5 text-white focus:outline-none focus:border-green-700 transition duration-150 ease-in-out'
                            : 'inline-flex items-center px-1 pt-1 border-b-2 border-transparent text-sm font-medium leading-5 text-white hover:text-gray-200 hover:border-green-100 focus:outline-none focus:text-gray-200 focus:border-gray-300 transition duration-150 ease-in-out'
            }
        }
    }
</script>
