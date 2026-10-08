<template>
  <section class="cart">
    <div class="cartHeader">
      <h2 class="cartTitle">Корзина</h2>
      <button type="button" class="closeButton" @click="$emit('close')">
        Закрыть
      </button>
    </div>

    <div v-if="items.length" class="cartContent">
      <ul class="itemList">
        <li v-for="item in items" :key="item.id" class="item">
          <img :src="item.image" :alt="item.title" class="itemImage" />

          <div class="itemInfo">
            <h3 class="itemTitle">{{ item.title }}</h3>
            <p class="itemMeta">{{ item.price }}$ × {{ item.quantity }}</p>
            <p class="itemTotal">Итого: {{ item.price * item.quantity }}$</p>
          </div>

          <div class="itemActions">
            <div class="selector">
              <button
                type="button"
                class="qtyButton"
                :disabled="item.quantity <= 1"
                @click="$emit('update-quantity', item.id, item.quantity - 1)"
              >
                −
              </button>
              <span class="value">{{ String(item.quantity).padStart(2, '0') }}</span>
              <button
                type="button"
                class="qtyButton"
                @click="$emit('update-quantity', item.id, item.quantity + 1)"
              >
                +
              </button>
            </div>

            <button
              type="button"
              class="button outline removeButton"
              @click="$emit('remove-item', item.id)"
            >
              Удалить
            </button>
          </div>
        </li>
      </ul>

      <div class="summary">
        <p class="summaryText">Всего товаров: {{ totalQuantity }}</p>
        <p class="summaryTotal">Сумма: {{ totalPrice }}$</p>
        <button
          type="button"
          class="button primary checkoutButton"
          :class="{ disabled: items.length === 0 }"
          :disabled="items.length === 0"
          @click="$emit('checkout')"
        >
          Оформить заказ
        </button>
      </div>
    </div>

    <div v-else class="emptyCart">
      <p class="emptyTitle">Корзина пуста</p>
      <p class="emptyText">Добавьте курсы с витрины, чтобы оформить заказ.</p>
      <button type="button" class="button primary" @click="$emit('close')">
        К каталогу
      </button>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ShoppingCart',
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  emits: ['close', 'remove-item', 'update-quantity', 'checkout'],
  data() {
    return {}
  },
  computed: {
    totalQuantity() {
      return this.items.reduce((sum, item) => sum + item.quantity, 0)
    },
    totalPrice() {
      return this.items.reduce(
        (sum, item) => sum + item.price * item.quantity,
        0,
      )
    },
  },
}
</script>

<style scoped>
.cart {
  width: 100%;
  max-width: 900px;
  margin: 0 auto;
  padding: 0 64px 48px;
}

.cartHeader {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24px;
}

.cartTitle {
  margin: 0;
  font-size: 28px;
  font-weight: 700;
  color: var(--color-text);
}

.closeButton {
  background: transparent;
  border: none;
  color: var(--color-primary);
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
}

.itemList {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.item {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 16px;
  background-color: var(--color-surface);
  border-radius: 16px;
}

.itemImage {
  width: 96px;
  height: 72px;
  object-fit: cover;
  border-radius: 10px;
  flex-shrink: 0;
}

.itemInfo {
  flex: 1;
  min-width: 0;
}

.itemTitle {
  margin: 0 0 6px;
  font-size: 18px;
  font-weight: 700;
  color: var(--color-text);
}

.itemMeta,
.itemTotal {
  margin: 0;
  font-size: 14px;
  color: var(--color-text-muted);
}

.itemTotal {
  margin-top: 4px;
  color: var(--color-text);
  font-weight: 600;
}

.itemActions {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 10px;
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
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  color: var(--color-text);
}

.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
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

.disabled,
.button:disabled {
  background-color: var(--color-inactive);
  color: #000000;
  border-color: var(--color-inactive);
  cursor: not-allowed;
}

.primary.disabled,
.primary:disabled {
  color: var(--color-text);
}

.removeButton {
  min-height: 36px;
  padding: 6px 12px;
  font-size: 13px;
}

.summary {
  margin-top: 24px;
  padding: 20px;
  background-color: var(--color-surface);
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.summaryText,
.summaryTotal {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
}

.summaryTotal {
  font-size: 22px;
  margin-bottom: 8px;
}

.checkoutButton {
  width: 100%;
}

.emptyCart {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  padding: 48px 20px;
  text-align: center;
  background-color: var(--color-surface);
  border-radius: 20px;
}

.emptyTitle {
  margin: 0;
  font-size: 22px;
  font-weight: 700;
}

.emptyText {
  margin: 0 0 12px;
  color: var(--color-text-muted);
  font-size: 15px;
}

@media (max-width: 768px) {
  .cart {
    padding: 0 20px 32px;
  }

  .item {
    flex-direction: column;
    align-items: stretch;
  }

  .itemActions {
    align-items: stretch;
  }

  .removeButton {
    width: 100%;
  }
}
</style>
