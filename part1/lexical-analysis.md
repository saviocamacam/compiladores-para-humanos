Analise Léxica
======

### Introdução

A análise léxica e também conhecida como scanner ou leitura é a primeira fase de um processo de compilação e sua função é fazer a leitura do programa fonte, caractere a caractere, e traduzi-lo para uma sequência de símbolos léxicos ou tokens, os caracteres são agrupados em lexemas e produzem uma sequência de tokens, a sequência de tokens é enviado para a próxima fase da compilação que é a analise sintática. 

O analisador léxico deve interagir com a tabela de símbolos inserindo informações de alguns tokens, na maioria dos casos identificadores. A nível de implementação a analise léxica normalmente é uma sub-rotina da análise sintática formando um único passo, porem ocorre uma divisão conceitual para simplificar a modularização do projeto de um compilador. 

A análise léxica pode ser dividida em duas etapas, a primeira chamada de escandimento que é uma simples varredura removendo comentários e compactando espaços em branco e a segunda etapa é a  analise léxica propriamente dita onde é produzido os tokens.

Ao discutir analise léxica temos três termos relacionados:

* Token: é um par constituído de um nome é um valor de atributo que é opcional. O nome de um token é um símbolo que representa uma unidade léxica. Ex: palavras reservadas, identificadores.

* Padrão: e a forma que os lexemas de um token podem assumir. No caso de palavras reservadas é a sequência de caracteres da palavra reservada, no caso de identificadores são os caracteres que formam nomes das variáveis, funções.

* Lexema: é uma sequência de caracteres reconhecidos por um padrão.

A tabela abaixo mostra os exemplos de uso dos termos na análise léxica.

| Token        | Padrão                                              | Lexema                                         | Descrição                        |
|--------------|-----------------------------------------------------|------------------------------------------------|----------------------------------|
| const        | Sequência das palavras c, o, n, s, t                | const                                          | Palavra reservada                |
| while        | Sequência das palavras w, h, i, l, e                | while, While, WHILE                            | Palavra reservada                |
| if           | Sequência das palavras i, f                         | If, IF, iF, If                                 | Palavra reservada                |
| comparadores | <, >, <=, >=, ==, !=                                | ==, !=                                         |                                  |
| numero       | Dígitos numéricos                                   | 0.6, 18, 0.009                                 | Constante numérica               |
| literal      | Caracteres entre “”                                 | “Olá Mundo”                                    | Constante literal                |
| id           | Nomes de variáveis, funções, parâmetros de funções. | nomeCliente, descricaoProduto, calcularPreco() | Nome de variável, nome de função |
| atribuicao   | =                                                   | =                                              | Comando de atribuição            |
| delimitador  | {, }, [, ]                                          | {, }, [, ]                                     | Delimitadores de início e fim    |

Veja a identificação dos termos relacionados.

`printf(“Total = %d\n”, score)`

onde: 

* `printf` e `score` são lexemas que casão com o padrão `id`, `id` é um token.   

* `Total = %d\n` é um lexema que casa com o padrão `literal`, `literal` é um token.

outro exemplo:

`const PI = 3.1416`

onde:

`const` é um token que casa com o padrão `const` que também é um lexema.  `PI` é um lexema que casa com o padrão `id` e `id` é um token. `=` é um lexema que casa com o padrão do token `atribuição`. `3.1416` é um lexema que casa com o padrão do token `numero`.

### Tokens

Os tokens ou símbolos léxicos são unidades básicas do texto do programa, e cada token é representado por três informações básicas:

* Classe do token: é o tipo de token reconhecido. Por exemplo identificadores, constantes numéricas, cadeias de caracteres, palavras reservadas, operadores e separadores.

* Valor do token: depende da classe do token. Caso seja uma constante inteira representa o valor inteiro, ou no caso de identificadores um apontador para a tabela de símbolos.

* Posição do token: Indica a linha e a coluna no texto onde o token foi encontrado. É utilizado principalmente para indicar o local do erro.

Os tokens podem ser divididos em dois grupos:

* Tokens simples: São tokens que não tem valor associado pois a classe do token já a descreve. Ex: palavras reservadas, operadores, delimitadores.

* Tokens com argumento: são tokens que tem valor associado e corresponde a elementos da linguagem definidos pelo programador. Ex: identificadores, constantes numéricas.

Um token na seguinte estrutura:

`<nome-token, valor-atributo>`

Onde o nome do token corresponde classe do token e o valor do atributo corresponde a um valor qualquer que pode ser atribuído valor do token.

A seguir é apresentado alguns exemplos do resultado da análise léxica.

#### Primeiro exemplo:

Código fonte 

```
while indice < 10 do
	indice:= total + índice;
```

Sequência de tokens

<while,><id,7> <<,><numero,10><do,><id,7><:=,><id,12><+,><id, 7><;, >

#### Segundo exemplo

Código fonte

```
position = initial + rate * 60
```

Sequência de tokens

`<id, 1> <=, > <id, 2> <+, > <id, 3> <*, > <numero, 60>`


#### Terceiro exemplo

Código fonte

```
a[index] = 4 + 2
```

Sequência de tokens

`<id, 1> <[,> <id, 2> <],> <=,> <numero, 43> <+,> <numero, 2>`
	
### Exemplo de analise léxica

Suponha que tenhamos o seguinte trecho de código.

```
total = entrada * saida() + 2
```

O seguinte fluxo de tokens é gerado.

`<id, 15> <=, > <id, 20> <*, > <id,30> <+, > <numero, 2>`

Então temos os seguintes tokens:

* <id, 15> :  apontador 15 da tabela de símbolos e token id.
* <=, >  operador de atribuição, sem necessidade de um valor para o atributo.
* <id, 20> : apontador 20 da tabela de símbolos e token id.
* <*, > :  operador de multiplicação, sem necessidade de um valor para o atributo.
* <id,30> : apontador 20 da tabela de símbolos e token id.
* <+, > :  operador de soma, sem necessidade de um valor para o atributo.
* <numero, 2> :  token numero, com valor para o atributo 2 indicado o valor do numero (constante numérica).

### Tabela de símbolos

A tabela de símbolos é uma estrutura de dados gerada pelo compilador com o objetivo de armazenar informações sobre os identificadores e outros nomes usados no programa fonte. Exemplo de informações armazenadas na tabela de símbolos são: variáveis; parâmetros de funções; procedimentos. 

Essa tabela normalmente armazenas informações como: tipo de dado (inteiro, string, etc.), escopo de visibilidade; limite de parâmetros, tamanho da variável). A tabela de símbolos normalmente é estrutura de tabelas hash, arvores binarias ou listas lineares.

O analisador léxico coleta informações sobre os tokens e seus atributos, para tokens que influenciam decisões de análise gramatical, como exemplo temos os identificadores, é criado uma entrada na tabela de símbolos, das quais as informações são mantidas para posterior uso.  Podemos armazenar na tabela de símbolos também informação sobre a linha e coluna que o token foi examinado para em caso de erro o compilador pode informar a posição exata do identificador.

### Erros léxicos

A análise léxica é muito prematura para identificar erros de compilação, veja o exemplo abaixo.

`fi (a == “123”) ...`

O analisador léxico não conseguira identificar o erro da instrução listada acima, pois ele não consegue identificar que em determinada posição deve ser declarada a palavra reservada `if`. Essa verificação somente é possível ser feita na análise sintática.

Suponha que em uma situação o analisador léxico não pode prosseguir pois identificou que o conjunto de caracteres não pertence a nenhum padrão conhecido. Uma estratégia utilizada para a recuperação de erros léxicos e a modalidade pânico, que remove caracteres de entrada até encontrar um token bem formado

Alguma possibilidade de recuperação de erros:

* Remover caracteres estranhos.
* Inserir caracteres ausentes.
* Substituir caracteres corretos por incorretos.

### Exercícios 

1 - Descreva com as suas palavras as principais tarefas da analise léxica.

2 - Defina o que é um token.

3 - Defina o que é um lexema.

4 - Descreva quais os dois grupos que um token pode ser divido e explique cada um deles.

5 - Para a seguinte linha de código dívida a sequência de tokens e escreva descrição de cada token.

```
int a;
a = a + 2;
``
| Lexema | Descrição |
|--------|-----------|
|        |           |
|        |           |
|        |           |
|        |           |
|        |           |

6 - Para a seguinte linha de código dívida a sequência de tokens e escreva descrição de cada token.

```
if (x > 0) {
  x = 1;
} else { 
  x = 2;
}
```

| Lexema | Descrição |
|--------|-----------|
|        |           |
|        |           |
|        |           |
|        |           |
|        |           |




