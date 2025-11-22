<template>
  <div class="container mb-5">
    <h2 class="mb-4">Catálogo de celulares</h2>
    <div v-if="loading" class="text-center py-5">
      <div class="spinner-border text-primary" role="status"></div>
      <p class="mt-3">Cargando productos...</p>
    </div>
    <div v-else-if="error" class="alert alert-warning">
      Ocurrió un error al cargar productos.
    </div>
    <div v-else>
      <div class="row g-4">
        <ProductCard
          v-for="prod in filtered"
          :key="prod.id"
          :product="prod"
          @add="add"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ProductCard from "./ProductCard.vue";
import { ref, computed, onMounted } from "vue";

export default {
  components: { ProductCard },
  props: ["search"],
  emits: ["add"],
  setup(props, { emit }) {
    const products = ref([]);
    const loading = ref(true);
    const error = ref("");

    onMounted(async () => {
      try {
        await new Promise(resolve => setTimeout(resolve, 1000));
        const phones = (await import("../assets/phones.js")).default;
        products.value = phones;
      } catch (err) {
        error.value = "No se pudo cargar el catálogo.";
      } finally {
        loading.value = false;
      }
    });

    const filtered = computed(() =>
      props.search
        ? products.value.filter(p =>
            p.name.toLowerCase().includes(props.search.toLowerCase()))
        : products.value
    );

    const add = (product, amount) => emit("add", product, amount);

    return { filtered, add, loading, error };
  }
};
</script>
