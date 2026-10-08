<template>
  <div class="page">
    <AppHeader
      :cart-count="cartCount"
      @toggle-cart="toggleCart"
      @toggle-search="toggleSearch"
    />

    <main class="app">
      <ShoppingCart
        v-if="showCart"
        :items="cartItems"
        @close="showCart = false"
        @remove-item="removeFromCart"
        @update-quantity="updateCartQuantity"
        @checkout="checkout"
      />

      <ProductList
        v-else
        :products="products"
        :show-search="showSearch"
        @add-to-cart="addToCart"
        @buy-now="buyNow"
      />
    </main>

    <p v-if="toastMessage" class="toast">{{ toastMessage }}</p>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import ProductList from './components/ProductList.vue'
import ShoppingCart from './components/ShoppingCart.vue'
import productWebdesign from './assets/product1.jpg'
import productUiux from './assets/product2.jpg'
import productFigma from './assets/product3.png'
import productResponsive from './assets/product4.png'

export default {
  name: 'App',
  components: {
    AppHeader,
    ProductList,
    ShoppingCart,
  },
  data() {
    return {
      showCart: false,
      showSearch: false,
      toastMessage: '',
      toastTimer: null,
      cartItems: [],
      products: [
        {
          id: 1,
          title: 'Основы веб-дизайна',
          description: 'Курс по современному веб-дизайну для начинающих.',
          price: 50,
          rating: 5,
          image: productWebdesign,
        },
        {
          id: 2,
          title: 'UI/UX дизайн PRO',
          description: 'Продвинутый курс по проектированию интерфейсов.',
          price: 79,
          rating: 4,
          image: productUiux,
        },
        {
          id: 3,
          title: 'Figma для дизайнеров',
          description: 'Практика работы в Figma: компоненты и автолейаут.',
          price: 45,
          rating: 5,
          image: productFigma,
        },
        {
          id: 4,
          title: 'Адаптивная вёрстка',
          description: 'Создание адаптивных интерфейсов на HTML и CSS.',
          price: 60,
          rating: 4,
          image: productResponsive,
        },
      ],
    }
  },
  computed: {
    cartCount() {
      return this.cartItems.reduce((sum, item) => sum + item.quantity, 0)
    },
  },
  methods: {
    toggleCart() {
      this.showCart = !this.showCart
    },
    toggleSearch() {
      if (this.showCart) {
        this.showCart = false
      }
      this.showSearch = !this.showSearch
    },
    addToCart({ product, quantity }) {
      const existing = this.cartItems.find((item) => item.id === product.id)

      if (existing) {
        existing.quantity += quantity
      } else {
        this.cartItems.push({
          id: product.id,
          title: product.title,
          price: product.price,
          image: product.image,
          quantity,
        })
      }

      this.showToast(`«${product.title}» добавлен в корзину`)
    },
    buyNow(payload) {
      this.addToCart(payload)
      this.showCart = true
      this.showSearch = false
    },
    removeFromCart(id) {
      this.cartItems = this.cartItems.filter((item) => item.id !== id)
    },
    updateCartQuantity(id, quantity) {
      const item = this.cartItems.find((cartItem) => cartItem.id === id)

      if (!item) {
        return
      }

      if (quantity < 1) {
        this.removeFromCart(id)
        return
      }

      item.quantity = quantity
    },
    checkout() {
      if (!this.cartItems.length) {
        return
      }

      this.showToast('Заказ оформлен! Спасибо за покупку.')
      this.cartItems = []
      this.showCart = false
    },
    showToast(message) {
      this.toastMessage = message

      if (this.toastTimer) {
        clearTimeout(this.toastTimer)
      }

      this.toastTimer = setTimeout(() => {
        this.toastMessage = ''
        this.toastTimer = null
      }, 2500)
    },
  },
  beforeUnmount() {
    if (this.toastTimer) {
      clearTimeout(this.toastTimer)
    }
  },
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background-color: var(--color-bg);
}

.app {
  padding: 32px 64px 80px;
}

.toast {
  position: fixed;
  left: 50%;
  bottom: 28px;
  transform: translateX(-50%);
  margin: 0;
  padding: 12px 20px;
  border-radius: 12px;
  background-color: var(--color-primary);
  color: #ffffff;
  font-size: 14px;
  font-weight: 600;
  box-shadow: 0 10px 30px rgba(139, 92, 246, 0.35);
  z-index: 100;
}

@media (max-width: 768px) {
  .app {
    padding: 24px 20px 64px;
  }
}
</style>
