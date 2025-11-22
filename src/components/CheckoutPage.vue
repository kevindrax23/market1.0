<template>
  <div class="modal fade show d-block bg-dark bg-opacity-25" tabindex="-1" aria-modal="true" role="dialog">
    <div class="modal-dialog modal-md modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title text-primary"><i class="bi bi-credit-card"></i> Pagar y envío</h5>
          <button type="button" class="btn-close" @click="$emit('close')"></button>
        </div>
        <form @submit.prevent="confirm">
          <div class="modal-body">
            <div class="mb-2">
              <label class="form-label">Nombre completo</label>
              <input type="text" v-model="form.name" class="form-control" required />
            </div>
            <div class="mb-2">
              <label class="form-label">Correo</label>
              <input type="email" v-model="form.email" class="form-control" required />
            </div>
            <div class="mb-2">
              <label class="form-label">Dirección de envío</label>
              <input type="text" v-model="form.address" class="form-control" required />
            </div>
            <div class="mb-2">
              <label class="form-label">Teléfono</label>
              <input type="tel" v-model="form.phone" class="form-control" required />
            </div>
            <div class="border-top pt-2 mt-2">
              <b>Productos en el pedido:</b>
              <ul>
                <li v-for="item in cart" :key="item.id">{{ item.name }} x{{ item.qty }} - ${{ item.price * item.qty }}</li>
              </ul>
              <div class="fw-bold text-end">Total: ${{ total }}</div>
            </div>
          </div>
          <div class="modal-footer">
            <button class="btn btn-success" type="submit">
              <i class="bi bi-check2-circle"></i> Confirmar pedido
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
export default {
  props: ["cart", "total"],
  setup(props, { emit }) {
    const form = ref({ name: "", email: "", address: "", phone: "" });
    const confirm = () => emit("confirm", form.value);
    return { form, confirm };
  }
};
</script>
