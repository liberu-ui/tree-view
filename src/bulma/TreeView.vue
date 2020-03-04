<template>
    <div class="box is-shadowless">
        <p class="control has-icons-left has-icons-right">
            <input class="input"
                v-model="query"
                :placeholder="i18n('Search')">
            <span class="icon is-left">
                <fa icon="search"/>
            </span>
            <span class="icon is-right">
                <a class="delete is-small"
                    @click="query = ''"
                    v-if="query"/>
            </span>
        </p>
        <div class="tree-items no-scrollbars">
            <items class="parent has-margin-top-large"
                :items="filteredItems"
                :selection="value"
                @selected="$emit('input', $event)"/>
        </div>
    </div>
</template>

<script>

import { library } from '@fortawesome/fontawesome-svg-core';
import { faSearch } from '@fortawesome/free-solid-svg-icons';
import Items from './Items.vue';

library.add(faSearch);

export default {
    name: 'TreeView',

    components: { Items },

    inject: ['i18n'],

    props: {
        items: {
            type: Array,
            required: true,
        },
        value: {
            type: Number,
            default: null,
        },
    },

    data: () => ({
        query: '',
    }),

    computed: {
        filteredItems() {
            return this.query
                ? this.filter(this.replica)
                : this.items;
        },
        replica() {
            return JSON.parse(JSON.stringify(this.items));
        },
    },

    methods: {
        filter(items) {
            return items
                .filter(item => this.matches(item))
                .map(item => this.map(item));
        },
        focus(items, value) {
            return value && items && items
                .some(item => this.shouldFocus(item, value));
        },
        map(item) {
            if (item.items) {
                item.items = this.filter(item.items);
            }

            return item;
        },
        matches(item) {
            return item.name.toLowerCase()
                .indexOf(this.query.toLowerCase()) > -1
                || item.items && item.items
                    .some(child => this.matches(child));
        },
        shouldFocus(item, value) {
            item.expanded = item.id === value || this.focus(item.items, value);

            return item.expanded;
        },
        updateFocus() {
            this.focus(this.items, this.value);
        },
    },
};
</script>

<style lang="scss">
    .tree-items {
        max-height: 25em;
        overflow: auto;
    }
</style>
