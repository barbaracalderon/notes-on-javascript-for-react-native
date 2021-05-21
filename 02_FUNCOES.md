# Funções do JavaScript

**CONTEÚDOS DAS ANOTAÇÕES DE "FUNÇÕES"**

8. "This" pode variar
9. "This" e a função bind #01
10. "This" e a função bind #02
11. Funções Arrow #01
12. Funções Arrow #02
13. Funções Arrow #03
14. Funções Anônimas

---
# 8. "This" pode variar

O ```this``` é uma forma de referenciar o objeto **atual da execução**.

É um paradigma comum na orientação a objetos: uma classe é um "molde" a partir do qual os objetos são criados. Objetos, por sua vez, possuem características (atributos) e ações próprias (funções).

Em JavaScript, o ```this``` **não se refere sempre ao objeto**. Ele se refere **ao objeto atual da execução** - e esta execução ocorre dentro de um **contexto**.

Dependendo de como você chama o ```this```, **ele pode variar**. 

Agora... **se você utilizar uma arrow function, o ```this``` nunca varia**. Ele é definido no momento de criação da função.

Por exemplo. se a função foi criada dentro de um contexto global, o ```this``` se torna a janela do software.

## Contexto

Se você **não** usa arrow functions, então o **contexto é de mega importância** porque o ```this``` **varia com esse contexto**.

---
# 9. "This" e a Função Bind #01

Veja o objeto pessoa abaixo.

```javascript
const pessoa = {
    saudacao: 'Bom dia!',
    falar() {
        console.log(this.saudacao)
    }
}

pessoa.falar()          // > Bom dia!

```
Acima temos um objeto chamado pessoa com um atributo ```saudacao``` e a função ```falar()```. Esse método, ou função, chama o console para mostrar a saudação de ```this```. **Neste caso, ```this``` é o objeto pessoa.**

Abaixo ocorre um problema.

```javascript
const pessoa = {
    saudacao: 'Bom dia!',
    falar() {
        console.log(this.saudacao)
    }
}

pessoa.falar()
const falar = pessoa.falar
falar()               // > undefined
```
Deu um problema!

Houve um conflito entre dois paradigmas: Programação Orientada a Objetos (OOP, do inglês) e a Programação Funcional (functional, do inglês). 

Apenas chamar a função ```falar()``` (paradigma funcional), **sem atrelar essa função a um objeto**, dá o resultado ```undefined``` porque, **na origem de criação**, o falar está atrelado a um objeto (paradigma OOP).

Leia o parágrafo acima de novo.

Como resolver? 

Neste caso, podemos usar o ```bind```.

Veja abaixo.

```javascript
const pessoa = {
    saudacao: 'Bom dia!',
    falar() {
        console.log(this.saudacao)
    }
}

pessoa.falar()
const falar = pessoa.falar
falar()                 // > undefined

const falarDePessoa = pessoa.falar.bind(pessoa)     // Bind aqui!
falarDePessoa()         // > Bom dia!

```
Acima, o ```bind``` disse que **o objeto ao qual o ```this``` se refere é *pessoa***. Ele amarrou esta noção ao colocar pessoa como **PARÂMETRO DE BIND**.

Ele realmente "amarra".

*"to bind" em inglês significa amarrar.*

---
# 10. "This" e a Função Bind #02

Aqui vamos conseguir: **armazenar o ```this``` em uma constante, no momento que o ```this``` apontar para o objeto que *de fato a gente quer referenciar***. 

Depois que ele fica numa constante, moleza. Podemos usar essa constante para acessar o ```this``` quando a gente quiser.

Dá uma olhada no próximo código abaixo e volta aqui.

Abaixo, uma função chamada ```Pessoa``` que tem atributo idade e uma outra função em si -```setInterval()```. 

Por sua vez, a função ```setInterval()``` chama outra função ```function()```, que neste caso é **vazia em nome** mas que faz incremento da idade e aparece na tela. 

A função ```setInterval()```, além de ter a ```function()``` como parâmetro, tem também o intervalo 1000. Isso significa que, **ao ser criado o objeto Pessoa** (derivado da função classe Pessoa), **é disparada a função ```setInterval()``` a cada 1000 milissegundos** (1 segundo) devido ao incremento.

Quem é o ```this``` neste caso?

```javascript
function Pessoa() {
    this.idade = 0

    setInterval(function() {
        this.idade++
        console.log(this.idade)
    }, 1000)
}

new Pessoa              // > NaN
                        // > NaN
                        // > Nan
                        // > ...
```
Neste caso, o ```this``` é um ```NaN```. 

Quem está executando a função ```function()``` é um temporizador interno da ```setInterval()``` - e **não o objeto Pessoa**. Por isso a "idade" sendo incrementada não é mostrada.

Vamos consertar isso.

```javascript
function Pessoa() {
    this.idade = 0

    setInterval(function() {
        this.idade++
        console.log(this.idade)
    }.bind(this), 1000)         // Bind aqui!
}

new Pessoa              // > 1
                        // > 2
                        // > 3
                        // > ...
```
Agora sim mostra a idade sendo incrementada em 1 unidade. 

A **idade é atributo de pessoa** e o ```bind``` agora faz o ```this``` **apontar para o objeto pessoa**. 

Ou.

Ainda podemos criar um outro artifício usando uma variável qualquer - que neste caso vamos chamar de ```self```. 

*(Esse é um "truque" usado por muitos desenvolvedores.)*

Veja.

```javascript
function Pessoa() {
    this.idade = 0
    
    const self = this               // artifício "self" -> amarrei o this ao contexto do Objeto Pessoa
                                    // antes de setInterval
    setInterval(function() {
        self.idade++
        console.log(self.idade)
    }, 1000)
}

new Pessoa              // > 1
                        // > 2
                        // > 3
                        // > ...
```
Veja que a criação da variável ```self``` foi, propositalmente, feita ANTES da função ```setInterval()``` justamente **para pegar o ```this``` da função pessoa**. 

O ```this``` **dentro da função ```setInterval()``` se refere ao contexto de ```setInterval()```** - e não mais ao contexto de Pessoa.

Esse é um truque comum que as pessoas utilizam para contornar o problema.

---
# 11. Funções Arrow #01 (=>)

Em 2015, foi introduzido a nova função.

*Arrow, em inglês, significa "Seta".*

Dois objetivos: 
- ser mais curta;
- ter um ```this``` associado ao contexto em que foi escrita.

Veja uma função escrita de três formas diferentes.

```javascript
//FORMA 01
let dobro = function (a) {
    return 2 * a
}

//FORMA 02
dobro = (a) => {        // arrow function 
    return 2 * a        // com parâmetro (a)
}

//FORMA 03
dobro = a => 2 * a      // arrow function 
                        // com "return" implícito"
```
Quando você tira as chaves ```{ }``` o **retorno é implícito**. Isto ocorre na terceira função.

As arrow functions te forçam a usar funções bem específicas.

---
# 12. Funções Arrow #02 (=>)

O ```this``` dentro de uma arrow function é ~**fixo**. **Ele se referencia ao contexto maior de onde está**.

Vamos reescrever o seguinte código para uma arrow function:

```javascript
function Pessoa() {
    this.idade = 0

    setInterval(function() {
        this.idade++
        console.log(this.idade)
    }, 1000)
}

new Pessoa              // > NaN
                        // > NaN
                        // > Nan
                        // > ...
```

O mesmo código de cima traduzido para uma arrow function:

```javascript
function Pessoa() {
    this.idade = 0

    setInterval( () => {        // Repare na típica setinha =>
        this.idade++            // Esse this é global, contexto Pessoa(), não setInterval()
        console.log(this.idade)
    }, 1000 )
}

new Pessoa              // > 1
                        // > 2
                        // > 3
                        // > ...
```

Veja que o ```this``` agora se refere ao objeto Pessoa criado  - e não mais ao temporizador interno da função ```setInterval()```.

Isso acontece porque o ```setInterval()``` foi criado dentro do contexto função Pessoa. O ```this``` responde a esse contexto numa arrow function.

---
# 13. Funções Arrow #03 (=>)

Pra entender o ```this``` no contexto de uma arrow function, preciso conhecer como ele se comporta em funções comuns.

Vamos no comum, depois nas arrow functions.

## THIS no contexto Global

No código abaixo, que não é uma arrow function, eu vou comparar se o ```this``` aponta para o parâmetro da função que eu criei.

O ```this``` se refere ao parâmetro da função que eu criei: sim ou não?

```javascript
let comparaComThis = function (param) { 
    console.log(this === param)
}

comparaComThis(global)          // true
```

A resposta é sim.

Veja que na última linha, eu coloquei como parâmetro o ```global``` - no contexto do Node.js se refere ao contexto que existe de maior; no contexto de um navegador, como o chrome, por exemplo, não existe o ```global``` mas existe o ```window```, que é o maior contexto possível. Se jogar a função ```comparaComThis(window)``` no navegador, vai dar ```true``` também.

*PS: No node, o objeto global é chamado de Global, enquanto que no navegador Chrome, o objeto global é chamado de Window.*

Resumindo.

Essa função que eu criei, ```comparaComThis```, me mostra se o ```this``` da função se refere ao parâmetro (e eu que escolho esse parâmetro): ```true``` ou ```false``` como resposta.

**ATENÇÃO**```

É por este motivo que se deve ter muito cuidado em chamar o ```this``` no escopo de uma função e determinar um atributo para esse ```this``` porque, na verdade, **você tá mexendo no this do contexto global**. Muitos erros vêm daqui.

## THIS no contexto do Objeto

Vamos olhar o mesmo código de cima, mas agora amarrando ao ```this``` um objeto qualquer, por meio do ```bind```. 

```javascript
let comparaComThis = function (param) { 
    console.log(this === param)
}

comparaComThis(global)          // > true

const obj = {}                  // criei o objeto 'obj'
comparaComThis = comparaComThis.bind(obj)   // Aqui o bind!

comparaComThis(global)          // > false
comparaComThis(obj)             // > true
```

Na penúltima linha, vemos que o ```this``` não se refere mais ao contexto global (apontada pelo parâmetro node "global") - ele mudou: agora o ```this``` se refere ao objeto. **Por isso o resultado é ```false```** nessa segunda leitura.

Na última linha, é ```true``` que o ```this``` se refere ao objeto. Sim, porque o objeto foi amarrado ao ```this``` por meio do ```bind```. Por isso retorna verdadeiro.

## THIS no contexto das Arrow Functions

Será que esse ```bind``` funciona nas arrow functions? Será que eu consigo mudar o ```this``` de uma arrow function? Na próxima seção vamos ver (a resposta é não), mas primeiro uma coisinha sobre contexto nas arrow functions.

Vimos anteriormente, que pela natureza da arrow function, **o ```this``` sempre se refere ao contexto no qual a função foi criada**. No caso de criarmos uma arrow function **dentro do ambiente node**, esse contexto **não é global**, ele é o contexto do **módulo**, pois todo arquivo js criado é um módulo dentro do node. O contexto do módulo é o ```module.exports```.

O mais "global" que a arrow function consegue chegar é o seu próprio módulo: ele não tem a visão global do todo. 

Então, quando crio uma arrow function dentro do ambiente node, ele aponta para o contexto do módulo. Não para o global.

Veja.

```javascript
let comparaComThis = function (param) { 
    console.log(this === param)
}

comparaComThis(global)                   // > true

const obj = {}                           // criei o objeto 'obj'
comparaComThis = comparaComThis.bind(obj) // Aqui o bind!

comparaComThis(global)                   // > false
comparaComThis(obj)                      // > true

// Arrow Function abaixo:
let comparaComThisArrow = param => console.log(this === param)

comparaComThisArrow(global)             // > false
comparaComThisArrow(module.exports)     // > true
```
Na penúltima linha, o ```this``` da arrow function não aponta para o contexto global. Por isto o resultado deu falso.

Na última linha, vemos que o ```this``` da arrow function aponta para o contexto do seu módulo - **pois foi onde a arrow function foi criada**. Por isto o resultado deu verdadeiro.
