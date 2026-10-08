<template>
  <section class="productList">
    <div v-if="showSearch" class="searchBar">
      <label class="searchLabel" for="product-search">Поиск курсов</label>
      <input
        id="product-search"
        v-model="searchQuery"
        type="text"
        class="searchInput"
        placeholder="Введите название курса..."
      />
    </div>

    <div v-if="filteredProducts.length" class="catalog">
      <ProductCard
        v-for="product in filteredProducts"
        :key="product.id"
        :product="product"
        @add-to-cart="onAddToCart"
        @buy-now="onBuyNow"
      />
    </div>

    <p v-else class="empty">
      По запросу «{{ searchQuery }}» ничего не найдено.
    </p>
  </section>
</template>

<script>
import ProductCard from './ProductCard.vue'

export default {
  name: 'ProductList',
  components: {
    ProductCard,
  },
  props: {
    products: {
      type: Array,
      required: true,
    },
    showSearch: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  emits: ['add-to-cart', 'buy-now'],
  data() {
    return {
      searchQuery: '',
    }
  },
  computed: {
    filteredProducts() {
      const query = this.searchQuery.trim().toLowerCase()

      if (!query) {
        return this.products
      }

      return this.products.filter((product) => {
        const title = product.title.toLowerCase()
        const description = product.description.toLowerCase()
        return title.includes(query) || description.includes(query)
      })
    },
  },
  methods: {
    onAddToCart(payload) {
      this.$emit('add-to-cart', payload)
    },
    onBuyNow(payload) {
      this.$emit('buy-now', payload)
    },
  },
}
</script>

<style scoped>
.productList {
  width: 100%;
}

.searchBar {
  display: flex;
  flex-direction: column;
  gap: 8px;
  max-width: 420px;
  margin: 0 0 28px;
}

.searchLabel {
  font-size: 14px;
  font-weight: 600;
  color: var(--color-text-muted);
}

.searchInput {
  width: 100%;
  min-height: 44px;
  padding: 10px 16px;
  border-radius: 10px;
  border: 2px solid var(--color-primary);
  background-color: var(--color-surface);
  color: var(--color-text);
  font-size: 15px;
  font-weight: 500;
  outline: none;
}

.searchInput::placeholder {
  color: var(--color-text-muted);
}

.catalog {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  gap: 32px;
}

.empty {
  margin: 48px 0;
  text-align: left;
  color: var(--color-text-muted);
  font-size: 16px;
  font-weight: 500;
}
</style>
