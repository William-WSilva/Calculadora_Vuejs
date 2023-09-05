<template>
    <div class="container">
        <section class="calculadora">
            <h1 class="titulo">WS</h1>
            <div class="calculadora__registrar">
                <input
                  type="text"
                  :class="{ active: inputActive === 0, numNegativo: inputNegativo }"
                  class="calculadora__registrar__inputs"
                  v-model="input0"
                  @click="setInputActive(0)"
                  @input="atualizarResultado"
                  @keydown="validarEntradaNumerica"
                >
                <span class="operacao">{{ operador }}</span>
                <input
                  type="text"
                  :class="{ active: inputActive === 1, numNegativo: inputNegativo }"
                  class="calculadora__registrar__inputs"
                  v-model="input1"
                  @click="setInputActive(1)"
                  @input="atualizarResultado"
                  @keydown="validarEntradaNumerica"
                  ref='inputFoco'
                >
            </div>
            <div class="calculadora__resultados">
                <span class="calculadora__resultados__valor" 
                :class="{ numNegativo: resultadoNegativo }">{{ resultado }}</span>
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
                        <img class="calculadora__bg__operacoes__select__historico"
                            :class="{ active: historico }"
                            src="./images/Historico.png" alt=""
                            @click="abrirFecharHistorico"
                        >
        
                        <div class="calculadora__bg__operacoes__select__historico__list"
                            :class="{ open: historico }">
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

// ===================== Importação, variaveis ===================== //
import { ref, watch } from 'vue';

const historico = ref(false);
const inputActive = ref(0);
const operador = ref('');
const resultado = ref(0);
const input0 = ref('');
const input1 = ref('');
const historicoList = ref([]);
const inputFoco = ref(null);
let resultadoNegativo = ref(false);
let inputNegativo = ref(false);
let calculo = 0;

// ===================== Eventos ===================== //

// ======= Assistir eventos nos inputs e troca de operadores ======= //
watch(operador, (newValue, oldValue) => {
atualizarResultado();
});

watch(input0, (newValue, oldValue) => {
    atualizarResultado();
    resultadoNegativo = resultado.value < 0 ? true : false;
    inputNegativo = input0.value < 0 ? true : false;
});

watch(input1, (newValue, oldValue) => {
    atualizarResultado();
    resultadoNegativo = resultado.value < 0 ? true : false;
    inputNegativo = input1.value < 0 ? true : false;
});



// ===================== Funções ===================== //

// ======= Validar botões clicados, executar ações e input de valores ======= //
function btnPress(valor) {
    const operadoresBtn = ["+", "-", "*", "/", "C", "CE", "="];
    
    if (operadoresBtn.includes(valor)) {
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

    if (!operadoresBtn.includes(valor)) {
        formatarZeroENegativos(valor);
    }
}

// ======= Validar teclas, executar ações e input de valores ======= //
function validarEntradaNumerica(event) {
const teclaPressionada = event.key;
const operadoresTeclado = [ "0", '1', '2', '3', '4', '5', '6', '7', '8', '9', '.', "Backspace", "Delete", "Tab"];

    if (teclaPressionada === "0") {
        event.preventDefault();
        formatarZeroENegativos(teclaPressionada);
    }

    if (!operadoresTeclado.includes(teclaPressionada)) {
        event.preventDefault();

        if ([ "+", "-", "*", "/"].includes(teclaPressionada)) {
            inputActive.value = 1;
            inputFoco.value.focus();
            operador.value = teclaPressionada;
        }else if(teclaPressionada === "Enter" ){
            gerarHistorico();
        }
    }
}

// ======= Atualiza o resultado sempre que valores inputs são alterados ======= //
function atualizarResultado(){
    const valorInput0 = parseFloat(input0.value);
    const valorInput1 = parseFloat(input1.value);
    
    switch (operador.value) {
        case '+':
            calculo = valorInput0 + valorInput1;
            validarNumero(calculo);
        break;
        case '-':
            calculo = valorInput0 - valorInput1;
            validarNumero(calculo);
        break;
        case '*':
            calculo = valorInput0 * valorInput1;
            validarNumero(calculo);
        break;
        case '/':
            calculo = valorInput0 / valorInput1;
            validarNumero(calculo);
        break;
    }
}

// ======= Validar input do tipo numeros e formatar zeros a esqueda ======= //
function formatarZeroENegativos(valor){
    const regexZeroAesquerda = /^0+(?!\.)/;   // Expressão regular para mais de 1 zero a esquerda
    const regexNumero = /^-?\d*\.?\d*$/; // Expressão regular para validar valor numérico ou decimal
    let valorFormatado = inputActive.value === 0 ? input0.value + valor.toString() : input1.value + valor.toString();
    
    if (!regexNumero.test(valorFormatado)) {
        valorFormatado = 0;
    }else if (regexZeroAesquerda.test(valorFormatado)) {
        valorFormatado = Number(valorFormatado.replace(regexZeroAesquerda, 0));
    }
    
    switch (inputActive.value){
        case 0:
            input0.value = valorFormatado
        break;
        case 1:
            input1.value = valorFormatado
        break;
    }
}

// ======= Validar número  ======= //
function validarNumero(calculo) {
    if (isFinite(calculo) === true) {
        const calcResult = calculo.toLocaleString('pt-BR');
        resultado.value = calculo.toLocaleString('pt-BR');
        
    } else {
        resultado.value = 0;
    }
}

// ======= Gerar hitórico de calculos  ======= //
function gerarHistorico() {
    const novoHistoricoItem = `${input0.value} ${operador.value} ${input1.value} = ${resultado.value}`;
    historicoList.value.push(novoHistoricoItem);

    input0.value = calculo;
    input1.value = '';
}

// ======= Abrir e Fechar seção histórico  ======= //
function abrirFecharHistorico() {
historico.value = !historico.value;
}

// ======= Mapear input ativo  ======= //
function setInputActive(index) {
inputActive.value = index;
}


</script>


<style src="./styles/Calculadora.css"></style>


