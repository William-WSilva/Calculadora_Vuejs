<template>
  <div class="container">
    <section class="calculadora">
        <h1 class="titulo">EBAC</h1>
        <div class="calculadora__registrar">
            <input
                type="text"
                :class="{ active: inputActive === 0 }"
                class="calculadora__registrar__inputs"
                v-model="input0"
                @click="setInputActive(0)"
                @input="atualizarResultado"
            >
            <span class="operacao">{{ operador }}</span>
            <input
                type="text"
                :class="{ active: inputActive === 1 }"
                class="calculadora__registrar__inputs"
                v-model="input1"
                @click="setInputActive(1)"
                @input="atualizarResultado"
            >
        </div>
        <div class="calculadora__resultados">
            <span class="calculadora__resultados__valor">{{ resultado }}</span>
        </div>
        
        <div class="calculadora__bg">
            
          <div class="calculadora__bg__operacoes">
            <div class="calculadora__bg__operacoes__select">
                <select class="calculadora__bg__operacoes__select__item btn_tema_cinza"
                    v-model="operador" 
                    @change="btnPress($event.target.value)">
                    <option value="" selected disabled>Selecione operação</option>
                    <option value="+">Adicionar</option>
                    <option value="-">Subtrair</option>
                    <option value="*">Multiplicar</option>
                    <option value="/">Dividir</option>
                </select>
                <img class="calculadora__bg__operacoes__select__historico" src="./images/Historico.png" alt=""
                @click="abrirFecharHistorico"
                >

                <div class="calculadora__bg__operacoes__select__historico__list"
                :class="{ open: historico }"
                >
                <span>Historico</span>
                <hr>
                <ol class="calculadora__bg__operacoes__select__historico__list__secao">
                    <li v-for="item in historicoList" :key="item">{{ item }}</li>
                </ol>
              </div>
            </div>
              
              <button class="calculadora__bg__operacoes__btn btn_tema_cinza" @click="btnPress('C')">C</button>
              <button class="calculadora__bg__operacoes__btn btn_tema_cinza" @click="btnPress('CE')">CE</button>
              <button class="calculadora__bg__operacoes__btn btn_tema" @click="btnPress('/')">/</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(7)">7</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(8)">8</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(9)">9</button>
              <button class="calculadora__bg__operacoes__btn btn_tema" @click="btnPress('*')">*</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(4)">4</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(5)">5</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(6)">6</button>
              <button class="calculadora__bg__operacoes__btn btn_tema" @click="btnPress('-')">-</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(1)">1</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(2)">2</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(3)">3</button>
              <button class="calculadora__bg__operacoes__btn btn_tema" @click="btnPress('+')">+</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress(0)">0</button>
              <button class="calculadora__bg__operacoes__btn" @click="btnPress('.')">.</button>
              <button class="calculadora__bg__operacoes__btn btn_tema" @click="btnPress('=')">=</button>
          </div>
        </div>
    </section>
  </div>
</template>



<script setup>
import { ref, watch } from 'vue';

const historico = ref(false);
const inputActive = ref(0);
const operador = ref('');
const resultado = ref(0);
const input0 = ref('');
const input1 = ref('');
const historicoList = ref([]);


// ======= Eventos ======= //
watch(operador, (newValue, oldValue) => {
  atualizarResultado();
});

watch(input0, (newValue, oldValue) => {
  atualizarResultado();
});

watch(input1, (newValue, oldValue) => {
  atualizarResultado();
});


function gerarHistorico() {
    const novoHistoricoItem = `${input0.value} ${operador.value} ${input1.value} = ${resultado.value}`;
    historicoList.value.push(novoHistoricoItem);
}


function atualizarResultado(){
    const valorInput0 = parseFloat(input0.value);
    const valorInput1 = parseFloat(input1.value);

    // console.log(`${valorInput0} ${operador.value} ${valorInput1}`);
    switch (operador.value) {
        case '+':
          resultado.value = valorInput0 + valorInput1;
          resultado.value = isFinite(resultado.value)  ? resultado.value.toFixed(2) : 0
        break;
        case '-':
          resultado.value = valorInput0 - valorInput1;
          resultado.value = isFinite(resultado.value)  ? resultado.value.toFixed(2) : 0
        break;
        case '*':
          resultado.value = valorInput0 * valorInput1;
          resultado.value = isFinite(resultado.value)  ? resultado.value.toFixed(2) : 0
        break;
        case '/':
          resultado.value = valorInput0 / valorInput1;
          resultado.value = isFinite(resultado.value)  ? resultado.value.toFixed(2) : 0
        break;
        default:
          resultado.value = 0;
    }
}

function btnPress(valor) {
    const operadores = ["+", "-", "*", "/", "C", "CE", "="];
    
    if (operadores.includes(valor)) {
        switch (valor) {
            case "+":
                operador.value = valor;
                inputActive.value = 1;
            break;
            case "-":
                operador.value = valor;
                inputActive.value = 1;
            break;
            case "*":
                operador.value = valor;
                inputActive.value = 1;
            break;
            case "/":
                operador.value = valor;
                inputActive.value = 1;
            break;
            case "=":
                gerarHistorico();
                input0.value = resultado.value;
                input1.value = "";
            break;
            case "CE":
                switch (inputActive.value){
                    case 0:
                        input0.value = input0.value.slice(0, -1);
                    break;
                    case 1:
                        input1.value = input1.value.slice(0, -1);
                    break;
                } 
            break;
            case "C":
                input0.value = "";
                input1.value = "";
                inputActive.value = 0;
                resultado.value = 0;
                operador.value = ""
            break;
        }
    }

    if (!operadores.includes(valor)) {
        switch (inputActive.value){
            case 0:
                input0.value += valor;
            break;
            case 1:
                input1.value += valor;
            break;
        }
        
    }
}

function abrirFecharHistorico() {
  historico.value = !historico.value;
}

function setInputActive(index) {
  inputActive.value = index;
}
</script>


<style scoped>
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    text-decoration: none;

    /* border: 1px solid gray; */
}

.container{
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.calculadora{
    display: flex;
    flex-direction: column;
    justify-content: end;
    align-items: center;
    background: radial-gradient(at 50% 0%, #1c4f73, #1c1f27);

    width: 320px;
    height: 540px;
    border-radius: 8px;
    color: #fff;
    font-size: 18px;
}

.titulo{
    text-align: center;
    width: 320px;
    margin-bottom: 60px;
}

.calculadora__bg{
    display: flex;
    justify-content: center;
    background-color: #262728;
    width: 320px;
    height: 340px;
    border-radius: 8px;
    position: relative;
}

.calculadora__bg__operacoes{
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 15px 15px;
    padding: 20px;
    width: 320px;
}

.calculadora__bg__operacoes__btn{
    /* width: 64px; */
    height: 35px;
    border-radius: 8px;
    font-size: 24px;
    background-color: #303136;
    color: #29a8ff;
    border: none;
    font-weight: bold;
    cursor: pointer;
    
}

.calculadora__bg__operacoes__btn:hover{
  box-shadow: 0px 0px 10px 5px #1c4f73;
}

.calculadora__bg__operacoes__select{
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-column: 1 / -1;
    gap: 20px 20px;
}

.calculadora__bg__operacoes__select__item{
    border-radius: 4px;
    border: none;
    grid-column: 1 / 4;
    height: 35px;
    font-size: 22px;
    cursor: pointer;
}

.calculadora__bg__operacoes__select__historico{
    height: 35px;
    cursor: pointer;
    border-radius: 8px;
}

.calculadora__bg__operacoes__select__historico:hover{
    background-color: transparent;
    border-radius: 8px;
    box-shadow:  0px 0px 10px 3px #1c4f73;

}

.calculadora__bg__operacoes__select__historico__list{
    position: absolute;
    top: 0;
    left: 0;
    padding: 5px;
    background: radial-gradient(at 50% 0%, #1c4f73, #1c1f27);
    opacity: 0.9;
    width: 240px;
    height: 340px;
    /* display: none; */
    overflow-y: auto;
    opacity: 0;
    visibility: hidden;
    transition: linear 0.3s;
    border-radius: 8px;
    
}
.calculadora__bg__operacoes__select__historico__list.open{
    opacity: 1;
    visibility: visible;
}

.calculadora__bg__operacoes__select__historico__list__secao{
    display: flex;
    flex-direction: column-reverse;
    padding-left: 30px ;
    font-size: 16px;
}

.calculadora__bg__operacoes__select__historico__list__secao li{
    cursor: default;
    margin: 5px 0;
}

.calculadora__bg__operacoes__select__historico__list__secao li:hover{
    box-shadow: 0px 0px 5px 3px #29a8ff;
}

.calculadora__bg__operacoes__btn:nth-child(3){
    grid-column: 2 / 4;
    
}

.calculadora__bg__operacoes__btn:last-child{
    grid-column: 3 / -1;
    
}

.calculadora__resultados{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 276px;
    height: 60px;
    font-size: 24px;
    color: #fff;
    font-weight: bold;
}

.calculadora__registrar{
    width: 276px;
    display: flex;
    justify-content: space-between;
}

.calculadora__registrar__inputs{
    text-align: end;
    width: 100%;
    height: 40px;
    font-size: 20px;
    color: #1c4f73;
    border-radius: 8px;
    border: none;
    padding-right: 3px;
}

.operacao{
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 28px;
    font-weight: bold;
    color: #fff;
    width: 100px;
    height: 100%;
}

.btn_tema{
    border: none;
    background-color: #29a8ff;
    color: #fff;
}

.btn_tema_cinza{
    border: none;
    background-color: #616161;
    color: #fff;
    font-size: 18px;
}

.active{
    box-shadow: 0px 0px 5px 3px #29a8ff;
}

.numNegativo{
    color: red;
}

option:disabled{
    display: none;
}

</style>
