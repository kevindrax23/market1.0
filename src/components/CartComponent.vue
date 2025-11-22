<template>
  <div class="modal fade show d-block bg-dark bg-opacity-25" tabindex="-1" aria-modal="true" role="dialog">
    <div class="modal-dialog modal-lg modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title"><i class="bi bi-cart3"></i> Carrito de compras</h5>
          <button type="button" class="btn-close" @click="$emit('close')"></button>
        </div>
        <div class="modal-body">
          <table v-if="cart.length" class="table align-middle">
            <thead>
              <tr>
                <th></th>
                <th>Producto</th>
                <th>Cantidad</th>
                <th>Precio</th>
                <th>Total</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in cart" :key="item.id">
                <td>
                  <img :src="item.image" :alt="item.name" width="40" height="40" class="rounded"/>
                </td>
                <td>{{ item.name }}</td>
                <td>
                  <button class="btn btn-outline-secondary btn-sm me-1" @click="$emit('dec', item.id)">
                    <i class="bi bi-dash"></i>
                  </button>
                  <span class="mx-1">{{ item.qty }}</span>
                  <button class="btn btn-outline-secondary btn-sm ms-1" @click="$emit('inc', item.id)">
                    <i class="bi bi-plus"></i>
                  </button>
                </td>
                <td>${{ item.price }}</td>
                <td>${{ item.price * item.qty }}</td>
                <td>
                  <button class="btn btn-outline-danger btn-sm" @click="$emit('remove', item.id)">
                    <i class="bi bi-trash"></i>
                  </button>
                </td>
              </tr>
              <tr>
                <td colspan="4" class="text-end fw-bold">Total general:</td>
                <td colspan="2" class="fw-bold text-primary">${{ total() }}</td>
              </tr>
            </tbody>
          </table>
          <div v-else class="text-center">
            <i class="bi bi-cart-x display-4 text-warning"></i>
            <p class="mt-3">El carrito está vacío.</p>
          </div>
        </div>
        <div class="modal-footer">
          <button v-if="cart.length" class="btn btn-primary" @click="$emit('checkout')">
            <i class="bi bi-credit-card"></i> Pagar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["cart", "total"]
};
</script>
