<template>
  <div>
    <h2>Produtos</h2>
    <div class="product-container">
      <div v-for="product in products" :key="product.id" class="product-item">
        <div class="product-info">
          <span class="product-name">{{ product.name }}</span>
          <span class="product-price">R$ {{ product.price.toFixed(2) }}</span>
        </div>
        <div class="quantity-controls">
          <button @click="decreaseQuantity(product)">-</button>
          <span>{{ product.quantity }}</span>
          <button @click="increaseQuantity(product)">+</button>
        </div>
        <button @click="addToCart(product)">Adicionar ao carrinho</button>
      </div>
    </div>

    <button @click="toggleCart" v-if="!showCart">
      Ver Carrinho ({{ getCartQuantity() }})
    </button>

    <h2 v-if="showCart">Carrinho de Compras</h2>
    <div v-if="showCart && cart.length === 0">
      Carrinho vazio.
    </div>
    <div v-if="showCart && cart.length > 0">
      <div class="cart-items">
        <div v-for="item in cart" :key="item.product.id" class="cart-item">
          <div class="item-info">
            <span class="item-name">{{ item.product.name }}</span>
            <div class="quantity-controls">
              <button @click="decreaseQuantity(item)">-</button>
              <span>{{ item.quantity }}</span>
              <button @click="increaseQuantity(item)">+</button>
            </div>
          </div>
          <button @click="removeFromCart(item)">Remover</button>
        </div>
      </div>
      <div class="cart-total">
        Total: R$ {{ calculateTotal() }}
        <button @click="clearCart">Limpar Carrinho</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        { id: 1, name: 'Camiseta', price: 49.90, quantity: 1 },
        { id: 2, name: 'Calça', price: 99.90, quantity: 1 },
        { id: 3, name: 'Meia', price: 9.90, quantity: 1 },
        { id: 4, name: 'Sapato', price: 199.90, quantity: 1 },
        { id: 5, name: 'Boné', price: 29.90, quantity: 1 },
        { id: 6, name: 'Óculos', price: 99.90, quantity: 1 },
        { id: 7, name: 'Relógio', price: 299.90, quantity: 1 },
        { id: 8, name: 'Bermuda', price: 79.90, quantity: 1 },
        { id: 9, name: 'Cueca', price: 19.90, quantity: 1 },
      ],
      cart: [],
      showCart: false,
    };
  },
  methods: {
    addToCart(product) {
      const existingItem = this.cart.find(item => item.product.id === product.id);
      if (existingItem) {
        existingItem.quantity += product.quantity;
      } else {
        this.cart.push({ product: product, quantity: product.quantity });
      }
      
      product.quantity = 1;
    },
    increaseQuantity(item) {
      item.quantity++;
    },
    decreaseQuantity(item) {
      if (item.quantity > 1) {
        item.quantity--;
      }
    },
    removeFromCart(item) {
      const index = this.cart.indexOf(item);
      this.cart.indexOf(item);
      this.cart.splice(index, 1);
    },
    calculateItemTotal(item) {
      return (item.product.price * item.quantity).toFixed(2);
    },
    calculateTotal() {
      let total = 0;
      for (const item of this.cart) {
        total += item.product.price * item.quantity;
      }
      return total.toFixed(2);
    },
    toggleCart() {
      this.showCart = !this.showCart;
    },
    getCartQuantity() {
      let totalQuantity = 0;
      for (const item of this.cart) {
        totalQuantity += item.quantity;
      }
      return totalQuantity;
    },
    clearCart() {
      this.cart = [];
    },
  },
};
</script>

<style scoped>
.product-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.product-item {
  flex-basis: calc(33.33% - 20px);
}

.cart-container {
  margin-top: 20px;
}

.cart-items {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.cart-item {
  flex-basis: calc(33.33% - 20px);
  color: #000000;
}
</style>