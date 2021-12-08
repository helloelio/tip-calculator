<script lang="js">
import { gsap } from 'gsap';
import { CSSPlugin } from 'gsap/CSSPlugin';
gsap.registerPlugin(CSSPlugin);

export default {
  data: () => ({
    totalPerPerson: 12.02,
    numberOfPeople: 0,
    check: '5',
    tipAmount: 0,
    tweenedTipAmount: 0,
    billAmount: 0,
    tweenedBillAmount: 0,
  }),

  computed: {
    animatedNumber() {
      return this.tweenedBillAmount.toFixed(2);
    },
    animatedTipAmount() {
      return this.tweenedTipAmount.toFixed(2);
    },
  },

  watch: {
    billAmount(newValue) {
      gsap.to(this.$data, {
        duration: 0.5,
        tweenedBillAmount: newValue,
      });
    },
    tipAmount(newValue) {
      this.billAmount = this.billAmount + this.tipAmount;
      gsap.to(this.$data, {
        duration: 0.5,
        tweenedTipAmount: newValue,
      });
    },
  },
  methods: {
    handleCalculate() {
      this.billAmount = this.handleBillAmount();
      this.tipAmount = this.handleTipAmount();
    },
    handleTipAmount() {
      return (this.billAmount * +this.check) / 100;
    },
    handleBillAmount() {
      return this.totalPerPerson * this.numberOfPeople;
    },
  },
};
</script>

<template>
  <div
    class="flex-auto shadow-md text-m rounded-xl bg-white w-6/12 m-auto mt-28"
  >
    <div class="flex items-center justify-center font-thin p-10 border-b-2">
      Tip Amount:
      <div class="flex items-start ml-8">
        <img src="./assets/dollar.svg" alt="" />
        <span class="text-7xl font-bold">{{ animatedTipAmount }}</span>
      </div>
    </div>
    <div class="flex font-thin items-center justify-center p-10">
      Total Per Person
      <div class="flex items-start ml-8">
        <img src="./assets/dollar.svg" alt="" />
        <span class="text-7xl font-bold"> 12.02</span>
      </div>
    </div>
    <div class="flex justify-center">
      <div class="flex text-m items-center justify-between w-10/12">
        <div class="w-80">
          <div class="flex-auto items-center">
            <div
              class="
                flex
                items-center
                justify-center
                border-dotted border-b-4
                p-5
              "
            >
              <img src="./assets/dollar.svg" alt="" />
              <span class="text-7xl font-bold" id="animated-number-demo">{{
                animatedNumber
              }}</span>
            </div>
            <div class="font-thin p-5">Bill Amount</div>
          </div>
        </div>
        <div class="w-50">
          <div class="flex-auto items-center">
            <div
              class="
                flex
                items-center
                justify-center
                border-dotted border-b-4
                p-5
              "
            >
              <img src="./assets/people.svg" alt="" />
              <input
                type="text"
                class="text-7xl font-bold w-20 border-b-2"
                maxlength="2"
                v-model="numberOfPeople"
                @keydown.enter="handleCalculate"
              />
            </div>
            <div class="font-thin p-5">Number of People</div>
          </div>
        </div>
      </div>
    </div>
    <div
      class="
        text-xl
        font-bold
        justify-around
        bg-gray-200
        p-5
        border-t-2 border-b-2
      "
    >
      <form class="flex justify-around" action="">
        <label class="lab">
          <input
            v-model="check"
            name="radio"
            type="radio"
            value="5"
            id="radio-1"
            checked
          />
          <span class="checkmark rounded-xl">5%</span>
        </label>
        <label class="lab">
          <input
            v-model="check"
            name="radio"
            type="radio"
            value="10"
            id="radio-2"
          />
          <span class="checkmark rounded-xl">10%</span>
        </label>
        <label class="lab">
          <input
            v-model="check"
            name="radio"
            type="radio"
            value="15"
            id="radio-3"
          />
          <span class="checkmark rounded-xl">15%</span>
        </label>
        <label class="lab">
          <input
            v-model="check"
            name="radio"
            type="radio"
            value="20"
            id="radio-4"
          />
          <span class="checkmark rounded-xl">20%</span>
        </label>
      </form>
    </div>
    <div class="flex text-xl font-bold items-end justify-end mr-10">
      <button
        class="
          p-3
          px-20
          rounded-xl
          m-5
          color
          bg-red-400
          text-white
          active:bg-red-700
          hover:bg-red-500
        "
        @click="handleCalculate"
      >
        Calculate
      </button>
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@700&family=Roboto:wght@700&display=swap');
body {
  background: #f6f6f3;
}
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333;
}
span {
  font-family: 'Inter', sans-serif;
}
/* Customize the label (the container) */
.lab {
  display: block;
  position: relative;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default radio button */
.lab input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom radio button */
.checkmark {
  padding: 10px 30px;
  background-color: #fff;
  color: #60c1b6;
}

/* On mouse-over, add a grey background color */
.lab:hover input ~ .checkmark {
  background-color: #eee;
}

/* When the radio button is checked, add a blue background */
.lab input:checked ~ .checkmark {
  background-color: #60c1b6;
  color: #fff;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.checkmark:after {
  content: '';
  position: absolute;
  display: none;
}

/* Style the indicator (dot/circle) */
.lab .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: white;
}
</style>
