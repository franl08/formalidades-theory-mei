# Teórica 05

## *Design* Estrutural com Alloy

### Estruturas de *Software*

- Estruturas de Dados;
- Esquemas de Bases de Dados;
- Arquiteturas;
- Topologias de Rede;
- Ontologias;
- Modelos de Domínio.
  
### *Design* Estrutural

Queremos perceber as entidades que temos e quais os seus relacionamentos, os requisitos do sistemas e, por fim, explorar diversas alternativas de *design*.

- Se tivermos uma maneira rápida de testar diversas abordagens pode ser muito útil!
  - Ajuda a encontrar a solução ideal.

`Alloy` é uma ferramenta de conceção de *software*.

### Modelação de domínio com UML

A modelação com UML apresenta algumas vulnerabilidades, entre as quais:

- Como validar o modelo?
- Existem algumas restrições redundantes ou esquecidas?
- O que é que as restrições querem mesmo dizer?
- As restrições apresentam todas as propriedades expectáveis do sistema?

Ou seja, as ferramentas UML não são capazes de nos dizer se o que temos é suficiente ou impossível...

Logo, era bom termos algo que pudesse combater este ponto e a utilização de `Alloy` deve ser capaz de ajudar nisso.

Atualmente, a abstração revela-se como a *skill* mais importante para modelar.

### *Design* de *Software* com *Alloy*

- É uma linguagem de modelação formal;
- Pode ser utilizada para declarar estruturas ou especificar restrições;
- Os modelos podem ser analisados automaticamente;
- Desenhada para abstração.

### Modelação de Domínio com *Alloy*

#### Assinaturas

- São conjuntos;
- Inabitadas por átomos de um universo finito do discurso;
- Assinaturas *top-level* são disjuntas;
- Uma assinatura de *extension* é um subconjunto do assinatura "parente";

##### Entidades = Assinaturas

```
sig Object {}
sig Entry {}
sig Name {}
```

##### "*Is a*" = Extensão

```
sig Dir extends Object {}
sig File extends Object {}
sig Root extends Dir {}
```

#### Campos

- São relacionamentos;
- Inabitada por conjuntos de tuplos de átomos do universo;
- São subconjuntos do produto cartesiano da fonte ou das assinaturas do tipo alvo;
- Todos os tuplos num campos têm a mesma aridade.

##### Relações = Campos

```
sig Dir extends Object {
  entries : set Entry
}

sig Entry {
  object : set Object,
  name : set Name
}
```

#### Restrições de Multiplicidade

$$fact \{ R\ in\ A\ m \rightarrow m\ B}$$

- A multiplicidade por *default* é *set*;
- A multiplicidade alvo pode, alternativamente, especificada na declaração;
- Nas declarações de um campo, a multiplicidade por *default* é *one*.

| Alloy | UML   |
|-------|-------|
| set   | 0..\* |
| lone  | 0..1  |
| some  | 1..\* |
| one   | 1     |

```
sig Dir extends Object {
  entries : set Entry
}

sig Entry {
  object : set Object,
  name : set Name
}

fact Multiplicities {
  entries in Dir one -> set Entry
  object in Entry set -> one Object
  name in Entry set -> one Name
}
```

##### Factos

- Especificam assunções;

```
fact {x}

```
- Podem ser nomeados;

```
fact Name {x}
```

- Podem ter múltiplas restrições, uma por linha.

```
fact{
  x
  y
}
```

#### *Bestiary*

```
R in A set -> some B      // R é inteiro
R in A set -> lone B      // R é simples
R in A some -> set B      // R é sobrejetiva
R in A lone -> set B      // R é injetiva

R in A lone -> some B     // R é uma representação
R in A some -> lone B     // R é abstração

R in A set -> one B       // R é uma função
R in A lone -> one B      // R é uma injeção
R in A some -> one B      // R é uma sobrejeção

R in A one -> one B       // R é uma bijeção
```

### Análise

#### Comandos

- O *Alloy* tem 2 tipos de comandos de análise:
  - `run { x }` pede um exemplo que satisfaça `x`;
  - `check { x }` pede um contra-exemplo que refute a asserção `x`.
- Podem ser nomeados e ter diversas restrições, uma por linha;
- No *visualizer* é possível pedir mais exemplos ou contraexemplos ao premir o botão *New*.

#### Instâncias

- Tanto os exemplos como os contraexemplos são instâncias do modelo;
- Uma instância é uma validação para todos as assinaturas e campos;
- Uma instância deverá satisfazer todas as declarações e todos os factos;
- Numa instância "tudo é uma relação":
  - Assinaturas são relações unárias (conjuntos de tuplos unários);
  - Contantes são relações unárias *singleton* (conjuntos com um tuplo unário).

#### *Scopes*

- De forma a garantir decidibilidade, os comandos têm um *scope*;
- O *scope* impõe um limite no tamanho de um universo finito que o *Analyzer* irá explorar de forma exaustiva;
- O *scope* por *default* impõe o limite de 3 átomos por assinatura *top-level*;
- A *keyword* `for` pode ser utilizado para especificar diferentes *scopes* para assinatures *top-level*;
- A *keyword* `but` pode ser utilizado para epecificar diferentes *scopes* para assinaturas específicas.

##### *The small scope hypothesis*

- Se o `run {x}` retornar uma instância, então dizemos que `x` é consistente, caso contrário, dizemos que `x` **pode** ser inconsistente;
  - Poderá ser consistente num maior *scope*...
- Se o `check {x}` retornar uma instância, então dizemos que `x` é inválido, caso contrário, dizemos que `x` **pode** ser inválido.
  - Poderá ser inválido num maior *scope*...

No entanto, existe uma teoria (*The small scope hypothesis*) que defende que grande parte dos *bugs* podem ser detetados em pequenos exemplos, pelo que isto não deve ser um problema muito grave.

### Átomos

- O universo de discurso contém átomos;
- Átomos não são interpretados (não têm semântica);
- São nomeados de forma automática de acordo com a sua respetiva assinatura;
- Duas instâncias dizem-se isomórficas (ou simétricas) se forem iguais e apenas os nomes dos átomos mudarem.
  - A análise implementa um processo que evita isto.

### Assinaturas Abstratas

- Todos os átomos em assinaturas abstratas pertencem a uma das suas extensões.

```
abstract sig Object {}
sig Dir extends Object {
  entries : set Entry
}
sig File extends Object {}
```

### Multiplicidades de Assinaturas

- Também se podem utilizar multiplicidades em declarações de assinaturas;
- Em particular, `one sig` denota uma constante.

```
one sig Root extends Dir {}
```

### Asserções

- São restrições nomeadas para serem verificadas.

```
assert NoPartitions {
  // Qualquer coisa
}

check NoPartitions
```

## Lógica Relacional

Para utilizarmos o `Alloy` temos de utilizar lógica relacional, pois com FOL as expressões seriam demasiado complexas e extensas.

A Lógica Relacional estende o FOL com:

- Fórmulas atómicas derivadas, nomeadamente verificações de cardinalidade;
- Operações derivadas para combinar predicados (relações) em predicados mais complexos;
- *Closures* transitivas e reflexivas que não podem ser expressas em FOL.