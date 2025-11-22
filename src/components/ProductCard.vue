<template>
  <div class="col-12 col-sm-6 col-md-4 col-lg-3">
    <div class="card h-100 shadow">
      <img :src="product.image" class="card-img-top" :alt="product.name"
        style="object-fit:contain;height:150px;padding:12px;">
      <div class="card-body d-flex flex-column justify-content-between">
        <h5 class="card-title">{{ product.name }}</h5>
        <p class="card-text fw-bold text-primary mb-2">${{ product.price }}</p>
        <div class="input-group mb-2">
          <button class="btn btn-outline-secondary" @click="decrement">
            <i class="bi bi-dash"></i>
          </button>
          <input type="number" min="1" class="form-control text-center" v-model.number="amount" />
          <button class="btn btn-outline-secondary" @click="increment">
            <i class="bi bi-plus"></i>
          </button>
        </div>
        <button class="btn btn-success w-100" @click="addCart">
          <i class="bi bi-cart-plus"></i> Agregar al carrito
        </button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: ["product"],
  emits: ["add"],
  data() {
    return { amount: 1 };
  },
  methods: {
    addCart() {
      this.$emit("add", this.product, this.amount);
      this.amount = 1;
    },
    increment() { this.amount += 1; },
    decrement() { if (this.amount > 1) this.amount -= 1; }
  }
};
</script>
