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

> 
> O que é um verbo?
> 

<br>

![O que é um verbo](http://i.imgur.com/84GZzef.png)

<br>

>
> **... do ponto de vista semântico, contêm as noções de ação, processo ou estado**
>

<br>
<br>


> O que é uma função?

<br>

![O que é uma função](http://i.imgur.com/5SggnxS.png)

<br>

>
> **... são sub-rotinas que executam uma tarefa particular.**

<br>

<br>

```js

const usingClick = `click`
const THEMES = document.querySelectorAll( `.theme` )

const addingClickToThem = ( element ) => 
  element.addEventListener( usingClick, addThemes )

map( THEMES, addingClickToThem )

```


> A semântica contrapõe-se com frequência à sintaxe, caso em que a primeira se ocupa do que algo significa, enquanto a segunda se debruça sobre as estruturas ou padrões formais do modo como esse algo é expresso (por exemplo, as relações entre predicados e seus argumentos). Dependendo da concepção de significado que se tenha, têm-se diferentes semânticas.

*fonte: [https://pt.wikipedia.org/wiki/Sem%C3%A2ntica](https://pt.wikipedia.org/wiki/Sem%C3%A2ntica)*



Na língua portuguesa, o significado das palavras leva em consideração:

Sinonímia: É a relação que se estabelece entre duas palavras ou mais que apresentam significados iguais ou semelhantes, ou seja, os sinônimos. Exemplos: Cômico - engraçado / Débil - fraco, frágil / Distante - afastado, remoto.

Antonímia: É a relação que se estabelece entre duas palavras ou mais que apresentam significados diferentes, contrários, isto é, os antônimos: Exemplos. Economizar - gastar / Bem - mal / Bom - ruim.
Homonímia: É a relação entre duas ou mais palavras que, apesar de possuírem significados diferentes, possuem a mesma estrutura fonológica, ou seja, os homônimos.