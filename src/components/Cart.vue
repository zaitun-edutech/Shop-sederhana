<template>
  <div>
    <ul class="list-group" v-for="item in data" :key="item">
      <li class="list-group-item">
        <button
          type="button"
          class="close"
          data-dismiss="modal"
          aria-label="Close"
          @click="removeCart(item.id)"
        >
          <span aria-hidden="true">&times;</span>
        </button>
        <div class="media">
          <img :src="item.imgUrl" :alt="item.title" />
          <div class="media-body">
            <p class="mt-0">{{ item.title }}</p>

            <button
              class="btn-qty btn btn-sm btn-secondary"
              @click="removeQty(item.id)"
            >
              -
            </button>
            x {{ item.quantity }}
            <button
              class="btn-qty btn btn-sm btn-secondary"
              @click="addQty(item.id)"
            >
              +
            </button>
          </div>
        </div>
      </li>
    </ul>
    <button
      v-if="data.length"
      class="checkout-button btn btn-lg btn-block btn-success"
    >
      ChekOut ({{ totalPrice.toLocaleString() }})
    </button>
  </div>
</template>
<script setup>
import { computed, onMounted } from "vue-demi";
import { counterStore } from "../store/product";

const counter = counterStore();
const data = computed(() => counter.cart);
// const addCart = (id) => {
//   counter.addToCart(id);
// };
const removeCart = (id) => {
  counter.removeFromCart(id);
};
const addQty = (id) => {
  counter.addQty(id);
};
const removeQty = (id) => {
  counter.removeQty(id);
};
const totalPrice = computed(() => {
  let total = 0;
  data.value.forEach((item) => {
    total += item.price * item.quantity;
  });
  return total;
});
onMounted(() => {
  console.log(data);
});
</script>
<style scoped>
.btn-qty {
  border-radius: 100%;
  width: 30px;
}
.media-body {
  text-align: left;
}
.media {
  width: 90%;
  padding: -30px;
}
img {
  width: 35%;
}
</style>