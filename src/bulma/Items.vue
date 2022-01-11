<template>
    <ul class="items">
        <item v-bind="$attrs"
            v-for="item in items"
            :key="item.id"
            :item="item"/>
    </ul>
</template>

<script>

export default {
    name: 'Items',

    components: { Item: () => import('./Item.vue') },

    props: {
        items: {
            type: Array,
            required: true,
        },
    },
};
</script>

<style>
    ul.items:not(.parent) > li{
        padding-left: 1em;
    }
</style>
