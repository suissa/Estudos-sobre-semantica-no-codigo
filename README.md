# Estudos sobre aplicação da semântica no código

Veremos como escrever um código mais semântico para maior legibilidade.

![o que é semantica](http://i.imgur.com/txHthtg.png)

> **...o componente do sentido das palavras e da interpretação das sentenças e dos enunciados.**

Agora quero que você analise comigo o seguinte código:

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

**Para corroborar com minha ideia irei explicar o porquê acredito ser melhor da segunda forma.**

<br>

## Semântica

Eu pessoalmente acredito que os iniciantes não irão entender facilmente o código abaixo, por mais simples que seja.

```js

const result = Array.prototype.map.call( themes, ( button ) => {
  button.addEventListener(`click`, () => {} )
} )

```

Logo, a melhor forma de refatorarmos é encapsular essa lógica com um nome mais descritivo e simples para que possa ser reaproveitada, por exemplo:

```js

const map = ( list, fn ) => Array.prototype.map.call( list, fn )

```

Isso todo mundo sabe, porém acredito que o mais complicado semanticamente é a seguinte parte:


```js

button.addEventListener(`click`, () => {} )

```

Por isso separei o evento `click` onde iremos utiliza-lo para criar uma oração.

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

Mostrei esses dois conceitos porque eles são basicamente a mesma coisa, agora analise comigo os seguintes trechos:

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

Podemos então inferir, logicamente, que uma função **precisa ser uma oração** para seguir seu próprio conceito.

<br>
<br>

## Função

Como vimos anteriormente uma função precisa ser uma oração, logo seu nome **precisa** conter um verbo, por exemplo:

```js

const elementId = ( id ) => document.getElementById( id )

```

No caso acima a função `elementId` não é uma oração, ela precisa ser corrigida então vamos renomea-la da seguinte maneira:

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

Essas duas preposicoes, **por** e **com**, são chamadas: **essenciais**.

- [por: exprime, entre outras relações, causa, modo, tempo, meio, lugar, qualidade, estado, preço, etc](https://pt.wiktionary.org/wiki/por)
- [com: indica várias relações: companhia, causa, instrumento, ligação, modo, oposição](https://pt.wiktionary.org/wiki/com)

<br>

> 
> **\- Por que você acha que cheguei nesse conceito?**
> 

<br>

Analise comigo como essa funçao é usada e vamos ver se podemos melhorar sua semântica:

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

Perceba que traduzindo nossa oração tem-se o seguite sentido:

> 
> Você precisa adicionar um evento para só depois obter o elemento com tal id.
>

**Mas queremos que tenha o seguinte sentido:**

> 
> Você precisa adicionar um evento usando o elemento com tal id.
>

<br>

Sabendo disso podemos refatorar nosso código para a seguinte forma:

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

Agora vamos analisar como ficou o sentido da nossa oração:

```

add The Event `click` modifing Element With The Id `resetMap` for Reset The Map

adicionar o evento `click` modificando o elemento com o Id `resetMap` para redefinir o mapa

```

<br>

Percebeu que utilizei também o artigo `the/o` para que nossa oração tenha um sentido mais correto e conciso.

Os nomes das funções com certeza serão maiores porém muito mais descritivas, isso para mim que **ensino** é de suma importância para o fácil entendimento do código, ainda mais quando se usa Programaçao Funcional, pois a mesma pode deixar nosso código bem complexo para se ler.

<br>

Veja como é nosso código sem encapsular nenhuma lógica em funções:

```js

const buttonReset = document
                      .getElementById( `resetMap` )
                      .addEventListener( `click`,  () => {} )

```

<br>

> 
> **\- Bem mais simples né?**
> 
> **Porém nada reusvel!** E provarei-te o porquê.
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

Percebeu que temos muita duplicaçao de código? Logo as boas práticas no dizem que devemos encapsular essa lógica em uma funçao para poder reusa-la. 

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

> **"O código deve ser escrito para que humanos leiam e ocasionalmente máquinas"**

<br>

> A semântica contrapõe-se com frequência à sintaxe, caso em que a primeira se ocupa do que algo significa, enquanto a segunda se debruça sobre as estruturas ou padrões formais do modo como esse algo é expresso (por exemplo, as relações entre predicados e seus argumentos). Dependendo da concepção de significado que se tenha, têm-se diferentes semânticas.

*fonte: [https://pt.wikipedia.org/wiki/Sem%C3%A2ntica](https://pt.wikipedia.org/wiki/Sem%C3%A2ntica)*


Por exemplo, usando a mesma sintaxe mas com semânticas diferentes:

> 
> Eu vou com você para o evento.
> 
> Eu vou sem você para o evento.
> 

É a mesma sintaxe pois com e sem sao preposições essenciais, na qual uma é o antônimo da outra.

<br>

Traduzindo para JS:

```js

[1, 2, 3, 4].map( ( n ) => n * 2 )

```


```js

const multipliquePor2 = (n) => n * 2

[1, 2, 3, 4].map( multipliquePor2 )

```

<br>

Ou melhorando ela para deixar mais reusavel:

```js

const multipliquePor = ( multiplicador ) => (n) => n * multiplicador

[1, 2, 3, 4].map( multipliquePor( 2 ) )

```

Percebeu que usamos a função `map` em um *array*, passando uma função como parâmetro do `map`, agora eu lhe pergunto:

> **\- Quais dos códigos anteriores é mais legível e, o mais importante, reusavel?**



### Tempo Verbal

### Preposição

### Conjunção

### Artigo

### Pronome
