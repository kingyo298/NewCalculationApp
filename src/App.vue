<template>
  <div class="text-white bg-main vh-100">
    <div class="py-5">
      <div class="frame px-3 py-4 m-auto bg-dark">
        <div>
            <label>input:</label>
            <input type="text" class="form-control" :value="formula" disabled/>
        </div>

        <div>
            <label>total:</label>
            <input type="text" class="form-control" :value="value" disabled/>
        </div>

        <div class="buttons mt-3 d-flex justify-content-around flex-wrap col-12">
          <button class="btn btn-gray mb-3" @click="formula += '7'">7</button>
          <button class="btn btn-gray mb-3" @click="formula += '8'">8</button>
          <button class="btn btn-gray mb-3" @click="formula += '9'">9</button>
          <button class="btn btn-gray mb-3" @click="formula += '4'">4</button>
          <button class="btn btn-gray mb-3" @click="formula += '5'">5</button>
          <button class="btn btn-gray mb-3" @click="formula += '6'">6</button>
          <button class="btn btn-gray mb-3" @click="formula += '1'">1</button>
          <button class="btn btn-gray mb-3" @click="formula += '2'">2</button>
          <button class="btn btn-gray mb-3" @click="formula += '3'">3</button>
          <button class="btn btn-gray mb-3" @click="formula += '0'">0</button>
          <button class="btn btn-orange mb-3" @click="concatenateOperator(' + ')">+</button>
          <button class="btn btn-orange mb-3" @click="concatenateOperator(' - ')">-</button>
          <button class="btn btn-orange mb-3" @click="concatenateOperator(' * ')">*</button>
          <button class="btn btn-orange mb-3" @click="concatenateOperator(' / ')">/</button>
          <button class="btn btn-orange mb-3" @click="calculate()">=</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
  const value = ref<number>();
  const formula = ref<string>("");

  const concatenateOperator = (operator: string) => {
    if(formula.value === undefined) return;
    if(formula.value.slice(-1) === " "){
      return formula.value = formula.value.substring(0,formula.value.length - 3) + operator;
    }else{
      return formula.value += operator;
    }
  }
  const calcDyadicOperator = (num1: number, operator: string, num2: number) => {
    let result;
    switch(operator){
      case "+" :
        result =  num1 + num2;
        break;
      case "-" :
        result =  num1 - num2;
        break;
      case "*" :
        result =  num1 * num2;
        break;
      case "/" :
        result =  num1 / num2;
        break;
      default :
        throw new SyntaxError(operator + 'is not a operator');
    }
    return result;
  }
  const calculate = () => {
    if(formula.value === undefined) return;
    let formulaArray: string[] = formula.value.split(" ");
    let temp = 0;
    while(formulaArray.length >= 3){
      temp = calcDyadicOperator(parseInt(formulaArray[0]),formulaArray[1],parseInt(formulaArray[2]));
      formulaArray.splice(0,3);
      formulaArray.unshift('' + temp);
    }
    formula.value = "";
    value.value = parseInt(formulaArray[0]);
  }
</script>
<style scoped>
.btn-orange{
  background-color: orange;
}
.btn-gray{
  background-color: #b8b8b8
  ;
}
.frame{
  width: 320px;
  border-radius: 20px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
}

.btn{
  width: 75px;
  height: 75px;
  border-radius: 50%;
}
.bg-main{
  background: #eee;
}
</style>
