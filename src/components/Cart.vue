<template>
  <div class="cart">
    <h1>{{ $t('cart.title') }}</h1>

    <table class="cart__contents">
      <thead>
        <tr>
          <th> # </th>
          <th> {{ $t('cart.productName') }} </th>
          <th> {{ $t('cart.productPrice') }} </th>
          <th> {{ $t('cart.productQuantity') }} </th>
          <th> {{ $t('cart.productTotal') }} </th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="(item, index) in items"
          :key="item.id"
        >
          <td> {{ index + 1 }} </td>
          <td> {{ item.name }} </td>
          <td>
            {{ $n(convertPrice(item.price), 'currency') }}
          </td>
          <td> {{ item.quantity }} </td>
          <td>
            {{ $n(convertPrice(item.quantity * item.price), 'currency') }}
          </td>
        </tr>

        <tr>
          <td />
          <td />
          <td />
          <td>
            <b> {{ $t('cart.orderTotal') }} </b>
          </td>

          <td>
            {{ $n(convertPrice(totalCost), 'currency') }}
          </td>
        </tr>
      </tbody>
    </table>

    <p v-html="arrivalEstimation" />

    <div class="cart__submit">
      <BaseButton
        variant="primary"
      >
        {{ $t('cart.goToPayment') }}
      </BaseButton>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { useI18n } from 'vue-i18n';

import BaseButton from '@/components/BaseButton.vue';

/**
 *  MOCKED 'API' DATA
 */
const dollarToPln = 4.3; //totally correct & up-to-date exchange rate
const items = [
  {
    id: 1,
    name: 'Vue T-Shirt - XL',
    quantity: 2,
    price: 19.99
  },
  {
    id: 2,
    name: 'Duck plushie',
    quantity: 12,
    price: 14.99
  },
  {
    id: 3,
    name: 'Vue coffee mug - white',
    quantity: 1,
    price: 9.99
  }
];

const i18n = useI18n();

const arrivalDate = new Date(2022, 8, 6); // 06.09.2022
/****/

const totalCost = computed(() => {
  return items.reduce((sum, item) => {
    return sum + item.price * item.quantity;
  }, 0);
});

const arrivalEstimation = computed(() => {
  const date = i18n.d(arrivalDate);

  return i18n.t('cart.arrivalDate', { date });
});

const convertPrice = (price: number) => {
  return i18n.locale.value === 'en'
    ? price
    : price * dollarToPln;
};
</script>

<style lang="scss">
.cart {
  th {
    text-align: left;
    background-color: var(--text-color);
    color: var(--white);
  }

  th, td {
    padding: 0.4rem;
    border: 0.1rem solid var(--border-color);

    &:empty {
      border: none;
    }
  }

  &__contents {
    width: 100%;
    border-spacing: 0;
    border-collapse: collapse;
  }

  &__submit {
    text-align: right;
  }
}
</style>
