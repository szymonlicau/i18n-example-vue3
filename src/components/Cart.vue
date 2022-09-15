<template>
  <div class="cart">
    <h1>{{ locale === 'en' ? 'Your order' : 'Twoje zamówienie' }}</h1>

    <table class="cart__contents">
      <thead>
        <tr>
          <th> # </th>
          <template v-if="locale === 'en'">
            <th> Name </th>
            <th> Each </th>
            <th> Quantity </th>
            <th> Total </th>
          </template>

          <template v-else>
            <th> Nazwa </th>
            <th> Cena </th>
            <th> Ilość </th>
            <th> Suma </th>
          </template>
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
            {{ formatPrice(item.price) }}
          </td>
          <td> {{ item.quantity }} </td>
          <td>
            {{ formatPrice(item.quantity * item.price) }}
          </td>
        </tr>

        <tr>
          <td />
          <td />
          <td />
          <td>
            <b>{{ locale === 'en' ? 'Order Total:' : 'Suma zamówienia:' }}</b>
          </td>

          <td>
            {{ formatPrice(totalCost) }}
          </td>
        </tr>
      </tbody>
    </table>

    <p v-html="arrivalEstimation" />

    <div class="cart__submit">
      <BaseButton
        variant="primary"
      >
        {{ locale === 'en' ? 'Go to payment' : 'Przejdź do płatności' }}
      </BaseButton>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

import BaseButton from '@/components/BaseButton.vue';

const props = withDefaults(defineProps<{
  locale: string;
}>(), {
  locale: 'en'
});

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

const arrivalDate = new Date(2022, 8, 6); // 06.09.2022
/****/

const totalCost = computed(() => {
  return items.reduce((sum, item) => {
    return sum + item.price * item.quantity;
  }, 0);
});

const arrivalEstimation = computed(() => {
  const date = new Intl.DateTimeFormat(props.locale)
    .format(arrivalDate);

  return props.locale === 'en'
    ? `Your order will arrive at: <b>${date}</b>`
    : `Twoje zamówienie dotrze: <b>${date}</b>`;
});


const formatPrice = (price: number) => {
  const convertedValue = props.locale === 'en'
    ? price
    : price * dollarToPln;

  return Intl.NumberFormat(props.locale, {
    style: 'currency',
    currency: props.locale === 'en' ? 'USD' : 'PLN'
  }).format(convertedValue);
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
