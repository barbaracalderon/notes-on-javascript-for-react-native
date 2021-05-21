# Classes em JavaScript

Por baixo dos panos, não existe classe propriamente dita no JavaScript. O conceito foi introduzido em 2015, porém, por baixo dos panos, trata-se de uma função construtora. 

Pra quem vem de outras linguagens de programação, o conceito de Classe é muito presente. O React, que utiliza componentes, usa a abstração de Classe e não a de função. Por isto, fica bacana que a partir de 2015, essa abstração da Classe tenha aparecido no JS. Facilita a comunicação em um padrãozão, mesmo que assim.

Na prática, talvez tu utilize mais as próprias funções para dar origem aos objetos; em vez de criar uma classe, que por si mesma cria a uma função, para poder criar um objeto. 

A conclusão é que a orientação a objetos em JavaScript é centrada em funções. Como vimos, a classe é convertida em função: se você der um ```typeof``` na classe, vai mostrar uma função.

## Criação de Classes

Dá uma olhada no código abaixo. Veja se dá pra entender. Linha por linha, vai com calma.

Depois, tem a explicação por partes.

```javascript
// Primeira classe
class Lancamento {
    constructor(nome = "Genérico", valor = 0) {
        this.nome = nome
        this.valor = valor
    }
}

// Segunda classe
class CicloFinanceiro {
    constructor(mes, ano) {
        this.mes = mes
        this.ano = ano
        this.lancamentos = []
    }

    addLancamentos(...lancamentos) {
        lancamentos.forEach(l => this.lancamentos.push(l))
    }

    sumario() {
        let valorConsolidado = 0
        this.lancamentos.forEach(l => {
            valorConsolidado += l.valor
        })
        return valorConsolidado
    }
}

// Fazendo um teste
const salario = new Lancamento('salario', 4500)
const contaDeLuz = new Lancamento('luz', -220)

const contas = new CicloFinanceiro(6, 2021)
contas.addLancamentos(salario, contaDeLuz)
console.log(contas.sumario())           // > 4780
```

Resumidamente sobre o código acima: foram criadas duas classes ```Lancamento``` (com atributos apenas) e ```CicloFinanceiro``` (com atributos e duas funções). 

## Classe Lancamentos

Primeiro vamos ver a classe ```Lancamentos```.

Segue abaixo.

```javascript
// Primeira classe
class Lancamento {
    constructor(nome = "Genérico", valor = 0) {
        this.nome = nome
        this.valor = valor
    }
}
```

Como você pode ver, essa classe utiliza uma função ```constructor``` por baixo dos panos: ela constroi o objeto com um nome genérico e um valor que, caso não seja indicado no parâmetro, inicia com zero. 

## Classe CicloFinanceiro

Agora vamos ver a classe ```CicloFinanceiro```.

```javascript
// Segunda classe
class CicloFinanceiro {
    constructor(mes, ano) {
        this.mes = mes
        this.ano = ano
        this.lancamentos = []
    }

    addLancamentos(...lancamentos) {
        lancamentos.forEach(l => this.lancamentos.push(l))
    }

    sumario() {
        let valorConsolidado = 0
        this.lancamentos.forEach(l => {
            valorConsolidado += l.valor
        })
        return valorConsolidado
    }
}
```

Esta segunda classe também utiliza a função ```constructor``` por baixo dos panos para inicializar o objeto com dois parâmetros, ```mes``` e ```ano```. Além disso, tem um atributo interno chamado ```this.lancamentos``` que comporta um array vazio.

Além desses atributos, também conta com **duas funções**: ```addLancamentos()``` e ```sumario()```.

Em ```addLancamentos()``` veja que seus parâmetros são quantos lancamentos forem necessários. Isso é o que diz ```(...lancamentos)```. Para cada lancamento que você for adicionar, adicione este lancamento no atributo array vazio ```this.lancamentos```. 

Em ```sumario()``` não existe nada passando como parâmetro. No entanto, essa função começa com uma variável ```valorConsolidado``` que inicia com zero. Depois, para cada um dos lancamentos adicionados no atributo array (inicialmente) vazio ```this.lancamentos```, vá incrementando a variável ```valorConsolidado``` com o valor do lancamento. Assim, ao final, retorne o valor da variável ```valorConsolidado```, que contém o total dos valores de todos os lancamentos indicados.

## Fazendo o teste

Finalmente, a última parte para entender é o teste em si usando essas duas classes (com seus atributos e suas funções). 

```javascript
// Fazendo um teste
const salario = new Lancamento('salario', 4500)
const contaDeLuz = new Lancamento('luz', -220)

const contas = new CicloFinanceiro(6, 2021)
contas.addLancamentos(salario, contaDeLuz)
console.log(contas.sumario())           // > 4780
```

Na primeira linha, cria-se um objeto da classe Lancamento com dois parâmetros (nome = 'salario' e valor = 4500). Guarde esse objeto na constante ```salario```.

Na segunda linha, cria-se um objeto da classe Lancamento com dois parâmetros (nome = 'luz' e valor = -220). O valor é negativo porque vamos debitar do salário ao fazer as contas. Guarde este objeto na constante ```contaDeLuz```.

Na próxima linha, cria-se um objeto da classe CicloFinanceiro com dois parâmetros (mes = 6, ano = 2021). E guarde este objeto na constante ```contas```.

Perceba que tenho três objetos guardados em constantes. Estas constantes são meus objetos: ```salario```, ```contaDeLuz``` e ```contas```.

Na penúltima linha, objeto ```contas``` vai realizar a função (típica de sua Classe) ```addLancamentos``` usando os objetos ```salario``` e ```contaDeLuz``` como parâmetros. 

Na última linha, o ```console``` vai mostrar o resultado da execução da função ```sumario``` realizada pelo objeto ```contas```. 

Assim funcionam as classes em JS.

# Herança de Classe em JavaScript

O exemplo aqui da Herança de Classe vai ser feita com a ideia de filho, pai e avô.

Primeiro, todo o código com as três classes (avô, pai e filho) e o teste, para mostrar o que contém no objeto filho, e depois explicação detalhada por partes.

Tente ver o seguinte código com calma.

Na sequência, tem explicação.

```javascript
// Classe Avô
class Avo {
    constructor(sobrenome) {
        this.sobrenome = sobrenome
    }
}

// Classe Pai (Herança de Avô)
class Pai extends Avo {
    constructor(sobrenome, profissao = 'Professor') {
        super(sobrenome)
        this.profissao = profissao
    }
}

// Classe Filho (Herança de Pai)
class Filho extends Pai {
    constructor() {
        super('Silva')
    }
}

// Teste: criação do objeto Filho
const filho = new Filho
console.log(filho)  // > Filho { sobrenome: 'Silva', profissao: 'Professor' }
```

## Classe Avô

Começando com a classe avô, foco nela.

```javascript
// Classe Avô
class Avo {
    constructor(sobrenome) {
        this.sobrenome = sobrenome
    }
}
```
Na classe avô, iniciamos o ```constructor``` com um parâmetro, sobrenome. Este parâmetro será recebido pelo atributo ```this.sobrenome```. Não tem nada mais.

## Classe Pai

Agora a classe Pai, foco nela.

```javascript
// Classe Pai (Herança de Avô)
class Pai extends Avo {
    constructor(sobrenome, profissao = 'Professor') {
        super(sobrenome)
        this.profissao = profissao
    }
}
```
A classe Pai é herança da classe Avô - isso significa que a classe Pai tem tudo que a classe avô tem e mais um pouco. Ou seja, a classe Pai **_extende_** a classe Avô. Por isso o ```extends``` ali, é a aplicação direta da Herança.

Além disso, a classe pai tem no seu ```constructor``` dois parâmetros: sobrenome e profissão. Caso o usuário **não** forneça o parâmetro profissão, automaticamente assume-se que a profissão então é de Professor.

Agora veja que dentro do ```constructor``` ainda existe o ```super(sobrenome)```. Ele é responsável por invocar a classe Avô e passar ```sobrenome``` como parâmetro da classe Avô. Repare que tudo isso na construção da classe Pai, na inicialização do objeto da classe Pai.

Finalmente, a classe Pai tem como atributo o ```this.profissao``` que recebe como valor... o segundo parâmetro passado nesta classe (caso exista, caso não então automaticamente é assumida a string Professor no lugar). 

*PS: o atributo ```this.profissao``` só existe na classe Pai.*


## Classe Filho

Finalmente, vamos à classe Filho.


```javascript
// Classe Filho (Herança de Pai)
class Filho extends Pai {
    constructor() {
        super('Silva')
    }
}
```

A classe Filho é uma extensão da classe Pai. O seu ```constructor()``` não tem nada como parâmetro - a classe Filho não tem nada que é apenas seu, neste nosso exemplo.

Mas ele tem o ```super()``` que invoca a classe Pai e passa como parâmetro uma string, neste caso o sobrenome Silva. Sabemos que é o sobrenome porque ocupa a primeira posição (classe Pai pode receber até dois parâmetros - sobrenome e profissão). A segunda posição seria a profissão, mas como não passamos nada como segundo parâmetro... então a classe Pai assume que a profissão é Professor.

## Teste: criação do objeto filho

Vamos ao teste de criar um objeto filho e ver o que o ```console.log()``` nos mostra.

```javascript
// Teste: criação do objeto Filho
const filho = new Filho
console.log(filho)  // > Filho { sobrenome: 'Silva', profissao: 'Professor' }
```

No caso acima, nós criamos o objeto ```filho``` instanciando a classe Filho (```new Filho```). Veja que não passamos nada como parâmetro. Quando mostramos o ```console.log(filho)```, ele nos mostra que o objeto filho foi criado com o sobrenome Silva e a profissão Professor.

Consegue identificar de onde vem o ```Silva``` e o ```Professor``` deste objeto? A resposta está nos códigos.

Assunto de classes por aqui é encerrado.