<script setup lang="ts">
import type { Ref } from 'vue';
import { shallowRef } from 'vue';
import { useFacets } from './useFacets';

interface Item {
    id: string;
    name: string;
    color: Set<string>;
    brand: Set<string>;
    size: Set<string>;
    category: Set<string>;
}

// const possibleColors = ['red', 'blue', 'green', 'yellow', 'purple', 'orange'];
// const possibleBrands = ['A', 'B', 'C', 'D', 'E', 'F'];
// const possibleCategories = ['AAA', 'BBB', 'CCC', 'DDD', 'EEE', 'FFF'];
// const possibleSizes = ['S', 'M', 'L', 'XL', 'XXL'];

// const items: Ref<Item[]> = shallowRef(
//     Array.from({ length: 10000 }, (_, i) => ({
//         id: i.toString(),
//         name: `Item ${i}`,
//         color: new Set([possibleColors[Math.floor(Math.random() * possibleColors.length)]]),
//         brand: new Set([possibleBrands[Math.floor(Math.random() * possibleBrands.length)]]),
//         size: new Set([possibleSizes[Math.floor(Math.random() * possibleSizes.length)]]),
//         category: new Set([
//             possibleCategories[Math.floor(Math.random() * possibleCategories.length)],
//         ]),
//     })),
// );

const items: Ref<Item[]> = shallowRef([
    {
        id: '1',
        name: 'Item 1',
        color: new Set(['red', 'green']),
        brand: new Set(['A']),
        size: new Set(['S']),
        category: new Set(['AAA']),
    },
    {
        id: '2',
        name: 'Item 2',
        color: new Set(['blue']),
        brand: new Set(['B']),
        size: new Set(['M']),
        category: new Set(['BBB']),
    },
    {
        id: '3',
        name: 'Item 3',
        color: new Set(['yellow', 'red']),
        brand: new Set(['C']),
        size: new Set(['L']),
        category: new Set(['CCC', 'AAA']),
    },
    {
        id: '4',
        name: 'Item 4',
        color: new Set(['yellow']),
        brand: new Set(['D']),
        size: new Set(['XL']),
        category: new Set(['DDD', 'AAA']),
    },
    {
        id: '5',
        name: 'Item 5',
        color: new Set(['purple']),
        brand: new Set(['E']),
        size: new Set(['XXL']),
        category: new Set(['EEE']),
    },
    {
        id: '6',
        name: 'Item 6',
        color: new Set(['orange']),
        brand: new Set(['F']),
        size: new Set(['S']),
        category: new Set(['FFF']),
    },
]);

const {
    count,
    facetCategories,
    facets,
    filteredItems,
    activeFacets,
    activeFacetsKeys,
    toggleFacet,
    removeFacet,
} = useFacets<Item, 'color' | 'brand' | 'size' | 'category'>(items, {
    isImmediate: true,
    facets: [
        {
            category: 'color',
            label: 'color',
            queryKey: 'color',
        },
        {
            category: 'brand',
            label: 'brand',
            queryKey: 'brand',
        },
        {
            category: 'size',
            label: 'size',
            queryKey: 'size',
        },
        {
            category: 'category',
            label: 'category',
            queryKey: 'category',
        },
    ],
});
</script>

<template>
    <main class="p-10">
        Active facets:
        <template v-for="category in activeFacetsKeys" :key="category">
            <button
                class="ml-5 bg-gray-200 px-5 py-3 rounded"
                v-for="facet in activeFacets[category]"
                :key="facet"
                @click="removeFacet(category, facet)"
            >
                {{ facet }}
            </button>
        </template>

        <div v-for="category in facetCategories" :key="category" class="mt-5">
            <span class="font-bold block text-xl">{{ category }}</span>

            <button
                :disabled="facets[category].facets[facet]?.isDisabled"
                class="px-5 py-3 rounded bg-gray-100 mr-5"
                v-for="facet in Object.keys(facets[category].facets)"
                :key="facet"
                @click="toggleFacet(category, facet)"
                :class="{
                    'bg-gray-200': facets[category].facets[facet]?.isActive,
                    'opacity-50': facets[category].facets[facet]?.isDisabled,
                }"
            >
                {{ facet }}
            </button>
        </div>

        <section class="mt-5">
            <p class="p-2 bg-gray-200 rounded">Items count: {{ count }}</p>

            <div class="grid grid-cols-4 gap-5 mt-5">
                <div
                    class="border bg-gray-100 rounded p-4"
                    v-for="item in filteredItems"
                    :key="item.id"
                >
                    <h3>{{ item.name }}</h3>
                    <p>color: {{ Array.from(item.color).join(', ') }}</p>
                    <p>brand: {{ Array.from(item.brand).join(', ') }}</p>
                    <p>size: {{ Array.from(item.size).join(', ') }}</p>
                    <p>category: {{ Array.from(item.category).join(', ') }}</p>
                </div>
            </div>
        </section>
    </main>
</template>
