<template>
  <router-view></router-view>
  <form @submit.prevent  class="calculate">
    <h1 class="calculate__title">Рассчитайте стоимость автомобиля в лизинг</h1>
    <div class="calculate__ranges">
      <div class="calculate__range">
        <label class="calculate__label" for="range-sum">Стоимость автомобиля</label>
        <div class="calculate__number-container">
          <ui-input type="number" id="range-sum" @change="validatePrice" @keydown.enter="validatePrice" oninput="javascript: if (this.value.length > this.maxLength) this.value = this.value.slice(0, this.maxLength);"  maxlength="7" :min="min" :max="max" v-model.number="price"></ui-input>
          <svg class="calculate__number-icon" width="21" height="21" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M11.6774 12.84C20.1074 12.84 20.1074 0.899999 11.6774 0.899999H2.79742V8.64H0.487422V12.84H2.79742V14.13H0.487422V18.33H2.79742V21H7.74742V18.33H14.9174V14.13H7.74742V12.84H11.6774ZM11.2274 5.7C13.3574 5.7 13.3274 8.64 11.2274 8.64H7.74742V5.7H11.2274Z" fill="#575757"/>
          </svg>
          <ui-slider :min="min" :max="max" v-model.number="price"></ui-slider>
        </div>
      </div>
      <div class="calculate__range">
        <label class="calculate__label" for="range-first">Первоначальный взнос</label>
        <div class="calculate__number-container">
          <ui-input :min="minPercent" :max="maxPercent" disabled id="range-sum" v-model.number="specialInitial"></ui-input>
           <svg class="calculate__number-icon--special" width="21" height="21" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M11.6774 12.84C20.1074 12.84 20.1074 0.899999 11.6774 0.899999H2.79742V8.64H0.487422V12.84H2.79742V14.13H0.487422V18.33H2.79742V21H7.74742V18.33H14.9174V14.13H7.74742V12.84H11.6774ZM11.2274 5.7C13.3574 5.7 13.3274 8.64 11.2274 8.64H7.74742V5.7H11.2274Z" fill="#575757"/>
            </svg>
          <input id="percent"  class="calculate__percent" maxlength="2" oninput="javascript: if (this.value.length > this.maxLength) this.value = this.value.slice(0, this.maxLength);" @change="validatePercent" @keydown.enter="validatePercent" :min="minPercent" step="1" :max="minPercent"  v-model="percentage"  type="number">
           <ui-slider :min="minPercent" :max="maxPercent" v-model.number="percentage"></ui-slider>
        </div>
      </div>
      <div class="calculate__range">
      <label class="calculate__label" for="range-time">Срок лизинга</label>
      <div class="calculate__number-container">
        <ui-input @change="validateMonth" @keydown.enter="validateMonth" :min="minMonth" :max="maxMonth" maxlength="2" oninput="javascript: if (this.value.length > this.maxLength) this.value = this.value.slice(0, this.maxLength);" id="range-time" v-model.number="month" type="number"></ui-input>
        <h2 class="calculate__number-icon">
            мес.
        </h2>
        <ui-slider :min="minMonth" :max="maxMonth" v-model.number="month"></ui-slider>
      </div>
    </div>
    </div>
    <div class="calculate__finish">
      <div>
        <h4 class="calculate__sum-title">Сумма договора лизинга</h4>
        <span class="calculate__sum-price">{{ numberWithSpaces(amount) }} ₽</span>
      </div>
      <div>
        <h4 class="calculate__sum-title">Ежемесячный платеж от</h4>
        <span class="calculate__sum-price">{{ numberWithSpaces(calculate) }} ₽</span>

      </div>
      <button type="submit" :disabled='isLoading' @click.prevent="created" class="calculate__button">
        <svg class="calculate__spinner" v-if="isLoading" width="21" height="21" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M13.8691 19.3825C11.9455 20.1121 9.83718 20.1976 7.86093 19.6261C5.88468 19.0546 4.14734 17.857 2.91009 16.2134C1.67285 14.5698 1.00256 12.569 1.00001 10.5118C0.99745 8.45459 1.66276 6.45212 2.89591 4.80546C4.12906 3.15879 5.86342 1.95691 7.83825 1.38051C9.81307 0.804109 11.9216 0.88433 13.847 1.60911C15.7723 2.3339 17.4103 3.66408 18.5148 5.39968C19.6193 7.13528 20.1305 9.18251 19.9716 11.2336" stroke="white" stroke-width="2" stroke-linecap="round"/>
        </svg>
        <span v-else>Оставить заявку</span>
      </button>
    </div>
  </form>
<!--  <div class="ui">-->
<!--    <h1 class="ui__title">Кнопки</h1>-->
<!--    <div class="ui__buttons">-->
<!--      <button class="ui__button">Passive</button>-->
<!--      <button class="ui__button_hover">Hover</button>-->
<!--    </div>-->
<!--  </div>-->
</template>

<script>
import UiInput from "./components/ui-input.vue";
import UiSlider from "./components/ui-slider.vue";

export default {
  components: {UiSlider, UiInput},
  data () {
    return {
      price: 3300000,
      min: 1000000,
      max: 6000000,

      month: 12,
      minMonth: 1,
      maxMonth: 60,

      percentage: 10,
      minPercent: 10,
      maxPercent: 60,

      backgroundSize: '0% 100%',
      isLoading: false,
    }
  },
  methods: {
    numberWithSpaces(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
    },
    validatePrice() {
      if (this.price < this.min) {
        return this.price = this.min;
      } else if (this.price > this.max) {
        return this.price = this.max;
      }
    },
    validatePercent(){
      if (this.percentage > this.maxPercent) {
        return this.percentage = this.maxPercent;
      } else if (this.percentage < this.minPercent) {
        return this.percentage = this.minPercent;
      }
    },
    validateMonth(){
      if (this.month > this.maxMonth) {
        return this.month = this.maxMonth;
      } else if (this.month < this.minMonth) {
        return this.month = this.minMonth;
      }
    },
    async created() {
      let data = {
        pricing: this.price,
        percent: this.percentage,
        month: this.month,
        amount: this.amount,
        perMonth: this.calculate
      }
      const requestOptions = {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({data})
      };
      await fetch('https://eoj3r7f3r4ef6v4.m.pipedream.net', requestOptions)
          .then(response => {
            console.log('sending')
            this.isLoading = true;
            (response.json())
          })
          .catch(() => (this.isLoading = false))
          .finally(() => (this.isLoading = true))
    }

  },
  computed: {
    initial (){
      return ((this.percentage / 100) * this.price).toFixed(0);
    },
    calculate(){
      return Math.floor((this.price - this.initial) * ((0.035 * Math.pow((1 + 0.035), this.month)) / (Math.pow((1 + 0.035), this.month) - 1)));
    },
    amount() {
      return ((this.percentage / 100 +  this.month)  * this.calculate).toFixed(0)
    },
    specialInitial() {
      return (new Intl.NumberFormat('ru-RU').format(this.initial))
    }
  },
}
</script>

<style lang="scss">
@import "scss/global.scss";
@import "scss/style";
@import "scss/ui-kit";
@import "scss/media";
</style>
