<template>
    <li>
        <a class="dropdown-item"
            :class="{ 'is-active': selected }"
            @click="toggle">
            <div class="level">
                <div class="level-left">
                    <div class="level-item is-marginless"
                        v-if="item.items">
                        <dropdown-indicator :open="item.expanded"/>
                    </div>
                    <div class="level-item">
                        <p :class="{ 'padded': !item.items }">
                            {{ item.name }}
                        </p>
                    </div>
                </div>
                <div class="level-right"
                    v-if="selected">
                    <div class="level-item">
                        <a class="delete is-small"
                            @click.stop="$emit('selected', null)"/>
                    </div>
                </div>
            </div>
        </a>
        <items :items="item.items"
            :selection="selection"
            v-if="item.items && item.expanded"/>
    </li>
</template>

<script>
import DropdownIndicator from '@liberu-ui/dropdown-indicator';
import Items from './Items.vue';

export default {
    name: 'Item',

    components: { Items, DropdownIndicator },

    props: {
        item: {
            type: Object,
            required: true,
        },
        selection: {
            type: Number,
            default: null,
        },
    },

    emits: ['selected'],

    computed: {
        selected() {
            return this.selection === this.item.id;
        },
    },

    methods: {
        toggle() {
            this.item.expanded = !this.item.expanded;
            this.$emit('selected', this.item.id);
        },
    },
};
</script>

<style>
    .padded {
        margin-left: 24px;
    }
</style>
