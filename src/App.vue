<template>
  <div>
    <nav class="navbar navbar-light bg-light mb-4">
      <div class="container-fluid">
        <span class="navbar-brand mb-0 h1 d-flex align-items-center">
          <i class="bi bi-phone me-2"></i> Market1.0
        </span>
        <input
          type="text"
          v-model="search"
          class="form-control w-50 mx-2"
          placeholder="Buscar celular..."
        />
        <button class="btn btn-outline-primary position-relative" @click="toggleCart">
          <i class="bi bi-cart3"></i>
          <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">
            {{ cart.length }}
          </span>
        </button>
      </div>
    </nav>
    <ProductList :search="search" @add="addToCart" />
    <CartComponent
      v-if="showCart"
      :cart="cart"
      @inc="increase"
      @dec="decrease"
      @remove="remove"
      :total="total"
      @close="toggleCart"
      @checkout="goCheckout"
    />
    <NotifyModal v-if="notify.show" :product="notify.product" :amount="notify.amount" @close="closeNotify" />
    <CheckoutPage
      v-if="checkingOut"
      :cart="cart"
      :total="total()"
      @confirm="confirmOrder"
      @close="checkingOut=false"
    />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import ProductList from "./components/ProductList.vue";
import CartComponent from "./components/CartComponent.vue";
import NotifyModal from "./components/NotifyModal.vue";
import CheckoutPage from "./components/CheckoutPage.vue";

const CART_KEY = "market1_cart";
export default {
  components: { ProductList, CartComponent, NotifyModal, CheckoutPage },
  setup() {
    const cart = ref([]);
    const showCart = ref(false);
    const notify = ref({ show: false, product: null, amount: 1 });
    const checkingOut = ref(false);
    const search = ref("");

    onMounted(() => {
      let saved = localStorage.getItem(CART_KEY);
      cart.value = saved ? JSON.parse(saved) : [];
    });

    const persist = () => localStorage.setItem(CART_KEY, JSON.stringify(cart.value));

    const addToCart = (product, amount) => {
      let item = cart.value.find(x => x.id === product.id);
      if (item) item.qty += amount;
      else cart.value.push({ ...product, qty: amount });
      persist();
      notify.value = { show: true, product, amount };
    };

    const increase = (id) => {
      let item = cart.value.find(x => x.id === id);
      if (item) item.qty += 1;
      persist();
    };

    const decrease = (id) => {
      let item = cart.value.find(x => x.id === id);
      if (item && item.qty > 1) item.qty -= 1;
      persist();
    };

    const remove = (id) => {
      cart.value = cart.value.filter(x => x.id !== id);
      persist();
    };

    const total = () => cart.value.reduce((sum, x) => sum + x.qty * x.price, 0);
    const toggleCart = () => showCart.value = !showCart.value;
    const closeNotify = () => notify.value = { show: false, product: null, amount: 1 };

    const goCheckout = () => {
      showCart.value = false;
      checkingOut.value = true;
    };

    const confirmOrder = (data) => {
      cart.value = [];
      persist();
      checkingOut.value = false;
      alert("Pedido confirmado");
    };

    return {
      cart, addToCart, increase, decrease, remove, total, showCart,
      toggleCart, notify, closeNotify, search, checkingOut, goCheckout, confirmOrder
    };
  }
};
</script>
