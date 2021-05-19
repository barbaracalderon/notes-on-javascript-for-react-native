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

A forma de instanciar um objeto em Javascript é por uma **função**. 

Nesse caso, a função exerce o papel de uma classe.

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
