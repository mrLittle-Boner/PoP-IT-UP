<template>
  <div class="modal" @click.self="$emit('close'), clearFrom()">
    <div class="content">
      <button class="close"  @click="$emit('close'), clearFrom()">
        <img :src="closeButton" alt="close window">
      </button>
      <h3 class="title">Налоговый вычет</h3>
      <p class="descr">Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13% от своего официального годового дохода.</p>
      <form @submit.prevent="calculateTaxDeduction()">
        <label class="salary"> Ваша зарплата в месяц
          <input 
            class="input" 
            placeholder="Введите данные" 
            type="number"
            v-model="salary"
          >
        </label>
        <button class="calculate" type="submit">Рассчитать</button>
      </form>

      <div class="result" v-show="renderedDeduction.length > 0">
        <span class="result__text" >И того можете внести в качестве досрочных:</span>
        <label v-for="result,index in renderedDeduction"
          :key="index"
          class="result__item" 
        >
          <input class="checkbox" type="checkbox">
          <span class="custom-check"></span>
          <span class="result__payment">
            {{result}} рублей 
            <span class="result_year">
              в {{index + 1}}-{{computedPostfix(index+1)}} год
            </span>
          </span>
        </label>
      </div>

      <div class="profit">
        <span class="choice">Что уменьшаем?</span>
        <div class="tags">
          <label class="tag" :class="{ 'tag__cheked' : radioValue == 'payment' }">
            <input type="radio" @change="radioValue = 'payment'" name="tags" value="payment">
            Платёж
          </label>
          <label class="tag" :class="{ 'tag__cheked' : radioValue == 'time' }">
            <input type="radio" @change="radioValue = 'time'" name="tags" value="time">
            Срок
          </label>
        </div>
      </div>

      <button class="addbtn button-bs">Добавить</button>
    </div>
  </div>
</template>

<script>
import closeBtn from '../assets/icon/close-btn.svg';
export default {
  name: 'Calculator',
  data() {
    return {
      calculatedResult: false,
      radioValue: 'payment',
      closeButton: closeBtn,
      salary: '',
      renderedDeduction: []
    }
  },
  methods: {
    calculateTaxDeduction() {
      let resultDeduction = [];
      if( this.salary >= 12500) {
        let taxDeduction = (this.salary*12)*0.13;
        let maxAmount = 260000;

        let iteration = Math.ceil(maxAmount/taxDeduction); 

        for(let i = iteration; i > 0 ; i--) {
          if(maxAmount > taxDeduction) {
            resultDeduction.push(taxDeduction.toFixed(1));
          }
          if(maxAmount < taxDeduction) {
            resultDeduction.push(maxAmount.toFixed(1));
          }

          maxAmount = maxAmount - taxDeduction
        }
        this.renderedDeduction = resultDeduction;
        this.salary = '';
      }
    },
    computedPostfix(num){
      switch(num){
        case 3:
          return 'ий';
        case 2:
        case 6:
        case 7:
        case 8:
          return 'ой';
        default:
          return 'ый';
      }
    },
    clearFrom(){
      this.salary = '';
      this.renderedDeduction = [];
    }
  }
}
</script>

<style scoped lang="scss">
  .modal {
    padding-top: 120px;
    padding-bottom: 88px;
    min-height: 100vh;
    background-color: rgba(0, 0, 0, 0.315);

    @media screen and (max-width: 425px) {
      padding: 0;
      width: 100%;
    }
  }

  .content {
    position: relative;
    background-color: #fff;
    padding: 32px;
    width: 552px;
    margin: 0 auto;
    border-radius: 30px;

    @media screen and (max-width: 768px) {
      width: 453px;
    }

    @media screen and (max-width: 571px) {
      width: 90%;
      padding: 16px;
    }

    @media screen and (max-width: 425px) {
      width: 100%;
      border-radius: 0;
      min-height: 100%;
      min-height: 100vh;
      padding-top: 32px;
      display: flex;
      flex-direction: column;
    }
  }

  .close {
    position: absolute;
    top: 23px;
    right: 27px;

    background: transparent;
    border: none;
    display: flex;

    @media screen and (max-width: 425px) {
      top: 22px;
      right: 22px;
    }
  }

  .title {
    font-size: 28px;
    line-height: 40px;
    margin-bottom: 16px;
  }

  .descr {
    color: #808080;
    font-size: 14px;
    line-height: 24px;
    width: 95%;
    margin-bottom: 24px;
  }

  .salary {
    color: #000;
    display: block;
    font-size: 14px;
    line-height: 24px;
    font-family: "Lab Grotesque-Medium";
    margin-bottom: 8px;
  }

  .input {
    display: block;
    width: 100%;
    padding: 8px 10px;
    border: 1px solid #DFE3E6;
    border-radius: 3px;
    margin-top: 8px;
  }

  .calculate {
    border: none;
    background-color: transparent;
    color: $red;
    font-size: 14px;
    line-height: 24px;
    font-family: "Lab Grotesque-Medium";
    margin-bottom: 16px;

    @media screen and (max-width: 425px) {
      align-self: baseline;
    }
  }

  .result {
    display: flex;
    flex-direction: column;

    &__text {
      color: #000;
      font-size: 14px;
      line-height: 24px;
      font-family: "Lab Grotesque-Medium";
    }

    &__item {
      width: 100%;
      border-bottom: 1px solid #DFE3E6;
      padding-bottom: 16px;
      margin-top: 16px;

      display: flex;
      align-items: center;

    }

    &_year {
      color: #808080;
    }
  }

  .checkbox {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  .custom-check {
    width: 20px;
    height: 20px;
    border: 1px solid #E0E0E0;
    margin-right: 12px;
    border-radius: 6px;
    position: relative;
  } 

  .checkbox:checked ~ .custom-check {
    background: $gradient-red;
    border: none;

    &::after {
      content: '';
      border: 2px solid transparent;
      border-bottom-color: #fff;
      border-left-color: #fff;
      position: absolute;
      top: 5px;
      left: 4px;
      width: 13px;
      height: 6px;
      transform: rotate(310deg);
    }
  }

  .profit {
    padding-top: 20px;
    display: flex;
    flex-direction: row;
    column-gap: 32px;
    align-items: center;
    margin-bottom: 40px;
    align-self: baseline;

    @media screen and (max-width: 425px) {
      flex-direction: column;
    }
  }

  .choice {
    color: #000;
    font-size: 14px;
    line-height: 24px;
    font-family: "Lab Grotesque-Medium";
    align-self: baseline;

    @media screen and (max-width: 425px) {
      margin-bottom: 24px;
    }
  }

  .tags {
    display: flex;
    column-gap: 16px;
  }

  .tag {
    cursor: pointer;
    color: #000;
    background: #EEF0F2;
    font-size: 14px;
    line-height: 24px;
    font-family: "Lab Grotesque-Regular";
    padding: 6px 12px;
    border-radius: 50px;

    &:hover {
      background: #DFE3E6;
    }

    &__cheked {
      color: #fff;
      background: $gradient-red;

      &:hover {
        color: #fff;
        background: $gradient-red;
      }
    }
  }

  .tag input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  .tag:checked {
    color: #fff;
    background: $gradient-red;
  }

  .addbtn {
    padding: 16px;

    @media screen and (max-width: 425px) {
      padding: 12px;
      margin-top: auto;
    }
  }
</style>
