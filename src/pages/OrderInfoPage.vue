<template>
<div>
  <main v-if="loading">Загрузка...</main>
  <main v-else-if="loadingFailed">Ошибка...</main>
    <main v-else class="content container">
    <div class="content__top">
      <ul class="breadcrumbs">
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="index.html">
            Каталог
          </a>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="cart.html">
            Корзина
          </a>
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link">
            Оформление заказа
          </a>
        </li>
      </ul>

      <h1 class="content__title">
        Заказ оформлен <span>№ 23621</span>
      </h1>
    </div>
    <section class="cart">
      <form class="cart__form form" action="#" method="POST">
        <div class="cart__field">
          <p class="cart__message">
            Благодарим за&nbsp;выбор нашего магазина. На&nbsp;Вашу почту придет письмо с&nbsp;деталями заказа.
            Наши менеджеры свяжутся с&nbsp;Вами в&nbsp;течение часа для уточнения деталей доставки.
          </p>

          <ul class="dictionary">
            <li class="dictionary__item">
              <span class="dictionary__key">
                Получатель
              </span>
              <span class="dictionary__value">
                {{orderInfoProducts.name}}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Адрес доставки
              </span>
              <span class="dictionary__value">
                {{orderInfoProducts.address}}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Телефон
              </span>
              <span class="dictionary__value">
                {{orderInfoProducts.phone}}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Email
              </span>
              <span class="dictionary__value">
                {{orderInfoProducts.email}}
              </span>
            </li>
            <li class="dictionary__item">
              <span class="dictionary__key">
                Способ оплаты
              </span>
              <span class="dictionary__value">
                картой при получении
              </span>
            </li>
          </ul>
        </div>

        <div class="cart__block">
          <ul class="cart__orders">
            <li class="cart__order" v-for="item in orderInfoProducts.basket.items" :key="item.product.id">
                <h3>{{item.product.title}}</h3>
              <b>{{item.product.price | numberFormat}} ₽</b>
              <span>Артикул: {{item.product.id}}</span>
            </li>
          </ul>
          <div class="cart__total">
            <p>Доставка: <b>500 ₽</b></p>
            <p>Итого: <b>{{amount}}</b> товара на сумму <b>{{totalPrice | numberFormat}} ₽</b></p>
          </div>
        </div>
      </form>
    </section>
  </main>
</div>
</template>

<script>
import { mapGetters } from 'vuex';
import numberFormat from '@/helpers/numberFormat';

export default {
  data() {
    return {
      loading: true,
      loadingFailed: false,
    };
  },
  filters: { numberFormat },
  created() {
    this.loading = true;
    this.loadingFailed = false;
    if (this.$store.state.orderInfo && this.$store.state.orderInfo.id === this.$route.params.id) {
      this.loading = false;

      return;
    }
    this.$store.dispatch('loadOrderInfo', this.$route.params.id).catch(() => {
      this.loadingFailed = true;
    }).then(() => {
      this.loading = false;
    });
  },
  computed: {
    ...mapGetters(['orderInfoProducts', 'orderInfoPrice', 'orderTotalProducts']),
    amount() {
      return this.orderTotalProducts;
    },
    totalPrice() {
      return this.orderInfoPrice;
    },
  },
};
</script>
