<template>
  <div class="modal fade show d-block bg-dark bg-opacity-25" tabindex="-1" aria-modal="true" role="dialog">
    <div class="modal-dialog modal-sm modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header py-2">
          <h5 class="modal-title text-success"><i class="bi bi-cart-check"></i> Agregado al carrito</h5>
          <button type="button" class="btn-close" @click="$emit('close')"></button>
        </div>
        <div class="modal-body text-center">
          <img :src="product.image" width="60" height="60" :alt="product.name" class="rounded mb-2"/>
          <p class="mb-1">{{ product.name }}</p>
          <span class="badge bg-success">x{{ amount }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ['product', 'amount']
};
</script>
