# Teórica 02

## Lógica Proposicional e SAT Solvers

Lógica formal consiste em:

- Linguagem lógica, na qual são expressas frases bem formadas. Consite em:
  - Símbolos lógicos de interpretação fixa;
  - Símbolos não-lógicos em que as interpretações podem variar.
- Semântica que define a interpretação dos símbolos e expressões da linguagem;
- Sistema de Prova que é uma *framework* de regras para derivações válidas.

## O que é um SAT?

**SAT (*Boolean Satisfiability Problem*)**:

Encontra um conjunto de variáveis proposicionais para os quais à fórmula lógica é avaliada como uma fórmula verdadeira, ou prova que não existe um conjunto de variáveis capazes de tornar a fórmula verdadeira.

É um problema de decisão NP-completo.

- Geralmente, os *SAT solvers* lidam com fórmulas CNF (*conjunctive normal form*):
  - ***literal***: variável proposicional ou a sua negação: $A, \neg{A}, B, \neg{B}, ...$;
  - ***clause***: disjunção de literais: $(A \vee \neg{B} \vee C)$;
  - ***conjunctive normal form***: conjunção de cláusulas: $(A \vee \neg{B} \vee C) \wedge (B \vee \neg{A}) \wedge \neg{C}$.

## Lógica Proposicional Clássica

### Válida *vs* Satisfazível *vs* Contradição

Uma dada fórmula $F$ é:

- **válida** sse é verdadeira para todos os conjuntos de variáveis. Escrevemos $\models F$. É chamada **tautologia**;
- **satisfazível** sse é verdadeira para, pelo menos, um conjunto de variáveis;
- **insatisfazível** sse é falsa para qualquer conjunto de variáveis. É chamada **contradição**;
- **refutável** sse não é válida.

**Proposição**:

$$F\  is\ valid\ \ \  iff\ \ \  \neg{F}\ is\ unsatisfiable$$

### Consequência e Equivalência

- $F \models G$ sse para cada proposição $A$, `if` $A \models F$ `then` $A \models G$. Diz-se que $G$ é **consequência** de $F$;
- $F \equiv G$ sse $F \models G$ e $G \models F$. Dizemos que $F$ e $G$ são equivalentes;
- Seja $\Gamma={F_1, F_2, F_3, ...}$ um dado conjunto de fórmulas:
  - $A \models T\ iff\ A \models F_i$ para cada fórmula $F_i$ em $\Gamma$. Dizemos que $A$ *models* $\Gamma$;
  - $\Gamma\models G\ iff\ A\models G \rightarrow A$ para cada $A$. Dizemos que $G$ é consequência de $\Gamma$.

### Consistência

Sendo $\Gamma = {F_1, F_2, F_3,...}$ um conjunto de fórmulas:
- $\Gamma$ é consistente ou satisfazível sse existir um conjunto que modela $\Gamma$;
- Dizemos que $\Gamma$ é inconsistente ou insatisfazível sse $\Gamma$ não for consistente. Denotamos isto com $\Gamma \models \perp$.

### Substituição

- A fórmula $G$ é uma subfórmula da fórmula $F$ se ocorrer sintaticamente de $F$;
- A fórmula $G$ é uma fórmula estrita de $F$ se $G$ for uma subfórmula de $F$ e $G \neq F$.

### Decidibilidade

**Problemas de Decibilidade**:
- **Problema da Validade**: "$F$ é válido?";
- **Problema da Satisfabilidade**: "$F$ é satisfazível?";
- **Problema da Consequência**: "Será $G$ uma consequência de $F$?";
- **Problema da Equivalência**: "Serão $F$ e $G$ equivalentes?".

Um algoritmo capaz de resolver um destes problemas é capaz de resolver todos os outros.

Um exemplo disto é a construção de uma tabela de verdade, no entanto, este algoritmo é extremamente ineficiente. Tem uma complexidade temporal exponencial: $O(2^n)$.

Ou seja, se $F$ tiver $n$ fórmulas atómicas, a tabela de verdade terá $2^n$ linhas.

### Complexidade

- Existem diversas técnicas e algoritmos para a resolução de SATs que apresentam uma melhor performance, em média.
- Qualquer algoritmo que resolva um SAT é, no pior dos casos, exponencial no número de variáveis

## Algoritmos de SAT *solving*

- Geralmente, recebem as fórmulas num formato sintático específico.
  - Assim, o primeiro passo, normalmente, é passar a fórmula original para este formato preservando a sua satisfazibilidade.

### Fórmulas Normais

Geralmente, os *SAT Solvers* recebem os *inputs* como CNFs (*conjunctive normal forms*).

De notar, que uma fórmula é um NNF (*negation normal form*) se os únicos conetores utilizados forem $\neg$, $\vee$ e $\wedge$, sendo que a negação só deve aparecer nos literais.

Para transformar uma fórmula em NFF ou CNF devem-se ir aplicando as derivações lógicas, tendo cuidado para não alterar a satisfazibilidade da fórmula original. Este algoritmo, embora seja simples, no pior caso irá aumentar de forma exponencial o tamanho da fórmula.

#### *Definitional CNF*

Duas fórmulas $F$ e $F'$ são equisatisfazíveis quando $F$ é satisfazível sse $F'$ é satisfazível.

Qualquer fórmula podem ser transformada numa fórmula equisatisfazível CNF com um aumento linear do tamanho da fórmula. Para isso, devem-se acrescentar *n* novas variáveis booleanas, em que *n* corresponde ao número de conetores lógicos na fórmula. Esta transformação é feita através do *Tseiten's encoding*.

A esta transformação é chamado de *definitional CNF*, visto que é feita, únicamente, com base na introdução de novos símbolos proposicionais que funcionam como subfórmulas da fórmula original.

### *Tseiten's Encoding*

1. Introduz uma nova variável para cada subfórmula composta;
2. Atribui uma variável a cada subfórmula;
3. Coloca as *local constraints* em CNF;
4. Faz uma conjunção entre as *local constraints* e a variável "raíz".

- Existem ainda diversos métodos de otimização deste processo, de forma a reduzir o tamanho da fórmula resultante e o número de variáveis adicionais.

### Algoritmos de resolução de SATs

A grande maioria pode ser classificada em 2 grandes grupos:

- Baseados numa ***stochastic local search***:
  - o *solver* escolhe um grupo de valores para testar, se o resultado for falso, começa a alterar os valores com base numa heurística.
- Baseados numa ***DPLL framework***:
  - otimização ao DPLL que correspondem a dar *backtrack search* através do espaço entre possíveis grupos de valores.

Em geral, os baseados em DPLL são melhores em grande parte dos casos.

### Estado de uma cláusula numa proposição

- **Satisfeita**: um ou mais dos seus literais foram satisfeitos;
- **Em Conflito**: se todos os seus literais foram atribuídos, mas não satisfeitos;
- ***Unit***: Não é satisfeito e todos os seus literais, exceto 1, foram atribuídos;
- **Não Resolvido**: Nos outros casos.

### *Unit Propogation (Boolean Constraint Propagation)*

- ***Unit Clause Rule***: Dada uma *unit clause*, o seu único literal não atribuído deve ter atribuído o valor 1 para a sua cláusula ser satisfeita.
- À aplicação iterativa desta regra dá-se o nome de *Unit Propagation*. 

