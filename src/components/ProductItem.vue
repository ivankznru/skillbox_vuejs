<template>
<div>
<router-link class="catalog__pic" :to="{name: 'product', params: {id: product.id}}">
              <img :src="product.image" :alt="product.title">
</router-link>

            <h3 class="catalog__title">
              <a href="#">
                {{ product.title }}
              </a>
            </h3>

            <span class="catalog__price">
              {{product.price | numberFormat}} ₽
            </span>

            <ul class="colors colors--black">
              <li class="colors__item" v-for="color in product.colors" :key="color.id">
                <label class="colors__label">
                  <input class="colors__radio sr-only" name="color" type="radio">
                  <span class="colors__value" :style="{'background-color': color.code}" >
                  </span>
                </label>
              </li>
            </ul>
</div>
</template>
<script>
import colorsData from './ProductFilter.vue';
import gotoPage from '@/helpers/gotoPage';
import numberFormat from '@/helpers/numberFormat';

export default {
  props: ['product'],
  filters: {
    numberFormat,
  },
  computed: {
    colors() {
      return colorsData;
    },
  },
  methods: {
    colorconnect(colorI) {
      return colorsData.find((e) => e.id === colorI)?.title;
    },
    gotoPage,
  },
};
</script>
