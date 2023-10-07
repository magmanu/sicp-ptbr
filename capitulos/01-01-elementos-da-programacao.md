# 1.1 Os Elementos da Programação

Uma linguagem de programaçãoo poderosa é mais do que um simples meio de instruir um computador a realizar tarefas. A linguagem também serve como o enquadramento pelo qual organizamos nossas ideias sobre os processos. Assim, quando descrevemos uma linguagem, deveríamos prestar atenção especialmente às formas que a linguagem oferece de combinar ideias simples para formar ideias mais complexas. Toda linguagem poderosa tem três mecanismos para isso:

- **expressões primitivas**, que representam as entidades mais simples da linguagem em questão,
- **meios de combinação**, que criam elementos compostos a partir de elementos mais simples,
- **meios de abstração**, que dão nome a elementos compostos e os tratam como unidades.

Em programação, lidamos com dois tipos de elementos: procedimentos e dados (mais tarde descobriremos que eles não são tão diferentes assim). Informalmente, dados são “coisas” que queremos manipular; procedimentos são descrições das regras que manipulam os dados. Assim, qualquer linguagem de programação poderosa deveria conseguir descrever dados primitivos e procedimentos primitivos, e deriam fornecer métodos para combinar e abstrair processos e dados.

Neste capítulo, vamos lidar apenas com dados numéricos simples para nos focarmos nas regras que constroem os procedimentos.^[Chamar números de “dados simples” é um bluff descarado. Na verdade, a forma de lidar com números é um dos aspectos mais difíceis e confusos em qualquer linguagem de programação. Alguns dos problemas típicos são: alguns sistemas de computador fazem distinção entre *números inteiros*, como 2, e *números reais*, como  2,71. O número real 2,00 é diferente do número inteiro 2? As operações aritiméticas usadas em números inteiros são iguais às operações usadas em números reais? A divisão de 6 por 2 resulta em 3 ou em 3,0? Quão grandes podem ser os números representados? Com quantas casas decimais de precisão podemos representar os números? O intervalo de números inteiros é o mesmo que o intervalo de números reais? Além de todas essas questões, claro, há um conjunto de problemas relacionados a erros de truncamento e arredondamento — toda a ciência de análise numérica. Como o foco deste livro é o design de programas de larga escala, e não técnicas numéricas, vamos ignorar esses problemas. Os exemplos numéricos neste capítulo seguirão o comportamento normal de arredondamento usado em operações aritiméticas que mantêm um número limitado de casas decimais de precisão em operações de números não naturais.] Em capítulos posteriores, veremos que essas mesmas regras nos permitem construir procedimentos para manipular dados compostos também.

## 1.1.1 Expressões

Uma maneira fácil de começar a programar é examinar algumas interações típicas com um interpretador do dialeto Scheme do Lisp. Imagine que você está em frente ao terminal de um computador. Você digita uma *expressão* e o interpretador responde, exibindo o resultado da *avaliação* daquela expressão.

Um tipo de expressão primitiva que você pode digitar é um número (mais precisamente, a expressão que você digita consiste em numerais que representam números decimais). Se você der ao Lisp o número

``` {.scheme}
486
```

o interpretador responderá exibindo^[Ao longo deste livro, fazemos a distinção entre o input humano e a resposta do interpretador mostrando a esta última em itálico.]

*486*

Expressões que representam números podem ser combinadas com expressões que representam um procedimento primitivo (por examplo, `+` ou `*`) para formar uma expressão composta que representa a aplicação de tal procedimento a esses números. Por exemplo:

``` {.scheme}
(+ 137 349)
```

_486*

``` {.scheme}
(- 1000 334)
```

_666*

``` {.scheme}
(* 5 99)
```

_495*

``` {.scheme}
(/ 10 5)
```

_2*

``` {.scheme}
(+ 2.7 10)
```

_12.7*

Expressões como essas, formadas ao delimitar uma lista de expressões entre parênteses para significar a aplicação de um procedimento, são chamadas de *combinações*. O elemento mais à esquerda é o *operador*, e os outros elementos são os *operandos*. O valor de uma combinação é obtida ao se aplicar o procedimento determinado pelo operador aos *argumentos*, que são os valores dos operandos.

A convenção de posicionar o operador à esquerda dos operandos é conhecida como *notação de prefixo*, e pode ser um pouco confuso no começo porque é significativamente diferente da convenção matemática convencional. Porém, a notação de prefixo traz várias vantagens. Uma delas é que ela pode acomodar procedimentos que recebem um número arbitrário de argumentos, como nos examplos a seguir:

``` {.scheme}
(+ 21 35 12 7)
```

_75*

``` {.scheme}
(* 25 4 12)
```

_1200*

Não há margem para ambiguidade porque o operador está sempre mais à esquerda e toda a combinação está contida entre parênteses.

Outra vantagem da notação de prefixo é que ela pode ser ampliada de forma clara para permitir que as combições sejam *aninhadas*, ou seja, as próprias combinações podem ter elementos que sejam outras combinações:

``` {.scheme}
(+ (* 3 5) (- 10 6))
```

_19*

Em princípio, não há limite para o nível de aninhamento nem para a complexidade geral das expressões que o interpretador do Lisp avalia. Somos nós, humanos, que ficamos confusos com expressões ainda relativamente simples, como

``` {.scheme}
(+ (* 3 (+ (* 2 4) (+ 3 5))) (+ (- 10 7) 6))
```

que o interpretador rapidamente avaliaria como 57. Para facilitar a nossa vida, podemos escrever a expressão como

``` {.scheme}
(+ (* 3
      (+ (* 2 4)
         (+ 3 5)))
   (+ (- 10 7)
      6))
```

seguindo uma conveção de formatação chamada *pretty-printing*, em que cada combinação longa alinha os operandos verticalmente. O recuo resultante mostra claramente a estrutura da expressão.^[Sistemas Lisp tipicamente oferecem recursos para ajudar na formatação de expressões. Dois recursos especialmente úteis são o recuo automático que exibe a posição em *pretty-printing* quando uma nova linha é criada e o recurso que destaca o parêntese esquerdo correspondente sempre que um parêntese é digitado.]

Mesmo em expressões complexas, o interpretador sempre opera o mesmo ciclo básico: lê a expressão do terminal, avalia a expressão e exibe o resultado. Geralmente se diz, nesse modo de operação, que o interpretador é executado como *read-eval-print loop* (*loop* ler-avaliar-exibir, também conhecido como REPL). Observe em particular que não é necessário instruir o interpretador explicitamente a exibir o valor da expressão.^[O Lisp segue a convenção de que toda expressão tem um valor. Esta convenção, juntamente com a velha reputação do Lisp como uma linguagem ineficiente, é o motivo pelo qual Alan Perlis (parafraseando Oscar Wilde) fez a provocação “programadores de Lisp sabem o valor de tudo mas não sabem o preço de nada”.]

## 1.1.2 Nomes e o Ambiente

Um aspecto crítico da uma linguagem de programação são as formas de usar nomes para nos referimos a objetos computacionais. Dizemos que o nome identifica a *variável* cujo *valor* é o objeto.

No dialeto Scheme do Lisp, damos nome às coisas usando o termo `define`. Quando digitamos

``` {.scheme}
(define tamanho 2)
```

fazemos com que o interpretador associe o valor 2 ao nome `tamanho`.^[Neste livro, não mostraremos a resposta do interpretador à avaliação de definições, já que depende muito da implementação.] Depois que o nome `tamanho` é associado ao número 2, podemos então nos referir ao valor 2 pelo nome:

``` {.scheme}
tamanho
```

_2*

``` {.scheme}
(* 5 tamanho)
```

_10*

Veja abaixo outros exemplos do uso de `define`:

``` {.scheme}
(define pi 3.14159)
(define raio 10)

(* pi (* raio raio))
```

_314.159*

``` {.scheme}
(define circunferência (* 2 pi raio))

circunferência
```

_62.8318*

`Define` é a forma de abstração mais simples da nossa linguagem, porque nos permite usar nomes simples para nos referirmos aos resultados de operações compostas, como a `circunferência` computada acima. No geral, objetos computacionais podem ter estruturas muito complexas, e seria extremamente inconveniente ter que lembrar e repetir os detalhes cada vez que os usássemos. De fato, programas complexos são construídos quando criamos, passo a passo, objetos computacionais cada vez mais complexos. O interpretador é bastante conveniente na construção do passo-a-passo do programa porque as associações nome-objeto podem ser criadas de forma incremental em iterações sucessivas. Essa característica encoraja o desenvolvimento e o teste incrementais de programas e é em grande parte responsável pelo fato de que um programa Lisp geralmente é formado por um grande número de procedimentos relativamente simples.

Deveria estar claro que a possibilidade de associar valores a símbolos e depois recuperá-los significa que o interpretdor deve manter alguma tipo de memória que rastreia os pares nome-objeto. Essa memória é chamada de  *ambiente* (mais precisamente, o *ambiente global*, já que veremos mais tarde que uma computação pode envolver diversos ambientes).^[No [capítulo 3](03-00-modularidade-objetos-estado.md) mostraremos que esta notação de ambiente é crucial, tanto para compreender como o interpretador funciona quanto para implementar interpretadores.]

## 1.1.3 Avaliação de Combinações

Um dos nossos objetivos neste capítulo é isolar problemas sobre o pensamento procedimental. Neste caso, vamos supor que, ao avaliar combinações, o próprio interpretador segue um procedimento.

> Para availar uma combinação, faça o seguinte:
> >
> > 1. Avalie a sub-expressão da combinação.
> >
> 2. Aplique o procedimento, que é o valor da sub-expressão mais à esquerda (o operador), aos argumentos, que são os valores das outras sub-expressões (os operandos).

Até essa regra simples ilustra alguns pontos importantes sobre processos no geral. Primeiro, observe que a primeira etapa determina que, para conseguir avaliar o processo de uma combinação, precisamos primeiro fazer o processo de avaliação em cada elemento da combinação. Assim, a regra de avaliação é de natureza *recursiva*; ou seja, ela inclui, como uma de suas etapas, a necessidade de invocar a própria regra.^[Pode parecer estranho que a regra de avaliação diga que, na primeira etapa, devemos avaliar o elemento mais à esquerda da combinação, já que neste ponto só pode existir um operador como `+` ou `*`, que representa um procedimento primitivo nativo como adição ou multiplicação. Veremos mais tarde que isso é útil para trabalhar com combinações cujos próprios operadores são expressões compostas.]

Repare como a ideia de recursão pode ser usada de forma sucinta para expressar o que seria de outra forma visto, no caso de combinações aninhadas em profundidade, como um processo meio complicado. Por exemplo, a avaliação de

``` {.scheme}
(* (+ 2 (* 4 6)) (+ 3 5 7))
```

exige que a regra de avaliação seja aplicada em quatro combinações diferentes. Podemos criar uma imagem desse processo ao represnetar a combinação na forma de uma uma árvore, como mostra a [Figura 1.1](#Figure-1_002e1). Cada combinação é representada por um nó com ramos correspondentes ao operador e os operandos que surgem dele. Os nós terminais (ou seja, nós dos quais não brotam ramos) representam operadores ou números. Vendo a avaliação na árvore, podemos imaginar que os valores dos operandos fluem de baixo para cima, começando dos nós terminais e a seguir, fazendo combinações em níveis cada vez mais acima. No geral, veremos que essa recursão é uma técnica muito poderosa para lidar com objetos hierárquicos em forma de árvore. Na verdade, a regra de avaliação com “valores que fluem de baixo para cima” é um exemplo de um tipo geral de processo conhecido como *acumulação de árvores*.

![](fig/chap1/Fig1.1g.std.svg 244.56x245.52)
**Figure 1.1:** Representação de árvore, mostrando o valor de cada subcombinação.

A seguir, obseve que a aplicação repetida da primeira etapa nos traz ao ponto em que precisamos avaliar não as combinações, mas expressões primitivas como numerais, operadores nativos ou outros nomes. Lidamos com os casos primitivos estipulando que

- os valores dos numerais são os números que lhes dão nome,
- os valores dos operadores nativos são as sequências de instruções da máquina que executam as operações correspondentes, e
- os valores de outros nomes são objetos associados a esses nomes no ambiente.

Podemos considerar que a segunda regra é um caso especial da terceira ao determinar que símbolos como `+` e `*` também estão incluídos no ambiente global, e são associados às sequências de instruções da máquina, que são seus “valores”. O principal ponto a reparar é o papel do ambiente em determinar o significado dos símbolos nas expressões. Em uma linguagem interativa como o Lisp, não faz sentido falar do valor de uma expressão como `(+ x 1)` sem especificar qualquer informação sobre o ambiente que daria significado ao símbolo `x` (ou mesmo ao símbolo `+`). Como veremos no [Capítulo 3](Chapter-3.xhtml#Chapter-3), a noção geral de que o ambiente fornece o contexto em que a avaliação acontece terá um papel importante no nosso entendimento da execução do programa.

Observe que a regra de avaliação dada acima não lida com definições. Por exemplo, avaliar `(define x 3)` não aplica `define` a dois arugmentos, um dos quais é o valor do símbolo `x` e de 3, já que o propósito de `define` é precisamente associar `x` a um valor (ou seja, `(define x 3)` não é uma combinação.)

Tais exceções à regra de avaliação geral são chamadas *formas especiais*. `Define` foi o único exemplo de uma forma especial que vimos até agora, mas vamos encontrar outros em breve. Cada forma especial tem sua própria regra de avaliação. Os vários tipos de expressões (cada um com sua regra de avaliação associada) criam a sintaxe da linguagem de programação. Em comparação com outras linguagens, o Lisp tem uma sintaxe simples, ou seja, a regra de avaliação para expressões pode ser descrita por uma regra geral simples junto com regras especializadas de um pequeno número de formas especiais.^[Formas sintáticas especiais que são apenas uma estrutura superficial alternativa e conveniente para coisas que podem ser escritas de maneira mais uniforme são por vezes chamadas de *açúcar sintático*, usando um termo criado por Peter Landin. Comparando com outras linguagens, programadores de Lisp via de regra preocupam-se menos com sintaxe. (Contraste isso com qualquer manual de Pascal e repare como as descrições de sintaxe ocupam um grande espaço.) Esse desdém por sintaxe se deve parte à flexibilidade do Lisp, que facilita mudar a sintaxe superficial, e parte à observação de que muitas construções sintáticas “convenientes”, que deixam a linguagem menos uniforme, acabam causando mais problemas do que benefícios quando os programas se tornam grandes e complexos. Nas palavras de Alan Perlis, “Açúcar sintático causa câncer no ponto e vírgula” (em tradução literal. O original é um trocadilho entre “semicolon”, ponto e vírgula, e “colon”, o reto).]

## 1.1.4 Procedimentos Compostos

Identificamos no Lisp alguns dos elementos que devem aparecer em qualquer linguagem de programação poderosa:

- Números e operações aritiméticas são dados e procedimentos primitivos.
- O aninhamento de combinações permite combinar operações.
- Definições que associam nomes a valores proporcionam meios limitados de abstração.

Agora vamos aprender sobre *definições de procedimento*, uma técnica muito mais poderosa de abstrações em que uma operação composta pode receber um nome e referida a seguir como sendo uma unidade.

Começaremos examinando como expressar a idea de “elevar ao quadrado”. Podemos dizer que “Para elevar ao quadradado um elemento, multiplique o elemento por si mesmo”. Em nossa linguagem, isso seria expresso como

``` {.scheme}
(define (quadrado x) (* x x))
```

We can understand this in the following way:

``` {.example}
(define (     quadrado             x      ) (*             x           x     ))
  |               |                |         |             |           |
 Para      elevar ao quadrado um elemento,  multiple o elemento por si mesmo.
```

Aqui temos um *procedimento composto*, que recebeu o nome de `quadrado`. O procedimento representa a operação que multiplica algo por si mesmo. A coisa a ser multiplicada recebe um nome local, `x`, que tem o mesmo papel que um pronome em uma linguagem humana. Avaliar a definição cria este procedimento composto e o associa ao nome `quadrado`.^[Observe que hã duas operações diferentes sendo combinadas aquiÇ estamos criando o procedimento, e dando-lhe o nome `quadrado`. É possível, e até importante, conseguir separar essas duas noções: a de criar procedimentos sem lhes dar algum nome, e a noção de dar nomes a procedimentos que já foram criados. Vamos ver como fazer isso na seção [1.3.2](1_002e3.xhtml#g_t1_002e3_002e2).]

A forma geral de definir um procedimento é

(define (*⟨nome⟩ ⟨parâmetros formais⟩*) *⟨corpo⟩*)

O *`⟨nome⟩`* é um símbolo a ser associado com a definição do procedimento no ambiente.^[Ao longo deste livro, vamos descrever a sintaxe geral das expressões usando termos em itálico cercados por colchetes (por exemplo, *`⟨nome⟩`*) para significar as “casas” na expressão que devem ser preenchidas quanto tal expressão é usada.] Os *`⟨parâmetros formais⟩`* são os nomes usados dentro do corpo do procedimento para referir aos argumentos correspondentes do procedimento. O *`⟨corpo⟩`* é uma expressão que carrega o valor da aplicação do procedimento quando os parâmetros formais são substituídos pelos verdadeiros argumentos ao qual o procedimento é aplicado.^[De forma geral, o corpo d procedimento pode ser uma sequência de expresões. Nesse caso, o interpretador avalia cada expressão por vez e retorna o valor da expressão final como o valor da aplicação do procedimento.] O *`⟨nome⟩`* e os *`⟨parâmetros formais⟩`* ficam agrupados entre parênteses, como ficariam se fossem uma chamada verdadeira ao procedimento sendo definido.

Tendo definido `quadrado`, agora podemos usá-lo:

``` {.scheme}
(quadrado 21)
```

*441*

``` {.scheme}
(quadrado (+ 2 5))
```

49

``` {.scheme}
(quadrado (quadrado 3))
```

81

Também podemos usar `quadrado` como uma peça fundamental na definição de outros procedimentos. Por exemplo, $x^{2} + y^{2}$ pode ser expresso como

``` {.scheme}
(+ (quadrado x) (quadrado y))
```

Podemos facilmente definir um procedimento `soma-dos-quadrados` em que, dados dois números como argumentos, producs a seguinte soma de seus quadrados:

``` {.scheme}
(define (soma-dos-quadrados x y)
  (+ (quadrado x) (quadrado y)))

(soma-dos-quadrados 3 4)
25
```

Agora podemos usar `soma-dos-quadrados` como uma peça fundamental na construção de outros procedimentos:

``` {.scheme}
(define (f a)
  (soma-dos-quadrados (+ a 1) (* a 2)))

(f 5)
136
```

Procedimentos compostos são usados exatamente da mesma forma que procedimentos primitivos. Defato, não seria possível dizer a diferença entre `soma-dos-quadrados` apenas ao olhar se o `quadrado` é nativo do interpretador, como `+` e `*`, ou se foi definido como um procedimento composto.

## 1.1.5 O Modelo de Substituição para Aplicação de Procedimentos

Para avaliar uma combinação cujos operador nomeia um procedimento composto, o interpretador segue o mesmo processo que as combinações cujos operadores nomeiam procedimentos primitivos que descrevemos em [1.1.3](#g_t1_002e1_002e3). Isto é, o interpretador avalia os elementos da combinação e aplica o procedimento (que é o valor do operador da combinação) aos argumentos (que são os valores dos operandos da combinação).

Podemos presumir que o mecanismo para aplicar procedimentos primitivos a argumentos é construído pelo interpretador. Para procedimentos compostos, o processo de aplicação é o seguinte:

> Para aplicar um procedimento composta a argumentos, avalie o corpo do procedimento com cada parâmetro formal substituído pelo argumento correspondente.

Para ilustrar esse processo, vamos avaliar a combinação

``` {.scheme}
(f 5)
```

onde `f` é o processo definido em [1.1.4](#g_t1_002e1_002e4). Começamos obtendo o corpo de `f`:

``` {.scheme}
(soma-dos-quadrados (+ a 1) (* a 2))
```

A seguir, substituímos o parâmetro formal `a` pelo argumento 5:

``` {.scheme}
(soma-dos-quadrados (+ 5 1) (* 5 2))
```

Assim, o problema se reduz à avaliação de uma combinação com dois operandos e um operador `soma-dos-quadrados`. Avaliar esta combição envolver três sub-problemas. Precisamos avaliar o operador para obter o procedimento a ser aplicado, e devemos avaliar os operandos para obter os argumentos. Agora `(+ 5 1)` produz 6 and `(* 5 2)` produz 10, portanto devemos aplicar o procedimento `soma-dos-quadrados`a 6 e 10. Esses valores são substituídos pelos parâmetros formais `x` e `y` no corpo de `soma-dos-quadrados`, reduzindo a expressão a

``` {.scheme}
(+ (quadrado 6) (quadrado 10))
```

Se usarmos a definição de `quadrado`, isso reduz-se a

``` {.scheme}
(+ (* 6 6) (* 10 10))
```

que reduz a multiplicação a

``` {.scheme}
(+ 36 100)
```

e, por fim, a

``` {.scheme}
136
```

O processo que acabamos de descrever é chamado de  *modelo de substituição* na aplicação do procedimento. Pode ser considerado um modelo que determina o “significado” da aplicação do modelo, of procedure application, no que diz respeito aos procedimentos deste capítulo. No entanto, há dois pontos a serem destacados:

- O propósito da substituição é nos ajudar a pensar sobre a aplicação do procedimento, não é prover uma descrição de como o interpretador realmente funciona. Interpretadores típicos não avaliam aplicações de procedimentos através da manipulação o texto de um procedimento para substituir os valores pelos parâmetros formais. Na prática, a “substituição” é realizada usando o ambiente local para os parâmetros formais. Vamos discutir isso de forma mais completa no [Capítulo 3](Chapter-3.xhtml#Chapter-3) e no [Capítulo 4](Chapter-4.xhtml#Chapter-4) quando examinarmos a implementação de um interpretante em detalhes.
- Ao longo deste livro, vamos apresentar uma sequência cada vez mais elaborada de modelos de funcionamento de interpretadores, culminando com uma implementação completa de um interpretador e um compilador no [Capítulo 5](Chapter-5.xhtml#Chapter-5). O modelo de substituição é apenas o primeiro desses modelos - uma forma de começar a pensar formalmente sobre o processo de avaliação. No geral, ao modelar fenômenos em ciência e engenharia, começamos com modelos simplificados e incompletos. Ao examinar as coisas em maior detalhe, esses modelos simples se tornam inadequados e devem ser substituídos por modelos mais refinados. O modelo de substituição não é uma exceção. Em particlar, quando chegarmos no [Capítulo 3](Chapter-3.xhtml#Chapter-3), o uso de procedimentos com “dados mutáveis”, veremos que o modelo de substituição não resiste e deve ser substituído por um modelo mais complicado de aplicação de procedimentos.^[A despeito da simplicidade da ideia de substituição, é surpreendentemente complicado dar uma definição matemática rigorosa do processo de substituição. O problema vem da possibilidade de confusão entre nomes usados nps npmes usados em parâmetros formais de um procedimento e os nomes (possivelmente idênticos) usados em expressões para os quais os procedimentos podem ser aplicados. De fato, há um longo histórico de definições incorretas de *substituição* na literatura da semântica da lógica e da programação. Consulte [Stoy 1977](References.xhtml#Stoy-1977) para conhecer uma discussão cuidadosa sobre a substituição.]

### Ordem Aplicativa versus Ordem Normal

De acordo com a descrição de avaliação dada em [1.1.3](#g_t1_002e1_002e3), o interpretante primeiro avalia o operador e os operando e depois aplica o procedimento resultante aos argumentos resultantes. Essa não é a única maneira de realizar uma avaliação. Um modelo alternativo de avaliação não avaliaria operandos até que seus valores fossem necessários. Em vez disso, iria primeiramente substituir expressões de operandos por parâmetros até obter uma expressão envolvendo apenas operadores primitivos, e então realizar a avaliação. Se usássemos esse métido, a avaliação de `(f 5)` aconteceria de acordo com a sequência de expansões

``` {.scheme}
(soma-dos-quadrados (+ 5 1) (* 5 2))

(+ (quadrado (+ 5 1))
   (quadrado (* 5 2)))

(+ (* (+ 5 1) (+ 5 1)) 
   (* (* 5 2) (* 5 2)))
```

seguida pelas reduções

``` {.scheme}
(+ (* 6 6) 
   (* 10 10))

(+ 36 100)

136
```

Esta é a mesma resposta dada pelo modelo de avaliação anterior, mas o processo é diferente. Em particular, as avaliações de`(+ 5 1)` e `(* 5 2)` são executadas duas vezes cada neste modelo, corespondendo â redução da expressão `(* x x)` tendo `x` substituído respectivamente por `(+ 5 1)` e `(* 5 2)`.

O método alternativo de avaliação “expandir completamente e depois reduzir” é conhecido como  *avaliação em ordem normal*, em contraste com o método “avaliar os argumentos e depois aplicar”, que o interpretador realmente usa, que é chamado de *avaliação de applicative-order*. Pode ser demonstrado que, para alicações de procedimentos que podem ser modelados usando substituição (incliond todos os procedimentos nos primeiros dois capítulos deste livro) e que produzem valores legítimos, avaliações de ordem normal e de *applicative-order* produzem o mesmo valor. (Consulte o [Exercício 1.5](#Exercise-1_002e5) para conhecer um valor “ilegítimo” em que a avaliação em ordem normal e em  *applicative-order* não produzem o mesmo resultado.)

O Lisp usa a avaliação *applicative-order*, parcialmente devido à eficiência adicional obtida ao evitar múltiplas avaliações de expressões como as ilustradas em `(+ 5 1)` e `(* 5 2)` acima, e, de forma mais significativa, porque a avaliação em ordem normal se torna muito mais complicada de lidar quando deixamos o âmbito dos procedimentos que podem ser modelados por substituição. Por outro lado, a avaliação em ordem normal pode ser uma ferramenta extremamente valiosa, e investigaremos algumas de suas implicações no [Capítulo 3](Chapter-3.xhtml#Chapter-3) e no [Capítulo 4](Chapter-4.xhtml#Chapter-4).^[No [Capítulo 3](Chapter-3.xhtml#Chapter-3) vamos introduzirt  will introduce *stream processing* (processamento de fluxo), que é uma forma de lidar com estruturas de dados aparentemente “infinitas” ao incorporar uma forma limitada de avaliação de ordem normal. Na seção [4.2](4_002e2.xhtml#g_t4_002e2) vamos modificar o interpretador do Scheme para produzir uma variante de ordem normal do Scheme.]

## 1.1.6 Expressões Condicionais e Predicados

O poder expressivo da classe de procedimentos que podemos definir agora é muito limitado, porque não temos como fazer testes e executar diferentes operações dependendo do resultado de um teste. Por exemplo, não podemos definir um procedimento que computa o valor absoluto de um número testando se o número é positivo, negativo ou zero, nem executar ações nos três casos de acordo com a regra

$$\left| x \right|\; = \;\left\{ \begin{array}{lll}
x & {\;\text{se}} & {x > 0,} \\
0 & {\;\text{se}} & {x = 0,} \\
{- x} & {\;\text{se}} & {x < 0.} \\
\end{array} \right.$$

 Esta construção é chamada *análise de caso*, e há uma forma especial no Lisp para fazer a notação de tal análise de caso. Chama-se `cond` (abreviação de “condicional”), e é usada da seguinte forma:

``` {.scheme}
(define (abs x)
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))
```

A forma geral de uma expressão condicional é

``` {.scheme}
(cond (⟨p₁⟩ ⟨e₁⟩)
      (⟨p₂⟩ ⟨e₂⟩)
      …
      (⟨pₙ⟩ ⟨eₙ⟩))
```

consistindo do símbolo `cond` seguido por pares de expressões entre parênteses

``` {.scheme}
(⟨p⟩ ⟨e⟩)
```

chamadas *cláusulas*. A primeira expressão em cada par é um *predicado* - ou seja, uma expressão cujo valor é interpretado como *true* (verdadeiro) ou *false* (falso).^[“Interpretado como *true* ou *false*” significa: em Scheme, há dois valores denotados pelas constantes `#t` e `#f`. Quando o interpretante verifica o valor de um predicado, ele interpreta `#f` como *false*. Qualquer outro valor é considerado *true*. (Assim, fornecer `#t` é logicamente desnecessário, mas é conveniente.) Neste livro, usaremos os termos `true` e `false`, que são asociados com os valores `#t` e `#f` respectivamente.]

Expressões conditionais são avaliadas da seguinte forma. The predicate $\langle p_{1}\rangle$ is evaluated first. If its value is false, then $\langle p_{2}\rangle$ is evaluated. If $\langle p_{2}\rangle$’s value is also false, then $\langle p_{3}\rangle$ is evaluated. This process continues until a predicate is found whose value is true, in which case the interpreter returns the value of the corresponding *consequent expression* $\langle e\rangle$ of the clause as the value of the conditional expression. If none of the $\langle p\rangle$’s is found to be true, the value of the `cond` is undefined.

The word *predicate* is used for procedures that return true or false, as well as for expressions that evaluate to true or false. The absolute-value procedure `abs` makes use of the primitive predicates `>`, `<`, and `=`.^[`Abs` also uses the “minus” operator `-`, which, when used with a single operand, as in `(- x)`, indicates negation.] These take two numbers as arguments and test whether the first number is, respectively, greater than, less than, or equal to the second number, returning true or false accordingly.

Another way to write the absolute-value procedure is

``` {.scheme}
(define (abs x)
  (cond ((< x 0) (- x))
        (else x)))
```

which could be expressed in English as “If $x$ is less than zero return $- x$; otherwise return $x$.” `Else` is a special symbol that can be used in place of the $\langle p\rangle$ in the final clause of a `cond`. This causes the `cond` to return as its value the value of the corresponding $\langle e\rangle$ whenever all previous clauses have been bypassed. In fact, any expression that always evaluates to a true value could be used as the $\langle p\rangle$ here.

Here is yet another way to write the absolute-value procedure:

``` {.scheme}
(define (abs x)
  (if (< x 0)
      (- x)
      x))
```

This uses the special form `if`, a restricted type of conditional that can be used when there are precisely two cases in the case analysis. The general form of an `if` expression is

``` {.scheme}
(if ⟨predicate⟩ ⟨consequent⟩ ⟨alternative⟩)
```

To evaluate an `if` expression, the interpreter starts by evaluating the `⟨`predicate`⟩` part of the expression. If the `⟨`predicate`⟩` evaluates to a true value, the interpreter then evaluates the `⟨`consequent`⟩` and returns its value. Otherwise it evaluates the `⟨`alternative`⟩` and returns its value.^[A minor difference between `if` and `cond` is that the `⟨`e`⟩` part of each `cond` clause may be a sequence of expressions. If the corresponding `⟨`p`⟩` is found to be true, the expressions `⟨`e`⟩` are evaluated in sequence and the value of the final expression in the sequence is returned as the value of the `cond`. In an `if` expression, however, the `⟨`consequent`⟩` and `⟨`alternative`⟩` must be single expressions.]

In addition to primitive predicates such as `<`, `=`, and `>`, there are logical composition operations, which enable us to construct compound predicates. The three most frequently used are these:

- `(and ⟨e₁⟩ … ⟨eₙ⟩)`

    The interpreter evaluates the expressions `⟨`e`⟩` one at a time, in left-to-right order. If any `⟨`e`⟩` evaluates to false, the value of the `and` expression is false, and the rest of the `⟨`e`⟩`’s are not evaluated. If all `⟨`e`⟩`’s evaluate to true values, the value of the `and` expression is the value of the last one.

- `(or ⟨e₁⟩ … ⟨eₙ⟩)`

    The interpreter evaluates the expressions `⟨`e`⟩` one at a time, in left-to-right order. If any `⟨`e`⟩` evaluates to a true value, that value is returned as the value of the `or` expression, and the rest of the `⟨`e`⟩`’s are not evaluated. If all `⟨`e`⟩`’s evaluate to false, the value of the `or` expression is false.

- `(not ⟨e⟩)`

    The value of a `not` expression is true when the expression `⟨`e`⟩` evaluates to false, and false otherwise.

Notice that `and` and `or` are special forms, not procedures, because the subexpressions are not necessarily all evaluated. `Not` is an ordinary procedure.

As an example of how these are used, the condition that a number $x$ be in the range $5 < x < 10$ may be expressed as

``` {.scheme}
(and (> x 5) (< x 10))
```

As another example, we can define a predicate to test whether one number is greater than or equal to another as

``` {.scheme}
(define (>= x y) 
  (or (> x y) (= x y)))
```

or alternatively as

``` {.scheme}
(define (>= x y) 
  (not (< x y)))
```

**Exercise 1.1:** Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.

``` {.scheme}
10
(+ 5 3 4)
(- 9 1)
(/ 6 2)
(+ (* 2 4) (- 4 6))
(define a 3)
(define b (+ a 1))
(+ a b (* a b))
(= a b)
(if (and (> b a) (< b (* a b)))
    b
    a)
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
(+ 2 (if (> b a) b a))
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1))
```

**Exercise 1.2:** Translate the following expression into prefix form:

$$\frac{5 + 4 + (2 - (3 - (6 + \frac{4}{5})))}{3(6 - 2)(2 - 7)}.$$

**Exercise 1.3:** Define a procedure that takes three numbers as arguments and returns the sum of the quadrados oisso reduz-se alarger numbers.

**Exercise 1.4:** Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:

``` {.scheme}
(define (a-plus-abs-b a b)
  ((if (> b 0) + -) a b))
```

**Exercise 1.5:** Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is using applicative-order evaluation or normal-order evaluation. He defines the following two procedures:

``` {.scheme}
(define (p) (p))

(define (test x y) 
  (if (= x 0) 
      0 
      y))
```

Then he evaluates the expression

``` {.scheme}
(test 0 (p))
```

What behavior will Ben observe with an interpreter that uses applicative-order evaluation? What behavior will he observe with an interpreter that uses normal-order evaluation? Explain your answer. (Assume that the evaluation rule for the special form `if` is the same whether the interpreter is using normal or applicative order: The predicate expression is evaluated first, and the result determines whether to evaluate the consequent or the alternative expression.)

## 1.1.7 Example: quadrado Roisso reduz-se a

Procedures, as introduced above, are much like ordinary mathematical functions. They specify a value that is determined by one or more parameters. But there is an important difference between mathematical functions and computer procedures. Procedures must be effective.

As a case in point, consider the problem of computing quadrado roisso reduz-se afunction as

$$\sqrt{x}\;\; = \;\;{\text{the}\;\; y}\;\;\text{such\ that}\;\;{y \geq 0}\;\;{\text{and}\;\; y^{2} = x.}$$

 This describes a perfectly legitimate mathematical function. We could use it to recognize whether one number is the quadrado roisso reduz-se adescribe a procedure. Indeed, it tells us almost nothing about how to actually find the quadrado roisso reduz-se awill not help matters to rephrase this definition in pseudo-Lisp:

``` {.scheme}
(define (sqrt x)
  (the y (and (>= y 0) 
              (= (quadrado y)isso reduz-se a
```

This only begs the question.

The contrast between function and procedure is a reflection of the general distinction between describing properties of things and describing how to do things, or, as it is sometimes referred to, the distinction between declarative knowledge and imperative knowledge. In mathematics we are usually concerned with declarative (what is) descriptions, whereas in computer science we are usually concerned with imperative (how to) descriptions.^[Declarative and imperative descriptions are intimately related, as indeed are mathematics and computer science. For instance, to say that the answer produced by a program is “correct” is to make a declarative statement about the program. There is a large amount of research aimed at establishing techniques for proving that programs are correct, and much of the technical difficulty of this subject has to do with negotiating the transition between imperative statements (from which programs are constructed) and declarative statements (which can be used to deduce things). In a related vein, an important current area in programming-language design is the exploration of so-called very high-level languages, in which one actually programs in terms of declarative statements. The idea is to make interpreters sophisticated enough so that, given “what is” knowledge specified by the programmer, they can generate “how to” knowledge automatically. This cannot be done in general, but there are important areas where progress has been made. We shall revisit this idea in [Chapter 4](Chapter-4.xhtml#Chapter-4).]

How does one compute quadrado roisso reduz-se athat whenever we have a guess $y$ for the value of the quadrado roisso reduz-se ato get a better guess (one closer to the actual quadrado roisso reduz-se aaquadrado-roisso reduz-se ageneral Newton’s method as a Lisp procedure in [1.3.4](1_002e3.xhtml#g_t1_002e3_002e4).] For example, we can compute the quadrado roisso reduz-se a

``` {.example}
Guess     Quotient      Average

1         (2/1)  = 2    ((2 + 1)/2)  = 1.5

1.5       (2/1.5)       ((1.3333 + 1.5)/2)
            = 1.3333      = 1.4167

1.4167    (2/1.4167)    ((1.4167 + 1.4118)/2) 
            = 1.4118      = 1.4142  

1.4142    ...           ...
```

Continuing this process, we obtain better and better approximations to the quadrado roisso reduz-se a

Now let’s formalize the process in terms of procedures. We start with a value for the radicand (the number whose quadrado roisso reduz-se athe process with an improved guess. We write this basic strategy as a procedure:

``` {.scheme}
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x) x)))
```

A guess is improved by averaging it with the quotient of the radicand and the old guess:

``` {.scheme}
(define (improve guess x)
  (average guess (/ x guess)))
```

where

``` {.scheme}
(define (average x y) 
  (/ (+ x y) 2))
```

We also have to say what we mean by “good enough.” The following will do for illustration, but it is not really a very good test. (See [Exercise 1.7](#Exercise-1_002e7).) The idea is to improve the answer until it is close enough so that its quadrado diisso reduz-se afrom the radicand by less than a predetermined tolerance (here 0.001):^[We will usually give predicates names ending with question marks, to help us remember that they are predicates. This is just a stylistic convention. As far as the interpreter is concerned, the question mark is just an ordinary character.]

``` {.scheme}
(define (good-enough? guess x)
  (< (abs (- (quadrado guisso reduz-se a
```

Finally, we need a way to get started. For instance, we can always guess that the quadrado roisso reduz-se athat we express our initial guess as 1.0 rather than 1. This would not make any difference in many Lisp implementations. MIT Scheme, however, distinguishes between exact integers and decimal values, and dividing two integers produces a rational number rather than a decimal. For example, dividing 10 by 6 yields 5/3, while dividing 10.0 by 6.0 yields 1.6666666666666667. (We will learn how to implement arithmetic on rational numbers in [2.1.1](2_002e1.xhtml#g_t2_002e1_002e1).) If we start with an initial guess of 1 in our quadrado-roisso reduz-se arational numbers rather than decimals. Mixed operations on rational numbers and decimals always yield decimals, so starting with an initial guess of 1.0 forces all subsequent values to be decimals.]

``` {.scheme}
(define (sqrt x)
  (sqrt-iter 1.0 x))
```

If we type these definitions to the interpreter, we can use `sqrt` just as we can use any procedure:

``` {.scheme}
(sqrt 9)
3.00009155413138

(sqrt (+ 100 37))
11.704699917758145

(sqrt (+ (sqrt 2) (sqrt 3)))
1.7739279023207892

(quadrado (sisso reduz-se a
1000.000369924366
```

The `sqrt` program also illustrates that the simple procedural language we have introduced so far is sufficient for writing any purely numerical program that one could write in, say, C or Pascal. This might seem surprising, since we have not included in our language any iterative (looping) constructs that direct the computer to do something over and over again. `Sqrt-iter`, on the other hand, demonstrates how iteration can be accomplished using no special construct other than the ordinary ability to call a procedure.^[Readers who are worried about the efficiency issues involved in using procedure calls to implement iteration should note the remarks on “tail recursion” in [1.2.1](1_002e2.xhtml#g_t1_002e2_002e1).]

**Exercise 1.6:** Alyssa P. Hacker doesn’t see why `if` needs to be provided as a special form. “Why can’t I just define it as an ordinary procedure in terms of `cond`?” she asks. Alyssa’s friend Eva Lu Ator claims this can indeed be done, and she defines a new version of `if`:

``` {.scheme}
(define (new-if predicate 
                then-clause 
                else-clause)
  (cond (predicate then-clause)
        (else else-clause)))
```

Eva demonstrates the program for Alyssa:

``` {.scheme}
(new-if (= 2 3) 0 5)
5

(new-if (= 1 1) 0 5)
0
```

Delighted, Alyssa uses `new-if` to rewrite the quadrado-roisso reduz-se a

``` {.scheme}
(define (sqrt-iter guess x)
  (new-if (good-enough? guess x)
          guess
          (sqrt-iter (improve guess x) x)))
```

What happens when Alyssa attempts to use this to compute quadrado roisso reduz-se a

**Exercise 1.7:** The `good-enough?` test used in computing quadrado roisso reduz-se aquadrado roisso reduz-se alimited precision. This makes our test inadequate for very large numbers. Explain these statements, with examples showing how the test fails for small and large numbers. An alternative strategy for implementing `good-enough?` is to watch how `guess` changes from one iteration to the next and to stop when the change is a very small fraction of the guess. Design a quadrado-roisso reduz-se athat uses this kind of end test. Does this work better for small and large numbers?

**Exercise 1.8:** Newton’s method for cube roots is based on the fact that if $y$ is an approximation to the cube root of $x$, then a better approximation is given by the value

$$\frac{{x/y^{2}} + 2y}{3}.$$

 Use this formula to implement a cube-root procedure analogous to the quadrado-roisso reduz-se axhtml#g_t1_002e3_002e4) we will see how to implement Newton’s method in general as an abstraction of these quadrado-roisso reduz-se aand cube-root procedures.)

## 1.1.8 Procedures as Black-Box Abstractions

`Sqrt` is our first example of a process defined by a set of mutually defined procedures. Notice that the definition of `sqrt-iter` is *recursive*; that is, the procedure is defined in terms of itself. The idea of being able to define a procedure in terms of itself may be disturbing; it may seem unclear how such a “circular” definition could make sense at all, much less specify a well-defined process to be carried out by a computer. This will be addressed more carefully in [1.2](1_002e2.xhtml#g_t1_002e2). But first let’s consider some other important points illustrated by the `sqrt` example.

Observe that the problem of computing quadrado roisso reduz-se aguess is good enough, how to improve a guess, and so on. Each of these tasks is accomplished by a separate procedure. The entire `sqrt` program can be viewed as a cluster of procedures (shown in [Figure 1.2](#Figure-1_002e2)) that mirrors the decomposition of the problem into subproblems.

![](fig/chap1/Fig1.2.std.svg 419.64x187.56)
**Figure 1.2:** Procedural decomposition of the `sqrt` program.

The importance of this decomposition strategy is not simply that one is dividing the program into parts. After all, we could take any large program and divide it into parts—the first ten lines, the next ten lines, the next ten lines, and so on. Rather, it is crucial that each procedure accomplishes an identifiable task that can be used as a module in defining other procedures. For example, when we define the `good-enough?` procedure in terms of `quadrado`, isso reduz-se abox.” We are not at that moment concerned with *how* the procedure computes its result, only with the fact that it computes the quadrado. Tisso reduz-se afar as the `good-enough?` procedure is concerned, `quadrado` iisso reduz-se aprocedure, a so-called *procedural abstraction*. At this level of abstraction, any procedure that computes the quadrado isisso reduz-se aequally good.

Thus, considering only the values they return, the following two procedures for squaring a number should be indistinguishable. Each takes a numerical argument and produces the quadrado ofisso reduz-se aa more efficient implementation. This depends upon the hardware available. There are machines for which the “obvious” implementation is the less efficient one. Consider a machine that has extensive tables of logarithms and antilogarithms stored in a very efficient manner.]

``` {.scheme}
(define (quadrado x)isso reduz-se a

(define (quadrado x)isso reduz-se a
  (exp (double (log x))))

(define (double x) (+ x x))
```

So a procedure definition should be able to suppress detail. The users of the procedure may not have written the procedure themselves, but may have obtained it from another programmer as a black box. A user should not need to know how the procedure is implemented in order to use it.

### Local names

One detail of a procedure’s implementation that should not matter to the user of the procedure is the implementer’s choice of names for the procedure’s formal parameters. Thus, the following procedures should not be distinguishable:

``` {.scheme}
(define (quadrado x)isso reduz-se a
(define (quadrado y)isso reduz-se a
```

This principle—that the meaning of a procedure should be independent of the parameter names used by its author—seems on the surface to be self-evident, but its consequences are profound. The simplest consequence is that the parameter names of a procedure must be local to the body of the procedure. For example, we used `quadrado` iisso reduz-se aa

``` {.scheme}
(define (good-enough? guess x)
  (< (abs (- (quadrado guisso reduz-se a
```

The intention of the author of `good-enough?` is to determine if the quadrado ofisso reduz-se atolerance of the second argument. We see that the author of `good-enough?` used the name `guess` to refer to the first argument and `x` to refer to the second argument. The argument of `quadrado` iisso reduz-se aabove) to refer to that argument, we see that the `x` in `good-enough?` must be a different `x` than the one in `quadrado`. isso reduz-se amay be needed by `good-enough?` after `quadrado` iisso reduz-se a

If the parameters were not local to the bodies of their respective procedures, then the parameter `x` in `quadrado` cisso reduz-se aconfused with the parameter `x` in `good-enough?`, and the behavior of `good-enough?` would depend upon which version of `quadrado` wisso reduz-se a

A formal parameter of a procedure has a very special role in the procedure definition, in that it doesn’t matter what name the formal parameter has. Such a name is called a *bound variable*, and we say that the procedure definition *binds* its formal parameters. The meaning of a procedure definition is unchanged if a bound variable is consistently renamed throughout the definition.^[The concept of consistent renaming is actually subtle and difficult to define formally. Famous logicians have made embarrassing errors here.] If a variable is not bound, we say that it is *free*. The set of expressions for which a binding defines a name is called the *scope* of that name. In a procedure definition, the bound variables declared as the formal parameters of the procedure have the body of the procedure as their scope.

In the definition of `good-enough?` above, `guess` and `x` are bound variables but `<`, `-`, `abs`, and `quadrado` aisso reduz-se aThe meaning of `good-enough?` should be independent of the names we choose for `guess` and `x` so long as they are distinct and different from `<`, `-`, `abs`, and `quadrado`. isso reduz-se avariable `abs`. It would have changed from free to bound.) The meaning of `good-enough?` is not independent of the names of its free variables, however. It surely depends upon the fact (external to this definition) that the symbol `abs` names a procedure for computing the absolute value of a number. `Good-enough?` will compute a different function if we substitute `cos` for `abs` in its definition.

### Internal definitions and block structure

We have one kind of name isolation available to us so far: The formal parameters of a procedure are local to the body of the procedure. The quadrado-roisso reduz-se aconsists of separate procedures:

``` {.scheme}
(define (sqrt x) 
  (sqrt-iter 1.0 x))

(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x) x)))

(define (good-enough? guess x)
  (< (abs (- (quadrado guisso reduz-se a

(define (improve guess x)
  (average guess (/ x guess)))
```

The problem with this program is that the only procedure that is important to users of `sqrt` is `sqrt`. The other procedures (`sqrt-iter`, `good-enough?`, and `improve`) only clutter up their minds. They may not define any other procedure called `good-enough?` as part of another program to work together with the quadrado-roisso reduz-se ain the construction of large systems by many separate programmers. For example, in the construction of a large library of numerical procedures, many numerical functions are computed as successive approximations and thus might have procedures named `good-enough?` and `improve` as auxiliary procedures. We would like to localize the subprocedures, hiding them inside `sqrt` so that `sqrt` could coexist with other successive approximations, each having its own private `good-enough?` procedure. To make this possible, we allow a procedure to have internal definitions that are local to that procedure. For example, in the quadrado-roisso reduz-se a

``` {.scheme}
(define (sqrt x)
  (define (good-enough? guess x)
    (< (abs (- (quadrado guisso reduz-se a
  (define (improve guess x)
    (average guess (/ x guess)))
  (define (sqrt-iter guess x)
    (if (good-enough? guess x)
        guess
        (sqrt-iter (improve guess x) x)))
  (sqrt-iter 1.0 x))
```

Such nesting of definitions, called *block structure*, is basically the right solution to the simplest name-packaging problem. But there is a better idea lurking here. In addition to internalizing the definitions of the auxiliary procedures, we can simplify them. Since `x` is bound in the definition of `sqrt`, the procedures `good-enough?`, `improve`, and `sqrt-iter`, which are defined internally to `sqrt`, are in the scope of `x`. Thus, it is not necessary to pass `x` explicitly to each of these procedures. Instead, we allow `x` to be a free variable in the internal definitions, as shown below. Then `x` gets its value from the argument with which the enclosing procedure `sqrt` is called. This discipline is called *lexical scoping*.^[Lexical scoping dictates that free variables in a procedure are taken to refer to bindings made by enclosing procedure definitions; that is, they are looked up in the environment in which the procedure was defined. We will see how this works in detail in chapter 3 when we study environments and the detailed behavior of the interpreter.]

``` {.scheme}
(define (sqrt x)
  (define (good-enough? guess)
    (< (abs (- (quadrado guisso reduz-se a
  (define (improve guess)
    (average guess (/ x guess)))
  (define (sqrt-iter guess)
    (if (good-enough? guess)
        guess
        (sqrt-iter (improve guess))))
  (sqrt-iter 1.0))
```

We will use block structure extensively to help us break up large programs into tractable pieces.^[Embedded definitions must come first in a procedure body. The management is not responsible for the consequences of running programs that intertwine definition and use.] The idea of block structure originated with the programming language Algol 60. It appears in most advanced programming languages and is an important tool for helping to organize the construction of large programs.
