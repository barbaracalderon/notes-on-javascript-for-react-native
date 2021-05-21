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

*CONTINUA...*