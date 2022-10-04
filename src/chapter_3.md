# Teórica 03

## Lógica de Primeira Ordem Clássica

É uma linguagem mais rica que a lógica proposicional. O seu léxico é composto não só pelos símbolos $\wedge$, $\vee$, $\neg$ e $\rightarrow$, como pelos símbolos $\exists$ e $\forall$ para "existe" e "para todos", além de diversos símbolos para representar variáveis, constantes, funções e relações.

Tem 2 principais componentes:

- **Termos**: denotam objetos sobre os quais estamos a falar;
- **Fórmulas**: denotam valores verdadeiros.

O alfabeto da lógica de primeira ordem é organizado nas seguintes categorias:

- **Variáveis**: $x, y, z, ... \in \mathcal{X}$ (elementos arbitrários num dado domínio);
- **Constantes**: $a, b, c, ... \in \mathcal{C}$ (elementos específicos num dado domínio);
- **Funções**: $f, g, h, ... \in \mathcal{F}$ (todas as funções têm uma aridade fixa, $ar(f)$);
- **Predicados**: $P, Q, R, ... \in \mathcal{P}$ (todos os predicados têm uma aridade fixa, $ar(P)$);
- **Conetores Lógicos**: $\bot, \top, \wedge, \vee, \neg, \rightarrow, \forall, \exists$;
- **Símbolos Auxiliares**: ".", "(" e ")".

## Modelação com FOL (*First Order Logic*)

### "Nem todos os pássaros voam"

Vamos assumir 2 predicados unários:
```
B(x) - x é um pássaro;
F(x) - x pode voar.
```
Assim, podemos passar a frase para:

$$\neg(\forall_x. B(x)\rightarrow F(x))$$

Ou:

$$\exists_x.B(x) \wedge \neg F(x)$$

### "Todas as mães são mais velhas que os filhos" e "O John e o Peter têm a mesma avó materna"

Usando as constantes $j$ para John, $p$ para Peter e os seguintes predicados:

```
mother(x, y) - x é mãe de y
older(x, y) - x é mais velhor que y
```

Podemos escrever as expressões anteriores:

$$\forall_x. \forall_y. mother(x, y)\rightarrow older(x, y)$$
$$\forall_{x, y, u, v}. mother(x, y) \wedge mother(y, j) \wedge mother(u, v) \wedge mother(v, p)
\rightarrow x = u$$

De forma a simplificar a expressão, podemos utilizar uma função ao invés de uma relação para o caso da mãe. Assim, seja $m(y)$ mãe de $y$:

$$\forall_x.older(m(x), x)$$
$$m(m(j)) = m(m(p))$$

## Variáveis Livres e Vinculadas

- As **variáveis livres** de uma fórmula $\phi$ são as variáveis que ocorram em $\phi$ e que não contêm quantificadores. $FV(\phi)$ denota o conjunto de variáveis livres que ocorrem em $\phi$;
- As **variáveis vinculadas** de uma fórmula $\phi$ são as variáveis que ocorrem $\phi$ e que contêm quantificadores. $BV(\phi)$ denota o conjunto de variáveis vinculadas presentes em $\phi$;
- Uma dada variável pode conter ocorrências livres e vinculadas na mesma fórmula;
- Uma fórmula diz-se fechada se não tiver qualquer variável livre;
- Se $FV(\phi) = \{x_1, ..., x_n\}$ então:
  - o seu *universal closure* é $\forall_{x_{1}}, \cdots, \forall_{x_{n}}.\phi$
  - o seu *existential closure* é $\exists_{x{_1}},\cdots, \exists_{x_{n}}.\phi$

## Substituição

- Definimos $u[t/x]$ como o termo obtido por substituir todas as ocorrências da variável $x$ em $u$ por $t$;
- Definimos $\phi[t/x]$ como a fórmula obtida por substituir todas as ocorrências **livres** da variável $x$ em $\phi$ pot $t$.

Deve-se ter bastante cuidado a mexer com substituições, pois, facilmente, podem obter-se resultados inesperados.

## Semântica

### *V-Structure*

Uma $\mathcal{V}$-*Structure* $\mathcal{M}$ é um par $\mathcal{M} = (D, I)$ em que $D$ é um conjunto não vazio chamado **domínio de interpretação** e $I$ é uma **função de interpretação** que designa constantes, funções e predicados em D para símbolos de $\mathcal{V}$:
- Para cada símbolo constante $c \in \mathcal{C}$, a interpretação de $c$ é a constante $I(c) \in D$;
- Para cada $f \in \mathcal{F}$, a interpretação de $f$ é a função $I(f): D^{ar(f)}\rightarrow D$;
- Para cada $P \in \mathcal{P}$, a interpretação de $\mathcal{P}$ é a função $I(P): D^{ar(P)} \rightarrow \{0,1\}$. Em particular, símbolos de predicados $0-ários$
são interpretados como valores verdadeiros.

A isto é ainda dado o nome de *models* de $\mathcal{V}$.

### *Assignment*

Um *assignment* para um domínio $D$ é uma função $\alpha:\mathcal{X}\rightarrow D$.

Denota-se $\alpha[x\mapsto a]$ o *assignment* que mapeia $x$ para $a$ e qualquer outra variável $y$ para $\alpha(y)$.

### Validade e Satisfabilidade

Uma fórmula $\phi$ diz-se:

- **válida** sse $\mathcal{M}, \alpha \models \phi$ se verifica para toda a estrutura $\mathcal{M}$ e dá *assign* de $\alpha$. Uma fórmula válida é chamada de **tautologia**. Escrevemos $\models \phi$;
- **satisfazível** sse existe alguma estrutura $\mathcal{M}$ e algum *assignment* de $\alpha$ tais que dado $\mathcal{M}$, verifica-se $\alpha \models \phi$;
- **insatisfazível** sse não é satisfazível. A isto chama-se **contradição**;
- **refutável** sse não é válida.

### Consistência

Um conjunto $\Gamma$ é consistente ou satisfazível sse existe um modelo $\mathcal{M}$ e um *assignment* $\alpha$ tais que $\mathcal{M}, \alpha \models \phi$ verifica-se para todo o $\phi \in \Gamma$.

### Substituição

- Uma fórmula $\psi$ diz-se subfórmula de $\phi$ se ocorrer sintaticamente em $\phi$;
- Uma fórmula $\psi$ é uma subfórmula estrita de $\phi$ se $\psi$ é uma subfórmula de $\phi$ e $\psi \neq \phi$

## Decidibilidade

**Problemas de Decisão**:

- **Problema da Validade**: "$\phi$ é válido?";
- **Problema da Satisfabilidade**: "$\phi$ é satisfazível?";
- **Problema da Consequência**: "$\psi$ é uma consequência de $\phi$?";
- **Problema da Equivalência**: "$\phi$ e $\psi$ são equivalentes?"

Estes são, de certa forma, variações do mesmo problema:

- $\phi$ é válido **sse** $\neg \phi$ é não satisfazível;
- $\phi \models \psi$ **sse** $\neg(\phi\rightarrow \psi)$ é insatisfazível;
- $\phi \equiv \psi$ **sse** $\phi \models \psi$ e $\psi \models \phi$;
- $\phi$ é satisfazível **sse** $\neg \phi$ é não válido.

Uma solução para um problema de decidibilidade é um programa que recebe instâncias do problema como *input* e termina **sempre** produzindo um *output* "sim" ou "não" correto.

### Semi-decibilidade

Um problema de decisão é semi-decidível se existe um procedimento que, dado um input:

- Pára e responde "sim" **sse** "sim" é a resposta correta;
- Pára e responde "não" se "não" for a resposta correta;
- Não pára e "não" é a resposta correta".

Ao contrário de um problema de decisão, este procedimente apenas pára, de forma garantida, no caso da resposta correta ser "sim".

## Formas Normais

Uma fórmula de primeira ordem está numa **NNF** (*negation normal form*) se o conetivo da implicação não é utilizado e a negação apenas é aplicada a fórmulas atómicas.

Uma fórmula está na *prenex form* se estiver na forma $\mathcal{Q}_1x_1.\mathcal{Q}_2x_2.\cdots.\mathcal{Q}_nx_n.\psi$ em que cada $Q_i$ é um quantificador ($\forall$ ou $\exists$) e $\psi$ é uma fórmula livre de quantificadores.

## *Herbrand*/*Skolem* *Normal Forms*

Seja $\phi$ uma fórmula de primeira oredem na forma *prenex normal*:

- A *Herbrandization* de $\phi$ ($\phi^H$) é a fórmula existencial de $\phi$ que se obtém após aplicar de forma repetida e exaustiva a seguinte transformação:

$$\exists_{x_1},\cdots,x_n.\forall_y.\psi \rightsquigarrow \exists_{x_1},\cdots,x_n.\psi[f(x_1,\cdots,x_n)/y]$$

- A *Skolemization* de $\phi$ ($\phi^S$) é a fórmula universal obtida a partir da aplicação repetida da transformação:

$$\forall{x_1},\cdots,x_n.\exists_y.\psi \rightsquigarrow \forall_{x_1},\cdots,x_n.\psi[f(x_1,\cdots,x_n)/y]$$

Uma fórmula $\phi$ e a sua *Herbrandization/Skolemization* não são logicamente equivalentes.

- $\phi$ é válida **sse** a sua *Herbrandization* $\phi^H$ é válida;
- $\phi$ é satisfazível **sse** a sua *Skolemization* $\phi^S$ é satisfazível.

## FOL com igualdade

Esta abordagem considera a igualdade como um símbolo lógico de interpretação fixa.

O símbolo de igualdade é interpretado como a relação de igualdade no domínio da interpretação. Assim, tem-se uma estrutura $\mathcal{M}=(D, I)$ e um *assignment* $alpha: \mathcal{X}\rightarrow D$, tal que:

$$\mathcal{M}, \alpha \models t_1 = t_2\ iff\ \alpha_{\mathcal{M}}(t_1)\ and\ \alpha_{\mathcal{M}}(t_2)\ são\ o\ mesmo\ elemento\ de\ D$$

## *Many-sorted* FOL

- Permite a coexistência de diferentes domínios de elementos na mesma *framework*.
- É composta por conjunto de *sorts*, um conjunto de símbolos de funções e um conjunto de símbolos de predicados.
  - Cada símbolo de função $f$ tem associado um tipo da forma $S_1 \times \cdots \times S_{ar(f)} \rightarrow S$, em que S são *sorts*;
  - Cada símbolo de predicado $P$ tem associado um tipo da forma $S_1 \times \cdots \times S_{ar(P)}$;
  - Cada variável tem a si associado um *sort*.

- A formação dos termos e das fórmulas é feito respeitando os *sorts*;
- O domínio do discurso de cada estrutura de um vocabulário *many-sorted* é fragmentado em diferentes *subsets*, um para cada *sort*;
- A noção de *assignment* e de estrutura para um vocabulário *many-sorted* e a sua interpretação de termos e fórmulas estão definidas da fórmula expectável.

## *SMT Solvers*

O problema SMT (*Satisfability Modulo Theories*) é a variação do SAT para a lógica de primeira ordem com a interpretação dos símbolos restrita a um conjunto de teorias específicas.

## Modelos de Domínio

Um modelo de domínio é um modelo concetual de um sistema que descreve as diversas entidades involvidas no sistema e nas suas relações.