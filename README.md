# Estudos sobre semântica no codigo

Estudos de como escrever um codigo mais semântico para maior legibilidade.

![o que é semantica](http://i.imgur.com/txHthtg.png)

> **...o componente do sentido das palavras e da interpretação das sentenças e dos enunciados.**

Então quero que você analise comigo o seguinte código:

```js

const themes = document.querySelectorAll( `.theme` )

Array.prototype.map.call( themes, ( button ) => {
  button.addEventListener(`click`, () => {} )
} )

```

> 
> **\- Você achou bom?**
> 
> **\- E esse abaixo o que achas?**
> 

```js

const usingClick = `click`
const THEMES = document.querySelectorAll( `.theme` )

const map = ( list, fn ) => Array.prototype.map.call( list, fn )
const addingClickToThem = ( element ) => 
  element.addEventListener( usingClick, addThemes )

map( THEMES, addingClickToThem )

...

```

> 
> **\- Mais *"complicado"*?**
> 

Se você olhar por cima pode até parecer mais complexo, porém ele mostra-se mais legível e mais reusável.

<br>

**Para corroborar com essa minha ideia irei explicar o porquê que acredito ser melhor da segunda forma.**

<br>

## Semântica

Eu, pessoalmente, acredito que para quem esta iniciando nao ira entender facilmente o código abaixo, por mais simples que seja.

```js

const result = Array.prototype.map.call( themes, ( button ) => {
  button.addEventListener(`click`, () => {} )
} )

```

Logo o passo mais lógico para refatorarmos é encapsular essa lógica com um nome mais descritivo e simples, para que possa ser reaproveitada, por exemplo:

```js

const map = ( list, fn ) => Array.prototype.map.call( list, fn )

```

Isso todo mundo sabe, porém acredito que o mais complicado, semanticamente, é a seguinte parte:


```js

button.addEventListener(`click`, () => {} )

```

Por isso separei qual evento, `click`, iremos utilizar para criar uma oração.

> "A oração é uma unidade sintática. Trata-se de um enunciado linguístico cuja estrutura caracteriza-se, obrigatoriamente, pela presença de um verbo. Na verdade, a oração é caracterizada, sintaticamente, pela presença de um predicado, o qual é introduzido na língua portuguesa pela presença de um verbo. Geralmente, a oração apresenta um sujeito, termos essenciais, integrantes ou acessórios."

*fonte: [http://portugues.uol.com.br/gramatica/frase-oracao-periodo.html](http://portugues.uol.com.br/gramatica/frase-oracao-periodo.html)*

Por exemplo:

> Corra!
> 
> Choveu ontem.
> 
> Ele programa em JS.

<br>

**O conceito oração me remete a outros dois conceitos**

<br>
<br>
<br>

![O que é um verbo](http://i.imgur.com/84GZzef.png)

![O que é uma função](http://i.imgur.com/5SggnxS.png)

<br>

<br>

Mostrei esses dois conceitos pois eles basicamente são a mesma coisa, analise comigo os seguintes trechos:

<br>

>
> **... do ponto de vista semântico, contêm as noções de ação, processo ou estado**
>
> **... são sub-rotinas que executam uma tarefa particular.**
> 

<br>

>**\- Concorda comigo agora?**

<br>
<br>

Logo podemos inferir, logicamente, que uma função **precisa ser uma oração** para seguir seu próprio conceito.

<br>
<br>

## Função

Como vimos anteriormente, uma função precisa ser uma oração, logo seu nome **precisa** conter um verbo, por exemplo:

```js

const elementId = ( id ) => document.getElementById( id )

```

No caso acima, a função `elementId` nao é uma oracao, para corrigi-la podemos renomea-la da seguinte maneira:

```js

const getElementWithId = ( id ) => document.getElementById( id )

```


- by id: por id
- with: com id

Isso ira nos remeter ao conceito de preposição.

![preposição](http://i.imgur.com/P0f5Abn.png)

> 
> "... liga dois elementos de uma frase, estabelecendo uma relação entre eles."
> 

>
> "As preposições são muito importantes na estrutura da língua pois estabelecem a coesão textual e possuem valores semânticos indispensáveis para a compreensão do texto."
>

*fonte: [http://www.infoescola.com/portugues/preposicao/](http://www.infoescola.com/portugues/preposicao/)*

Essa duas preposicoes, por e com; sao preposicoes chamadas: **essenciais**.

- [por: exprime, entre outras relações, causa, modo, tempo, meio, lugar, qualidade, estado, preço, etc](https://pt.wiktionary.org/wiki/por)
- [com: indica várias relações: companhia, causa, instrumento, ligação, modo, oposição](https://pt.wiktionary.org/wiki/com)

<br>

> 
> **\- Por que você acha que cheguei nesse conceito?**
> 

<br>

Analise comigo como essa funçao é usada e vamos ver se podemos melhorar sua semantica:

<br>

```js

const getElementWithId = ( id ) => document.getElementById( id )

// ...

const buttonReset = addEventTo( getElementWithId( `resetMap` ),
                                            usingClick, 
                                            forResetMap )
```

<br>

```

add Event To get Element With Id
Adicionar evento para obter elemento com id

```

<br>

Perceba, que traduzindo, nossa oracao tem o seguite sentido:

> 
> Você precisa adicionar um evento para só depois obter o elemento com tal id.
>

**Mas queremos que tenha o seguinte sentido:**

> 
> Você precisa adicionar um evento usando o elemento com tal id.
>

<br>

Sabendo disso, podemos refatorar nosso código para a seguinte forma:

<br>

```js

const getElementById = ( id ) => document.getElementById( id )
const modifyingTheElementWithTheId = ( id ) => getElementById( id )

const addTheEvent = ( el, event, andRun ) => 
  el.addEventListener( event, andRun ) 

const forResetMap = () => {}

const buttonReset = addTheEvent( `click`, 
                              modifyingTheElementWithTheId( `resetMap` ),
                              forResetTheMap )
```

<br>

Agora vamos analisar como ficou o sentido da nossa oracao:

```

add The Event `click` modifing Element With The Id `resetMap` for Reset The Map

adicionar o evento `click` modificando o elemento com o Id `resetMap` para redefinir o mapa

```

<br>

Percebeu que utilizei também o artigo `the/o` para que nossa oraçao tenha um sentido mais correto e conciso.

Os nomes das funçoes com certeza serao maiores, porém muito mais descritivas e isso, para mim que **ensino**, é de suma importancia para o facil entendimento do codigo ainda mais quando se usa Programaçao Funcional, pois a mesma pode deixar nosso código bem complexo para se ler.

<br>

Veja como é nosso código sem encapsular nenhuma lógica em funçoes:

```js

const buttonReset = document
                      .getElementById( `resetMap` )
                      .addEventListener( `click`,  () => {} )

```

<br>

> 
> **\- Bem mais simples né?**
> 
> **Porém nada reusavel!** E vou te provar o porquê.
> 

<br>

Óbvio que para 1 elemento é de boas, entretanto vamos ver como fica esse código quando possuimos mais elementos para adicionarmos algum evento.

```js

const buttonReset = document
                      .getElementById( `getDomainLocation` )
                      .addEventListener( `click`,  () => {} )

const buttonReset = document
                      .getElementById( `getOwnLocation` )
                      .addEventListener( `click`,  () => {} )

const buttonReset = document
                      .getElementById( `resetMap` )
                      .addEventListener( `click`,  () => {} )

```

<br>

Percebeu que temos muita duplicaçao de código? Logo as boas praticas no dizem que devemos encapsular essa lógica em uma funçao para poder reusa-la. 

<br>

```js

const getElementById = ( id ) => document.getElementById( id )
const modifyingTheElementWithTheId = ( id ) => getElementById( id )

const addTheEvent = ( el, event, andRun ) => 
  el.addEventListener( event, andRun ) 

const forResetMap = () => {}

const buttonGetDomainLocation = addTheEvent(  `click`, 
                                              modifyingTheElementWithId( `getDomainLocation` ),
                                              forGetDomainLocation )

const buttonGetYourOwnLocation = addTheEvent( `click`, 
                                              modifyingTheElementWithId( `getOwnLocation` ),
                                              forGetYourOwnLocation )

const buttonReset = addTheEvent(  `click`, 
                                  modifyingTheElementWithId( `resetMap` ),
                                  forResetTheMap )
```

Isso me recordou a seguinte frase:

> **"O código deve ser escrito para que humanos leiam e ocasionalmente maquinas"**

<br>

> A semântica contrapõe-se com frequência à sintaxe, caso em que a primeira se ocupa do que algo significa, enquanto a segunda se debruça sobre as estruturas ou padrões formais do modo como esse algo é expresso (por exemplo, as relações entre predicados e seus argumentos). Dependendo da concepção de significado que se tenha, têm-se diferentes semânticas.

*fonte: [https://pt.wikipedia.org/wiki/Sem%C3%A2ntica](https://pt.wikipedia.org/wiki/Sem%C3%A2ntica)*



Na língua portuguesa, o significado das palavras leva em consideração:

Sinonímia: É a relação que se estabelece entre duas palavras ou mais que apresentam significados iguais ou semelhantes, ou seja, os sinônimos. Exemplos: Cômico - engraçado / Débil - fraco, frágil / Distante - afastado, remoto.

Antonímia: É a relação que se estabelece entre duas palavras ou mais que apresentam significados diferentes, contrários, isto é, os antônimos: Exemplos. Economizar - gastar / Bem - mal / Bom - ruim.
Homonímia: É a relação entre duas ou mais palavras que, apesar de possuírem significados diferentes, possuem a mesma estrutura fonológica, ou seja, os homônimos.