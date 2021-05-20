# Fundamentos do JavaScript
**CONTEÚDOS DAS ANOTAÇÕES DE "FUNDAMENTOS"**
1. O básico var, let e const
2. Função vs. Objeto
3. Par Nome/Valor
4. Operadores: Destructuring #01
5. Operadores: Destructuring #02
6. Operadores: Destructuring #03
7. Operadores: Destructuring #04

---

# 1. O Básico de VAR, LET e CONST

O nome que você dá para as variáveis, funções, objetos, etc. é muito importante. Ele deve **refletir** o que você está fazendo. Vale a pena gastar um tempo pensando no nome das coisas.

```
var a = 3
let b = 4
const c = 5
```

VAR e LET declaram **variáveis**.

CONST declaram **constantes**. Não pode trocar o seu conteúdo uma vez declarado.

## Regra Geral

Dar preferência por usar a declaração LET no lugar de VAR. Essa é a maneira mais moderna e de acordo com as autoridades no assunto (Ecmascript).

---
# 2. Função vs. Objeto

A forma de instanciar um objeto em JavaScript é por uma **função**. 

Nesse caso, **a função exerce o papel de uma classe.**

Já foi introduzido o termo "class" mas, na prática, o próprio "class" é uma função (por baixo dos panos). As funções têm atributos e comportamentos e podem ser instanciadas. Ou seja, aqui a função é classe.

## EXEMPLO 01 - **Instanciando uma função Object (criando um objeto).**
```
console.log(typeof Object)
console.log(typeof new Object)

/*
typeof Object -> é uma função em JS.
typeof new Object -> é um objeto. Eu instanciei um objeto a partir da função Object.
*/
```
## EXEMPLO 02 - **Instanciando uma função que eu criei (criando um objeto).**

```
const Cliente = function() {}
console.log(typeof Cliente)
console.log(typeof new Cliente)

/*
const Cliente -> criei minha função.
typeof Cliente -> é uma função.
typeof new Cliente -> é um objeto. Eu instanciei esse objeto a partir da função Cliente que eu criei.
*/
```
## EXEMPLO 03 - **Criando uma classe (instanciando uma função) e (para instanciar um objeto).**

```
class Produto {}
console.log(typeof Produto)
console.log(typeof new Produto)

/*
class Produto -> é uma função.
typeof Produto -> é uma função.
typeof new Produto -> é um objeto. Eu instanciei esse objeto a partir da classe que eu criei. Essa classe é uma função (na prática).
*/
```

---
# 3. Par Nome/Valor

Chamado também de: nome/valor, par/valor, chave/valor.

Em um código de Javascript, vemos por todos os lados pares de nome/valor. Esse nome e valor podem ser conectados por ```=``` ou ```:``` conforme nos exemplos abaixo.

```
const saudacao = 'Opa!'        \\ contexto léxico 01 (escopo)

function exec() {
    const saudacao = 'E aí!'    \\ contexto léxico 02 (escopo)
    return saudacao
}

const Cliente = {
    nome: 'Maria',
    idade: 32,
    peso: 70,
    endereco:  {
        logradouro: 'Rua muito legal',
        numero: 123
    }
}
```

---
# 4. Operadores: Destructuring #01 - Objeto {}

Em 2015, um novo operador foi introduzido em Javascript: operador destructuring. Ele tira da estrutura alguma coisa. A estrutura normalmente é um objeto. Então, na prática, ele tira alguma coisa do objeto. É uma forma de extrair do objeto seus atributos, elementos.

Tem duas formas de estrutura.

No âmbito do objeto, usamos as chaves ```{}``` e no âmbito do array, usamos os colchetes ```[]```.

Aqui tratamos do âmbito do objeto. Na próxima seção, no âmbito do array.

## EXEMPLO 01 - **no âmbito do objeto.**

```
const pessoa = {
    nome: 'Ana',
    idade: 5,
    endereco: {
        logradouro: 'Rua ABC',
        numero: 1000
    }
}

const { nome, pessoa } = pessoa     \\ Operador Destructuring
console.log(nome, idade)            \\ > Ana 5
```
No exemplo de cima, eu tenho um objeto chamado pessoa com alguns atributos (nome, idade, endereco). Eu utilizo o operador destructuring para tirar do objeto pessoa, os atributos "nome" e "idade".

```
const { nome, pessoa } = pessoa

console.log(nome, pessoa)       \\ > Ana 5
```
As chaves aqui indicam o operador Destructuring. Essa operação diz o seguinte: 'usando esse operador, eu quero os atributos "nome" e "pessoa" que vão ser retirados do objeto "pessoa".'

O console.log vai imprimir o que está contido em "nome" e "idade": respectivamente, 'Ana' e '5'.

Agora, eu posso dar um novo nome a esses atributos pra usar no meu código. Em vez de fazer como acima, eu faço conforme abaixo. Veja abaixo.

```
const { nome: n, idade: i } = pessoa

console.log(n, i)       \\ > Ana 5
```
O console.log vai imprimir o que está contido em "n" e "i": respectivamente, 'Ana' e '5'. Assim como acima.

## Atributo não existe (undefined)
E se eu tirar um atributo que não existe no objeto?

O normal é retornar undefined. A não ser que eu deixe "setado" no caso de vir undefined. Vamos supor que queremos extrair o atributo "sobrenome" (não existe) e o atributo "bemHumorada" (não existe mas vou setar).

```
const { sobrenome, bemHumorada = true } = pessoa

console.log(sobrenome, bemHumorada)     \\ > Undefined true
```

## Atributo aninhado (garantir o caminho até o atributo)

Ainda do exemplo do objeto pessoa de cima, vamos acessar os atributos de logradouro e número.

```
const { endereco: { logradouro, numero, cep } } = pessoa

console.log(logradouro, numero, cep)    \\ > Rua ABC 1000 undefined
```
Cuidado para acessar atributos que não existem. Tem que ter cuidado: é preciso ter certeza que o caminho até o atributo desejado está limpo.

# 5. Operadores: Destructuring #02 - Array []

Pegar elementos de dentro de um array.

## EXEMPLO 02 - **no âmbito do array.**

Criei uma lista com um único elemento "a" que vai receber o valor 10 dentro da lista. 
```
const [a] = 10

console.log(a)              \\ > 10
```
Aqui estou atribuindo valores ao meu array. 
```
const [n1, , n3, , n5, n6=10] = [10, 7, 9, 8]]

console.log(n1, n3, n5, n6)     \\ > 10 9 undefined 0
```
Veja que não existe n2, n4 - eu pulei eles. Seria assim n2=7 e n4=8. Então só sobram os valores 10, 9 para colocar ali dentro do meu array.

Também posso fazer um array de arrays.

```
const [, [, nota]] = [[, 8, 8], [9, 6, 8]]

console.log(nota)         \\ > 6
```

---
# 6. Operadores: Destructuring #03 - Função(destructuring-objeto)

Vou criar uma função rand() que vai me retornar um número aleatório.

```
function rand( { min = 0, max = 1000} ) 
```

Eu passei como parâmetro um objeto? Não.

Significa que eu passei como parâmetro da função rand, um operador destructuring que vai dar como parâmetro dois valores, 0 e 100, para a minha função. Assim, não preciso usar a notação ```rand.min``` ou ```rand.max``` pra acessar esses valores no interior da minha função.

Continuando.

```
function rand( { min = 0, max = 1000} ) {
    const valor = Math.random() * (max - min) + min
    return Math.floor(valor)
}

const obj = { max: 50, min: 40 }

console.log(rand(obj))          \\ > 43 *randômico
```

No exemplo acima, eu desestruturei os valores 0 e 100 para poder usar no interior da minha função. Eles são o min e o max. Dentro da função, usei apenas min e max e fiz o algoritmo.

Depois, eu criei um objeto (objetos tem chaves {}) com atributos max e min. Então quando eu chamo a função rand com este objeto (rand(obj)), eu uso os atributos do obj como parâmetros da minha função rand.

---
# 7. Operadores: Destructuring #04 - Função(destructuring-array)

Fazer conforme o destructuring #03 porém agora usando array no lugar de objeto.

```
function rand([min = 0, max = 1000]) {
    if (min > max) [min, max] = [max, min]
    const valor = Math.random() * (max - min) + min
    return Math.floor(valor)
}

console.log(rand([50, 40]))         \\ > 43 *randômico

```
---
Chegamos ao final. Próxima seção lida com funções.

