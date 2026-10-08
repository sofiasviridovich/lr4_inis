<template>
  <article class="card">
    <img
      v-if="product.image"
      :src="product.image"
      :alt="product.title"
      class="image"
    />
    <div v-else class="imagePlaceholder"></div>

    <div class="content">
      <h2 class="title">{{ product.title }}</h2>
      <p class="description">{{ product.description }}</p>

      <div class="rating" aria-label="Рейтинг">
        <img
          v-for="star in 5"
          :key="star"
          :src="star <= product.rating ? starFilled : starEmpty"
          alt=""
          class="star"
        />
      </div>

      <div class="priceRow">
        <span class="price">{{ product.price }}$</span>

        <div class="selector">
          <button
            type="button"
            class="qtyButton"
            :disabled="quantity <= 1"
            @click="decrease"
          >
            −
          </button>
          <span class="value">{{ paddedQuantity }}</span>
          <button type="button" class="qtyButton" @click="increase">+</button>
        </div>
      </div>
    </div>

    <div class="actions">
      <button type="button" class="button outline" @click="addToCart">
        Add to Cart
      </button>
      <button type="button" class="button primary" @click="buyNow">
        Buy Now
      </button>
    </div>
  </article>
</template>

<script>
import starFilled from '../assets/star.svg'
import starEmpty from '../assets/star-outline.svg'

export default {
  name: 'ProductCard',
  props: {
    product: {
      type: Object,
      required: true,
      validator(value) {
        return (
          value &&
          typeof value.id !== 'undefined' &&
          typeof value.title === 'string' &&
          typeof value.price === 'number'
        )
      },
    },
  },
  emits: ['add-to-cart', 'buy-now'],
  data() {
    return {
      quantity: 1,
      starFilled,
      starEmpty,
    }
  },
  computed: {
    paddedQuantity() {
      return String(this.quantity).padStart(2, '0')
    },
  },
  methods: {
    increase() {
      this.quantity += 1
    },
    decrease() {
      if (this.quantity > 1) {
        this.quantity -= 1
      }
    },
    addToCart() {
      this.$emit('add-to-cart', {
        product: this.product,
        quantity: this.quantity,
      })
    },
    buyNow() {
      this.$emit('buy-now', {
        product: this.product,
        quantity: this.quantity,
      })
    },
  },
}
</script>

<style scoped>
.card {
  width: 320px;
  background-color: var(--color-surface);
  border-radius: 20px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 12px;
}

.imagePlaceholder {
  width: 100%;
  height: 200px;
  background-color: var(--color-image-placeholder);
  border-radius: 12px;
}

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  text-align: center;
}

.title {
  margin: 0;
  font-family: var(--product-name-h2-font-family);
  font-size: var(--product-name-h2-font-size);
  font-style: var(--product-name-h2-font-style);
  font-weight: var(--product-name-h2-font-weight);
  letter-spacing: var(--product-name-h2-letter-spacing);
  line-height: var(--product-name-h2-line-height);
  color: var(--color-text);
}

.description {
  margin: 0;
  font-family: var(--font-family);
  font-size: 14px;
  font-weight: 500;
  line-height: 1.4;
  color: var(--color-text-muted);
  min-height: 40px;
}

.rating {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
}

.star {
  width: 20px;
  height: 20px;
}

.priceRow {
  display: inline-flex;
  align-items: center;
  gap: 42px;
  position: relative;
  margin-top: 8px;
}

.price {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex: 0 0 auto;
  gap: 10px;
  color: #ffffff;
  font-family: var(--product-name-h2-font-family);
  font-size: var(--product-name-h2-font-size);
  font-style: var(--product-name-h2-font-style);
  font-weight: var(--product-name-h2-font-weight);
  letter-spacing: var(--product-name-h2-letter-spacing);
  line-height: var(--product-name-h2-line-height);
  text-align: center;
  white-space: nowrap;
  width: fit-content;
}

.selector {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.qtyButton {
  width: 36px;
  height: 36px;
  background-color: #1a1a1a;
  border: none;
  border-radius: 8px;
  color: var(--color-text);
  font-size: 18px;
  font-weight: 500;
  line-height: 1;
  cursor: pointer;
  transition: background-color 0.15s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.qtyButton:hover:not(:disabled) {
  background-color: #333333;
}

.qtyButton:disabled {
  color: #555555;
  cursor: not-allowed;
}

.value {
  min-width: 48px;
  height: 36px;
  padding: 0 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: transparent;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  color: var(--color-text);
}

.actions {
  display: flex;
  gap: 10px;
  width: 100%;
  margin-top: 6px;
}

.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex: 1;
  min-height: 44px;
  padding: 10px 16px;
  border-radius: 10px;
  font-family: var(--font-family);
  font-size: 15px;
  font-weight: 600;
  line-height: 1.2;
  cursor: pointer;
  transition: background-color 0.15s ease, color 0.15s ease, border-color 0.15s ease;
  white-space: nowrap;
  border: 2px solid transparent;
}

.primary {
  background-color: var(--color-primary);
  color: var(--color-text);
  border-color: var(--color-primary);
}

.primary:hover:not(:disabled) {
  background-color: var(--color-primary-hover);
  border-color: var(--color-primary-hover);
}

.outline {
  background-color: transparent;
  color: var(--color-primary);
  border-color: var(--color-primary);
}

.outline:hover:not(:disabled) {
  background-color: var(--color-primary);
  color: #000000;
}

@media (max-width: 480px) {
  .card {
    width: 100%;
  }

  .actions {
    flex-direction: column;
  }

  .priceRow {
    gap: 24px;
  }
}
</style>
