# Array em JavaScript

## Contexto

Array é um objeto em Javascript e não uma estrutura própria. 

Quando lidamos com o array e deletamos um elemento, Javascript na verdade substitui o elemento pelo valor 'undefined'. A mesma coisa acontece quando adicionamos um elemento em uma posição maior que o array: ele insere o elemento nesse índice desejado, porém preenche com 'undefined' as posições do meio. Assim, o array fica de um tamanho que vai até o índice cujo elemento foi inserido, nem que pra isso ele preencha todos os lugares com 'undefined' pra chegar lá. 

### Splice: deleta e adiciona

Agora sobre o splice.

O método splice é interessante porque ele manipula o array, deletando ou adicionando elementos nele - ao mesmo tempo inclusive. Por exemplo:

`.splice(1, x)`: vai no índice 1 e delete x elementos a partir (inclusivo) desse índice. 

`.splice(1, x, 'elemento1', 'elemento2')`: além de fazer o que está descrito na linha de cima, ele adiciona o elemento1 e elemento2 na sequência. 

## Métodos mais específicos (e comparação com Python)

```javascript
const pilotos = ['Vettel', 'Alonso', 'Schumacher']
// Esse const aqui em cima significa que aponta sempre pra esse array,
// mas dá pra manipulá-lo.
```

* `.pop()` é igual o `.pop()` do Python;
* `.push()` é o `.append()` do Python;
* `.shift()` é o `.remove(0)` do Python, que remove o primeiro elemento
* `.unshift('Hamilton')` adiciona 'Hamilton' como primeiro elemento
* `const algunsPiloos1 = pilotos.slice(2)` cria um novo array, cópia a partir do índice 2
* `const algunsPilotos2 = pilotos.slice(1, 4)` cria um novo array, cópia a partir do índice 1 e vai até o índice 3 (não adiciona o 4, é exclusivo).

## forEach #01

Existem algumas formas de percorrer um array. O forEach é a forma mais simples, mas ainda tem o `map, filter, reduce` que também percorrem... mas percorrem com mais propósito, mais específico.

A função forEach tem até 3 parâmetros: nome, indice e array. Nome é o valor iterado. Índice é o index. E Array é o próprio array que está sendo iterado.

Se passar um quarto parâmetro, por exemplo 'x', a função callback (de retorno) do forEach é 'undefined'.

```javascript
const aprovados = ['Ana', 'Aldo', 'Daniel', 'Raquel']

// Mostrar o nome, o index e o próprio array no console.log:
aprovados.forEach(function(nome, indice, array, x=undefined) {
    console.log(`${indice + 1}) ${nome}`)
    console.log(array)
})

// Mostrar apenas cada elemento (nome) do array:
aprovados.forEach(nome => console.log(nome))

// Cria uma variável (exibirAprovados) que guarda o console.log de cada item iterado:
const exibirAprovados = aprovado => console.log(aprovado)
aprovados.forEach(exibirAprovados)  // Exibe o elemento no console.log
```

## forEach #01

Implementando nosso próprio forEach. Vamos chamar de forEach2.

```javascript
Array.prototype.forEach2 = function(callback) {
    for (let i=0; i<this.length; i++) {
        callback(this[i], i, this)
    }
}

const aprovados = ['Agatha', 'Aldo', 'Daniel', 'Raquel']

aprovados.forEach2(function(nome, indice) {
    console.log(`${indice + 1}) ${nome}`)
})

// 1) Agatha
// 2) Aldo
// 3) Daniel
// 4) Raquel
```



*Continua*.