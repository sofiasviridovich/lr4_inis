<template>
  <header class="header">
    <div class="brand">
      <img :src="logoSrc" alt="DigitalStore" class="logoImage" />
      <span class="logoText">DigitalStore</span>
    </div>

    <nav class="nav">
      <button
        class="iconButton"
        type="button"
        aria-label="Поиск"
        @click="$emit('toggle-search')"
      >
        <img :src="searchSrc" alt="" class="icon" />
      </button>

      <button
        class="iconButton"
        type="button"
        aria-label="Корзина"
        @click="$emit('toggle-cart')"
      >
        <img :src="cartSrc" alt="" class="icon" />
        <span v-if="cartCount > 0" class="badge">{{ cartCount }}</span>
      </button>

      <button class="iconButton" type="button" aria-label="Профиль">
        <img :src="userSrc" alt="" class="icon" />
      </button>
    </nav>
  </header>
</template>

<script>
import logoSrc from '../assets/logo.svg'
import searchSrc from '../assets/search.png'
import cartSrc from '../assets/cart.png'
import userSrc from '../assets/user.png'

export default {
  name: 'AppHeader',
  props: {
    cartCount: {
      type: Number,
      required: true,
    },
  },
  emits: ['toggle-cart', 'toggle-search'],
  data() {
    return {
      logoSrc,
      searchSrc,
      cartSrc,
      userSrc,
    }
  },
}
</script>

<style scoped>
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 33px 64px;
  background-color: var(--color-bg);
}

.brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
}

.logoImage {
  width: 64px;
  height: 64px;
  object-fit: contain;
  display: block;
}

.logoText {
  color: #ffffff;
  font-family: var(--header-h1-font-family);
  font-size: var(--header-h1-font-size);
  font-style: var(--header-h1-font-style);
  font-weight: var(--header-h1-font-weight);
  letter-spacing: var(--header-h1-letter-spacing);
  line-height: var(--header-h1-line-height);
  text-align: center;
  white-space: nowrap;
  width: fit-content;
}

.nav {
  display: inline-flex;
  align-items: center;
}

.iconButton {
  align-items: center;
  display: inline-flex;
  flex: 0 0 auto;
  gap: 10px;
  padding: 10px;
  background: transparent;
  border: none;
  cursor: pointer;
  position: relative;
}

.icon {
  width: 50px;
  height: 50px;
  object-fit: cover;
  position: relative;
  display: block;
}

.badge {
  position: absolute;
  top: 6px;
  right: 4px;
  min-width: 20px;
  height: 20px;
  padding: 0 5px;
  border-radius: 999px;
  background-color: var(--color-primary);
  color: #ffffff;
  font-size: 12px;
  font-weight: 700;
  line-height: 20px;
  text-align: center;
}

@media (max-width: 768px) {
  .header {
    padding: 16px 20px;
  }

  .logoImage {
    width: 40px;
    height: 40px;
  }

  .logoText {
    font-size: 20px;
  }

  .icon {
    width: 32px;
    height: 32px;
  }

  .iconButton {
    padding: 6px;
  }
}
</style>
