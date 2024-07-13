<template>
  <div>
    <div
      v-if="products && products.items"
      class="grid sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 p-8"
    >
      <div
        v-for="product in products.items"
        :key="product.id"
        class="bg-slate-800 text-gray-200 rounded-xl overflow-hidden shadow-lg p-6 border-2 hover:border-blue-900"
      >
        <div class="flex items-center justify-center pb-8">
          <img
            v-if="product.featuredAsset"
            :src="product.featuredAsset.preview"
            :alt="product.name"
            class="w-full h-48 object-cover rounded-xl"
          />
        </div>

        <h2 class="text-2xl font-bold mb-2">{{ product.name }}</h2>
        <p class="text-lg mb-2">{{ product.slug }}</p>
        <p class="text-lg mb-2">{{ product.description }}</p>
      </div>
    </div>
    <p v-else-if="loading">Loading...</p>
    <p v-else-if="error">Error: {{ error.message }}</p>
    <p v-else>No products found.</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from "vue";
import { useQuery } from "@vue/apollo-composable";
import { GetProductsDocument, GetProductsQuery } from "../gql/graphql"; // Adjust path as per your generated file location

export default defineComponent({
  setup() {
    const { result, loading, error } =
      useQuery<GetProductsQuery>(GetProductsDocument);

    const products = computed(() => result.value?.products);

    return { products, loading, error };
  },
});
</script>
