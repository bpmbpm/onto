## old
### 1

Да, `owl:Class`, `owl:Thing` и предикаты находятся **не на одном уровне**. Для точности нужно разделить:

1. RDF-уровень: ресурсы, RDF-термы и тройки;
2. OWL-синтаксический уровень: сущности, выражения и аксиомы;
3. OWL-семантический уровень: области интерпретации и функции интерпретации;
4. метамодельный уровень спецификации: UML-классы, описывающие структуру самого OWL.

Ваше `xxx` лучше называть **семантической областью**, **доменом интерпретации**, **множеством интерпретации** или **semantic domain**. В OWL Direct Semantics используются два ключевых домена:

- `ΔI` — object domain, область объектов;
- `ΔD` — data domain, область значений данных.

## 1. Сначала важное исправление

Предположение:

> корневые множества — `owl:Class`, `owl:Thing`, предикаты и множество ресурсов

смешивает два разных типа описания.

`owl:Class` и `owl:Thing` — это **IRI, обозначающие специальные классы OWL**, а не математические корневые множества Direct Semantics.

Предикаты — это RDF-термы и в семантике OWL 2 обычно интерпретируются как **бинарные отношения**, а не как самостоятельная «область предикатов».

Ресурсы RDF — это более общий термин RDF-модели. В Direct Semantics OWL 2 используются не «все RDF-ресурсы», а отдельные элементы словаря:

```text
classes
object properties
data properties
individuals
datatypes
literals
facets
```

Спецификация Direct Semantics явно вводит vocabulary как кортеж этих множеств. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

## 2. Самый верхний уровень: RDF

Наиболее общий уровень — RDF.

### RDF-термы

В RDF 1.1 есть три вида RDF-термов:

```text
RDF terms = IRIs ∪ blank nodes ∪ literals
```

То есть:

| RDF-терм | Пример | Роль |
|---|---|---|
| IRI | `:Book`, `rdf:type` | именованный ресурс |
| blank node | `_:x` | анонимный ресурс |
| literal | `"Book"@en`, `42` | значение данных |

RDF-тройка имеет форму:

```text
(subject, predicate, object)
```

Например:

```turtle
:book123 rdf:type :Book .
```

В RDF:

- `:book123` — subject;
- `rdf:type` — predicate;
- `:Book` — object.

Предикат также является IRI и может встречаться как узел RDF-графа. Поэтому RDF не образует строгой дизъюнкции «предикаты отдельно, ресурсы отдельно»: IRI `rdf:type` является и предикатом, и RDF-термом. [w3](https://www.w3.org/TR/rdf11-concepts/)

### Ресурс

В RDF спецификации **resource** — это то, что обозначается IRI или литералом; в более широком смысле ресурсами называют сущности дискурса:

```text
Resource ≈ то, что может быть обозначено и описано
```

Но важно не отождествлять автоматически:

```text
RDF resource ≠ OWL entity
```

`OWL entity` — более узкий термин OWL 2.

## 3. Верхний уровень OWL 2: три синтаксические категории

OWL 2 структурно разделяет онтологию на три основные категории:

```text
OWL ontology
├── Entities
├── Expressions
└── Axioms
```

Это нормативная структура OWL 2. [w3](https://www.w3.org/TR/owl2-syntax/)

### 3.1 Сущности (`entities`)

Сущности — именованные примитивные элементы, идентифицируемые IRI:

```text
Entity
├── Class
├── ObjectProperty
├── DataProperty
├── AnnotationProperty
├── Individual
└── Datatype
```

Пример:

```turtle
:Book      # Class
:hasAuthor # ObjectProperty
:pageCount # DataProperty
:book123   # NamedIndividual
:xsd:integer # Datatype
```

В OWL 2 сущность — это не просто любой RDF-ресурс. Это ресурс, которому в структурной модели OWL присвоена определённая роль.

### 3.2 Выражения (`expressions`)

Выражения строят сложные понятия из сущностей:

```text
Expression
├── ClassExpression
├── ObjectPropertyExpression
├── DataPropertyExpression
├── DataRange
└── DatatypeRestriction
```

Пример класса-выражения:

```owl
ObjectIntersectionOf( :Book :PrintedPublication )
```

Он обозначает пересечение двух классов:

\[
(:Book \sqcap :PrintedPublication)^C
=
:Book^C \cap :PrintedPublication^C
\]

Класс-выражение может быть анонимным и не иметь собственного IRI.

### 3.3 Аксиомы (`axioms`)

Аксиомы — утверждения онтологии:

```text
Axiom
├── Declaration
├── ClassAxiom
├── ObjectPropertyAxiom
├── DataPropertyAxiom
├── DatatypeDefinition
├── Key
└── Assertion
```

Примеры:

```owl
Declaration( Class( :Book ) )

SubClassOf( :Book :Publication )

ClassAssertion( :Book :book123 )

ObjectPropertyAssertion( :hasAuthor :book123 :Tolstoy )
```

Важное различие:

```text
:Book             — entity
ObjectIntersectionOf(...) — expression
SubClassOf(...)   — axiom
```

## 4. OWL Direct Semantics: корневые множества

Теперь перейдём к математической семантике.

OWL 2 Direct Semantics задаёт интерпретацию как кортеж:

\[
I =
\langle
\Delta_I,\Delta_D,
\cdot^C,\cdot^{OP},\cdot^{DP},
\cdot^I,\cdot^{DT},
\cdot^{LT},\cdot^{FA},
NAMED
\rangle
\]

Это не онтология и не RDF-граф, а математическая модель значения онтологии. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

### 4.1 Область объектов

\[
\Delta_I
\]

называется **object domain** — область объектов.

Она должна быть непустой:

\[
\Delta_I \neq \varnothing
\]

Это множество, элементы которого интерпретируются как объекты предметной области:

```text
people
books
documents
organizations
numbers-as-objects
abstract entities
```

Один элемент может быть интерпретацией конкретного индивида:

\[
(:book123)^I \in \Delta_I
\]

### 4.2 Область значений данных

\[
\Delta_D
\]

называется **data domain** — область данных.

Она также непуста и не пересекается с областью объектов:

\[
\Delta_D \neq \varnothing
\]

\[
\Delta_I \cap \Delta_D = \varnothing
\]

Например:

```text
строки
целые числа
даты
булевы значения
```

В Direct Semantics это принципиальное разделение:

```text
object domain ≠ data domain
```

Именно поэтому OWL разделяет:

```text
ObjectProperty
DataProperty
```

## 5. Как интерпретируются классы и свойства

### 5.1 Классы

Функция интерпретации классов:

\[
\cdot^C
\]

каждому классу `C` сопоставляет подмножество области объектов:

\[
C^C \subseteq \Delta_I
\]

Например:

\[
:Book^C \subseteq \Delta_I
\]

То есть класс `:Book` — не отдельный объект в этой формуле, а множество объектов, являющихся книгами.

Для универсального класса:

\[
owl:Thing^C = \Delta_I
\]

Для пустого класса:

\[
owl:Nothing^C = \varnothing
\]

Следовательно:

```text
owl:Thing — не всё множество RDF-ресурсов;
owl:Thing — всё множество объектов object domain.
```

### 5.2 Object properties

Функция интерпретации объектных свойств:

\[
\cdot^{OP}
\]

каждому object property сопоставляет бинарное отношение:

\[
OP^{OP} \subseteq \Delta_I \times \Delta_I
\]

Например:

\[
:hasAuthor^{OP}
\subseteq
\Delta_I \times \Delta_I
\]

Утверждение:

```turtle
:book123 :hasAuthor :Tolstoy .
```

означает:

\[
\bigl(
(:book123)^I,
(:Tolstoy)^I
\bigr)
\in
(:hasAuthor)^{OP}
\]

То есть `:hasAuthor` — не «множество предикатов», а конкретное бинарное отношение на объектах.

### 5.3 Data properties

Функция интерпретации data properties:

\[
\cdot^{DP}
\]

сопоставляет свойству отношение:

\[
DP^{DP} \subseteq \Delta_I \times \Delta_D
\]

Например:

```turtle
:book123 :pageCount 1225 .
```

означает:

\[
\bigl(
(:book123)^I,
1225^{LT}
\bigr)
\in
(:pageCount)^{DP}
\]

Здесь первый аргумент — объект из `ΔI`, второй — значение из `ΔD`.

## 6. Индивиды и литералы

### Индивиды

Функция:

\[
\cdot^I
\]

каждому OWL individual сопоставляет один объект области:

\[
a^I \in \Delta_I
\]

Например:

\[
(:book123)^I \in \Delta_I
\]

### Литералы

Функция:

\[
\cdot^{LT}
\]

сопоставляет литералу значение данных:

\[
("1225"^^xsd:integer)^{LT}
\in \Delta_D
\]

Таким образом:

```text
:book123 — OWL individual
:Book — OWL class
"1225"^^xsd:integer — literal/data value
```

## 7. Прямая семантика одного примера

Возьмём:

```turtle
:Book rdf:type owl:Class .

:book123 rdf:type :Book .

:book123 :hasAuthor :Tolstoy .

:book123 :pageCount 1225 .
```

В Direct Semantics это означает:

### Объявление класса

```turtle
:Book rdf:type owl:Class .
```

В структурном OWL:

```owl
Declaration( Class( :Book ) )
```

На семантическом уровне `:Book` получает интерпретацию как множество:

\[
:Book^C \subseteq \Delta_I
\]

### Членство индивида в классе

```turtle
:book123 rdf:type :Book .
```

В функциональном синтаксисе:

```owl
ClassAssertion( :Book :book123 )
```

Семантическое условие:

\[
(:book123)^I \in :Book^C
\]

### Объектное свойство

```turtle
:book123 :hasAuthor :Tolstoy .
```

Условие:

\[
\bigl(
(:book123)^I,
(:Tolstoy)^I
\bigr)
\in :hasAuthor^{OP}
\]

### Свойство данных

```turtle
:book123 :pageCount 1225 .
```

Условие:

\[
\bigl(
(:book123)^I,
1225^{LT}
\bigr)
\in :pageCount^{DP}
\]

## 8. Что такое `owl:Class` в этой системе

Здесь необходимо особенно аккуратно разделить уровни.

В RDF-графе:

```turtle
:Book rdf:type owl:Class .
```

можно прочитать как:

```text
ресурс :Book имеет RDF-тип owl:Class
```

В OWL RDF Mapping это является представлением декларации:

```owl
Declaration( Class( :Book ) )
```

Но в Direct Semantics OWL 2 класс `:Book` интерпретируется не как элемент отдельного множества «всех классов», а как подмножество:

\[
:Book^C \subseteq \Delta_I
\]

То есть `owl:Class` в RDF-ориентированном представлении и `Class` в структурном синтаксисе — не одно и то же, что `:Book^C` в математической модели.

Условная цепочка:

```text
RDF:
:Book rdf:type owl:Class

OWL structural:
Declaration( Class( :Book ) )

Direct Semantics:
:Book^C ⊆ ΔI
```

Это три уровня представления одного намерения, а не три последовательных класса, которые обязаны образовывать цепочку экземпляров.

## 9. `owl:Thing` и `owl:Class` не являются двумя корневыми множествами одного типа

Семантически:

\[
owl:Thing^C = \Delta_I
\]

Это универсальный класс объектов.

А `owl:Class` в OWL RDF vocabulary служит для RDF-представления OWL-классов. Его не следует включать в ту же схему как:

```text
owl:Thing
owl:Class
predicate set
resource set
```

Корректнее:

```text
OWL vocabulary symbols:
├── owl:Thing
├── owl:Nothing
├── owl:Class
├── owl:topObjectProperty
├── owl:bottomObjectProperty
└── ...

Direct semantic domains:
├── ΔI — object domain
└── ΔD — data domain

Interpretations:
├── classes → subsets of ΔI
├── object properties → subsets of ΔI × ΔI
├── data properties → subsets of ΔI × ΔD
├── individuals → elements of ΔI
└── datatypes → subsets of ΔD
```

## 10. Где находятся предикаты

В RDF все предикаты — IRI:

```turtle
rdf:type
rdfs:subClassOf
:hasAuthor
```

В OWL они распределяются по ролям.

### В RDF

```text
predicate IRI
```

Это третий компонент RDF-тройки.

### В OWL

```text
ObjectProperty
DataProperty
AnnotationProperty
```

### В Direct Semantics

| OWL-роль | Семантический объект |
|---|---|
| ObjectProperty | отношение `⊆ ΔI × ΔI` |
| DataProperty | отношение `⊆ ΔI × ΔD` |
| AnnotationProperty | не входит в логическую Direct Semantics |
| `rdf:type` в RDF Mapping | отображает различные OWL-конструкции, включая ClassAssertion и Declaration |

Последний пункт важен: `rdf:type` не является в Direct Semantics особым универсальным OWL-предикатом с одной-единственной интерпретацией. В RDF Mapping одна и та же RDF-конструкция может участвовать в отображении разных аксиом OWL в зависимости от объекта и контекста.

Например:

```turtle
:book123 rdf:type :Book .
```

отображает:

```owl
ClassAssertion( :Book :book123 )
```

а:

```turtle
:Book rdf:type owl:Class .
```

отображает:

```owl
Declaration( Class( :Book ) )
```

Поэтому в Direct Semantics лучше анализировать не саму строку `rdf:type`, а OWL-аксиому, в которую она преобразуется.

## 11. Две семантики OWL

OWL 2 допускает два нормативных семантических режима:

```text
OWL 2
├── Direct Semantics
└── RDF-Based Semantics
```

### 11.1 Direct Semantics

Direct Semantics работает со структурой OWL:

```text
classes
properties
individuals
class expressions
axioms
```

Её модель напрямую основана на:

```text
ΔI
ΔD
class interpretation
property interpretations
individual interpretation
datatype interpretation
```

Она тесно связана с Description Logic `SROIQ`. Для OWL 2 DL именно эта семантика позволяет использовать классические DL reasoners. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

### 11.2 RDF-Based Semantics

RDF-Based Semantics работает с RDF-графом и RDF-термами:

```text
IRIs
blank nodes
literals
triples
```

В этой семантике RDF-ресурсы могут интерпретироваться более свободно, включая ситуации, в которых один и тот же ресурс используется как класс, индивид или свойство.

Она естественнее для общего RDF-графа и метамоделирования, но не совпадает во всех случаях с Direct Semantics OWL 2 DL.

Схематично:

```text
RDF graph
   │
   ├── RDF semantics
   ├── RDFS semantics
   ├── OWL 2 RDF-Based Semantics
   └── RDF mapping → OWL structural ontology → Direct Semantics
```

Спецификация OWL 2 Structural Syntax прямо указывает, что OWL 2-онтологии могут рассматриваться либо под RDF-Based Semantics, либо под Direct Semantics; для OWL 2 DL Direct Semantics связана с вычислимыми методами Description Logic. [w3](https://www.w3.org/TR/owl2-syntax/)

## 12. Общая иерархия спецификаций

Здесь полезно различать **слои стандартов**, а не пытаться расположить их в одну вертикальную иерархию.

```text
Web / Semantic Web
│
├── IRI / URI architecture
│
├── RDF
│   ├── RDF abstract syntax
│   │   ├── RDF terms
│   │   │   ├── IRIs
│   │   │   ├── blank nodes
│   │   │   └── literals
│   │   └── triples / graphs / datasets
│   │
│   ├── RDF Semantics
│   │
│   ├── RDF Schema (RDFS)
│   │   └── rdfs:Class, rdfs:subClassOf, rdfs:domain, rdfs:range
│   │
│   └── RDF serialization syntaxes
│       ├── Turtle
│       ├── RDF/XML
│       ├── JSON-LD
│       └── N-Triples
│
├── OWL 2
│   ├── Structural Specification
│   │   ├── entities
│   │   ├── expressions
│   │   └── axioms
│   │
│   ├── Functional-Style Syntax
│   │
│   ├── RDF Mapping
│   │
│   ├── Direct Semantics
│   │   └── SROIQ-related semantics
│   │
│   ├── RDF-Based Semantics
│   │
│   └── Profiles
│       ├── OWL 2 EL
│       ├── OWL 2 QL
│       └── OWL 2 RL
│
├── Rule and constraint layers
│   ├── RIF
│   ├── SHACL
│   └── ShEx
│
└── Query and application layers
    ├── SPARQL
    ├── ontology engineering tools
    └── domain vocabularies
```

## 13. OWL 2 Profiles

Профили OWL 2 — не отдельные семантики, а ограниченные фрагменты языка, ориентированные на разные свойства вычислимости.

```text
OWL 2 Full / general OWL 2 vocabulary
└── OWL 2 DL
    ├── OWL 2 EL
    ├── OWL 2 QL
    └── OWL 2 RL
```

Такую схему надо понимать осторожно:

- `OWL 2 EL`, `OWL 2 QL`, `OWL 2 RL` — профили с ограниченными конструкциями;
- они не образуют простую вложенную линейную иерархию;
- каждый профиль оптимизирован под собственный класс reasoning-задач;
- профильные онтологии могут быть OWL 2 DL-онтологиями при соблюдении соответствующих ограничений.

### OWL 2 EL

Ориентирован на большие терминологические иерархии и полиномиальное reasoning.

### OWL 2 QL

Ориентирован на запросы к большим реляционным данным, особенно через rewriting в SQL.

### OWL 2 RL

Ориентирован на rule-based reasoning и реализацию средствами правил.

Спецификация Profiles определяет эти фрагменты и ограничения; Direct Semantics также применяется к ним как к ограниченным OWL 2-онтологиям. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

## 14. OWL 2 Full и OWL 2 DL

Их тоже нельзя трактовать просто как «два уровня».

### OWL 2 DL

Это синтаксически ограниченный фрагмент OWL 2:

```text
OWL 2 DL
```

Он обеспечивает:

- контролируемое смешение ролей;
- связь с Description Logic;
- гарантии разрешимости для основных inference-задач;
- использование Direct Semantics.

### OWL 2 Full

Это более свободное использование OWL поверх RDF/RDFS, допускающее более сильное метамоделирование:

```turtle
:Book rdf:type owl:Class .
:Book rdf:type :Concept .
:Concept rdfs:subClassOf owl:Class .
```

OWL 2 Full позволяет намного свободнее использовать ресурсы в нескольких ролях. Но за это платят отсутствием тех же общих гарантий вычислимости, что у OWL 2 DL.

## 15. Метамодель спецификации OWL

Есть ещё один уровень, который легко спутать с `owl:Class`.

OWL 2 Structural Specification описывает саму структуру OWL с помощью UML/MOF-подобной метамодели:

```text
OWL specification metamodel
├── Ontology
├── Entity
│   ├── Class
│   ├── ObjectProperty
│   ├── DataProperty
│   ├── AnnotationProperty
│   ├── Individual
│   └── Datatype
├── ClassExpression
├── Axiom
└── Annotation
```

Здесь `Class`, `Individual`, `Axiom` — **UML-классы метамодели спецификации**, а не обязательно OWL-классы, присутствующие в вашей предметной онтологии.

Спецификация специально предупреждает о различии:

```text
UML class — элемент метамодели OWL 2
OWL class  — класс предметной онтологии
```

Например:

```text
:Book — экземпляр UML meta-class Class
:book123 — экземпляр UML meta-class Individual
ClassAssertion(...) — экземпляр UML meta-class ClassAssertion
```

Но это описание структуры документа, а не дополнительные RDF-тройки в вашей онтологии. [w3](https://www.w3.org/TR/owl2-syntax/)

## 16. Сводная схема всех уровней

```text
Уровень A. RDF abstract data model
│
├── RDF terms
│   ├── IRI
│   ├── blank node
│   └── literal
│
├── RDF graph
│   └── triple = subject–predicate–object
│
└── RDF resource / referent
```

```text
Уровень B. OWL 2 structural model
│
├── Entities
│   ├── Class
│   ├── ObjectProperty
│   ├── DataProperty
│   ├── AnnotationProperty
│   ├── Individual
│   └── Datatype
│
├── Expressions
│   ├── ClassExpression
│   ├── PropertyExpression
│   └── DataRange
│
└── Axioms
    ├── Declaration
    ├── SubClassOf
    ├── EquivalentClasses
    ├── ClassAssertion
    ├── PropertyAssertion
    └── ...
```

```text
Уровень C. OWL 2 Direct Semantics
│
├── ΔI — object domain
├── ΔD — data domain
│
├── C^C ⊆ ΔI
├── OP^OP ⊆ ΔI × ΔI
├── DP^DP ⊆ ΔI × ΔD
├── a^I ∈ ΔI
├── DT^DT ⊆ ΔD
└── lt^LT ∈ ΔD
```

```text
Уровень D. OWL RDF-Based Semantics
│
├── RDF resources
├── class extensions
├── property extensions
├── datatype mappings
└── RDF entailment rules / semantic conditions
```

```text
Уровень E. OWL specification metamodel
│
├── UML Class
├── UML Individual
├── UML Axiom
├── UML Ontology
└── structural associations
```

## 17. Как лучше формулировать в документации

Чтобы не смешивать уровни, рекомендую использовать такие обозначения:

### В RDF

```text
RDF term
RDF resource
IRI
RDF predicate
RDF object
RDF triple
```

### В OWL-синтаксисе

```text
OWL entity
OWL class
OWL individual
object property
data property
class expression
OWL axiom
```

### В Direct Semantics

```text
object domain ΔI
data domain ΔD
class interpretation
object-property interpretation
data-property interpretation
individual interpretation
datatype interpretation
model
interpretation
```

### В спецификации языка

```text
structural element
UML class of the OWL metamodel
ontology document
syntactic category
```

Не следует говорить:

```text
множество owl:Thing
множество owl:Class
множество предикатов
```

без уточнения уровня.

Точнее:

```text
owl:Thing^C = ΔI
```

— интерпретация класса `owl:Thing` совпадает с областью объектов;

```text
owl:Nothing^C = ∅
```

— интерпретация `owl:Nothing` пуста;

```text
OP^OP ⊆ ΔI × ΔI
```

— интерпретация object property является бинарным отношением;

```text
DP^DP ⊆ ΔI × ΔD
```

— интерпретация data property связывает объект с data value.

## Итоговая формула

Самая компактная корректная схема выглядит так:

\[
\boxed{
\begin{aligned}
&\text{RDF terms} =
\text{IRIs} \cup \text{blank nodes} \cup \text{literals}
\\[2mm]
&\text{OWL entities} =
\text{classes} \cup \text{properties} \cup \text{individuals} \cup \text{datatypes}
\\[2mm]
&\text{Direct domains} =
\Delta_I \cup \Delta_D
\\[2mm]
&C^C \subseteq \Delta_I
\\
&OP^{OP} \subseteq \Delta_I \times \Delta_I
\\
&DP^{DP} \subseteq \Delta_I \times \Delta_D
\\
&a^I \in \Delta_I
\\
&DT^{DT} \subseteq \Delta_D
\end{aligned}
}
\]

И главное:

```text
RDF resource
    ↓ может обозначать
OWL entity
    ↓ получает
OWL structural role
    ↓ интерпретируется в
Direct Semantics:
ΔI, ΔD, subsets, relations, individuals, data values
```

`owl:Thing` — специальный OWL-класс, чья интерпретация равна `ΔI`.  
`owl:Class` — RDF/OWL vocabulary term для обозначения классов, но не самостоятельное корневое семантическое множество.  
Предикаты — RDF IRIs; в Direct Semantics они становятся отношениями, если являются object/data properties.  
«Сущность» в OWL — именованный элемент структурной модели, а «ресурс» в RDF — более общий термин, относящийся к RDF-модели.  [w3](https://www.w3.org/TR/owl2-direct-semantics/)

Может быть для моего концепта (разделение на мир категорий - owl:Class и вещей owl:Thing) подходит OWL DL? Там ведь они взаимоисключающие множества.

### 2
Да, **OWL 2 DL подходит значительно лучше, чем OWL 2 Full**, если вы хотите строго разделить:

- мир вещей — индивиды;
- мир категорий — классы;
- свойства — отдельный вид сущностей.

Но есть важная поправка:

> В OWL 2 DL классы и индивиды разделены прежде всего как **роли сущностей и элементы словаря**, а не обязательно как два непересекающихся множества элементов одной предметной области.

OWL 2 DL обеспечивает **типовое разделение vocabulary**, но не означает автоматически, что `owl:Class` и `owl:Thing` — два класса, между которыми нужно задавать аксиому `DisjointClasses`.

## 1. Что именно разделяет OWL 2 DL

Упрощённо OWL 2 DL вводит такие категории имён:

```text
OWL 2 DL vocabulary
├── Class names
├── Object property names
├── Data property names
├── Annotation property names
├── Individual names
└── Datatype names
```

В классическом описании OWL DL это называлось **vocabulary separation**: классы, свойства и индивиды не должны свободно смешиваться. Однако OWL 2 DL допускает ограниченное метамоделирование через **punning** — один IRI может использоваться в разных ролях, например одновременно как класс и индивид, но эти роли имеют независимые интерпретации. [w3](https://www.w3.org/TR/owl2-new-features/)

В целом:

```text
OWL 2 DL — типизированная логика описаний
OWL 2 Full — более свободная RDF-совместимая метамодель
```

## 2. Что такое мир вещей в OWL 2 DL

В Direct Semantics есть область объектов:

\[
\Delta_I
\]

Это и есть домен индивидов — объектов, о которых говорит онтология.

Например:

```turtle
:book123 rdf:type :Book .
:tolstoy rdf:type :Person .
```

В Direct Semantics:

\[
(:book123)^I \in \Delta_I
\]

\[
(:tolstoy)^I \in \Delta_I
\]

То есть `:book123` и `:tolstoy` интерпретируются как элементы object domain.

`owl:Thing` имеет интерпретацию:

\[
owl:Thing^C = \Delta_I
\]

Поэтому `owl:Thing` можно считать универсальным классом всех объектов предметной области:

```text
owl:Thing — универсальная категория объектов
```

Но это не отдельное множество, внешнее по отношению к `ΔI`:

```text
owl:Thing^C = ΔI
```

## 3. Что такое мир категорий

В Direct Semantics OWL 2 класс не является элементом отдельного домена `ΔClass`.

Класс интерпретируется как подмножество object domain:

\[
C^C \subseteq \Delta_I
\]

Например:

\[
:Book^C \subseteq \Delta_I
\]

Это означает:

```text
:Book интерпретируется множеством элементов из ΔI,
которые являются книгами.
```

Следовательно, в строгой Direct Semantics OWL 2:

```text
мир классов ≠ отдельный корневой домен категорий
```

Вместо этого:

```text
класс = множество объектов ΔI
```

Именно поэтому обычная логика OWL 2 DL не строится на двух доменах:

```text
ΔThing
ΔClass
```

Она строится на:

```text
ΔI — область объектов
ΔD — область значений данных
```

а классы являются подмножествами `ΔI`.

## 4. Где возникает `owl:Class`

Запись:

```turtle
:Book rdf:type owl:Class .
```

в RDF Mapping соответствует структурной OWL-декларации:

```owl
Declaration( Class( :Book ) )
```

Она означает:

```text
:Book используется как OWL-сущность типа Class.
```

Но не означает, что Direct Semantics вводит отдельный объект:

```text
(:Book)^I ∈ ΔClass
```

Такого общего домена `ΔClass` в Direct Semantics OWL 2 DL нет.

Более точная схема:

```text
RDF vocabulary:
:Book rdf:type owl:Class

OWL structural syntax:
Declaration( Class( :Book ) )

Direct Semantics:
:Book^C ⊆ ΔI
```

Поэтому `owl:Class` — прежде всего элемент OWL/RDF vocabulary и средство представления декларации класса, а не «множество всех категорий» в том же смысле, в каком `owl:Thing` представляет область объектов.

## 5. Можно ли сказать, что классы и индивиды взаимоисключающие?

Только с уточнением уровня.

### В OWL 2 DL — да, в смысле ролей

В OWL 2 DL IRI обычно не может произвольно использоваться одновременно во всех категориях. Например, существуют ограничения на смешение:

```text
Class
Datatype
ObjectProperty
DataProperty
```

Один IRI не может свободно играть любую из этих ролей.

### Но не полностью в смысле ресурсов

OWL 2 DL допускает punning:

```turtle
:Eagle rdf:type owl:Class .
:Eagle rdf:type :Species .
```

Здесь `:Eagle` может использоваться:

- как класс птиц;
- как индивидуальный объект, представляющий вид Eagle.

Это не означает, что класс и индивид буквально отождествлены. OWL 2 DL рассматривает эти использования как разные семантические роли, несмотря на одинаковый IRI.

Официальное описание OWL 2 прямо приводит пример с `Eagle`: один и тот же термин может обозначать класс орлов и индивид, представляющий биологический вид. [w3](https://www.w3.org/TR/owl2-new-features/)

Поэтому правильная формулировка:

> OWL 2 DL обеспечивает ограниченное разделение ролей и допускает контролируемое метамоделирование, но не является строгой двухсортной логикой «мир классов против мира вещей».

## 6. Почему `owl:Class` и `owl:Thing` нельзя просто объявить непересекающимися

Можно попытаться записать:

```turtle
owl:Class owl:disjointWith owl:Thing .
```

Но это почти наверняка не выражает ваш замысел.

`owl:disjointWith` — это аксиома о классах индивидов:

```turtle
C owl:disjointWith D .
```

Она означает:

\[
C^C \cap D^C = \varnothing
\]

То есть ни один объект предметной области не может быть одновременно членом `C` и `D`.

Но:

```text
owl:Thing^C = ΔI
```

Если объявить:

```text
owl:Class owl:disjointWith owl:Thing .
```

то получится:

\[
owl:Class^C \cap \Delta_I = \varnothing
\]

а так как `owl:Class^C` должно быть подмножеством `ΔI`, это приведёт к:

\[
owl:Class^C = \varnothing
\]

То есть вы фактически объявите `owl:Class` пустым классом. Это не разделит «мир классов» и «мир вещей», а создаст противоречивую или бессодержательную модель.

## 7. Почему `owl:Thing` не является миром вещей в смысле второго сорта

`owl:Thing` — это класс всех объектов `ΔI`:

\[
owl:Thing^C = \Delta_I
\]

Класс `:Book` является подмножеством `owl:Thing`:

\[
:Book^C \subseteq owl:Thing^C
\]

Индивид `:book123` принадлежит `owl:Thing`:

\[
(:book123)^I \in owl:Thing^C
\]

Но сам класс `:Book` не обязан быть отдельным элементом «мира классов» в Direct Semantics. Его интерпретация — множество:

\[
:Book^C \subseteq \Delta_I
\]

Поэтому более точная схема OWL 2 DL такова:

```text
ΔI — объекты интерпретации
│
├── :book123
├── :tolstoy
├── ...
│
├── :Book^C — подмножество ΔI
├── :Person^C — подмножество ΔI
└── :Publication^C — подмножество ΔI
```

Не следует рисовать `:Book` как объект, находящийся «над» `ΔI`, если вы описываете именно Direct Semantics.

## 8. Ваша концепция в двух возможных вариантах

Ваше разделение может означать две разные модели.

### Вариант A: категории как классы, вещи как индивиды

Это обычное применение OWL 2 DL:

```turtle
:Book rdf:type owl:Class .

:book123 rdf:type :Book .
```

Модель:

```text
:Book    — класс, множество книг
:book123 — индивид, конкретная книга
```

Здесь OWL 2 DL подходит.

### Вариант B: категории как отдельные объекты метамодели

Вы хотите, чтобы:

```text
CategoryWorld ∩ ThingWorld = ∅
```

и чтобы категории сами были объектами отдельного типа:

```text
:Book rdf:type :Category .
:book123 rdf:type :Thing .
```

Тогда обычная модель OWL 2 DL недостаточна как естественная формализация, поскольку в ней нет двух независимых корневых доменов:

```text
ΔCategory
ΔThing
```

Можно моделировать это прикладными классами:

```turtle
:Category rdf:type owl:Class .
:Thing rdf:type owl:Class .

:Category owl:disjointWith :Thing .

:Book rdf:type :Category .
:book123 rdf:type :Thing .
```

Но это уже будет утверждение о **двух классах внутри общего `ΔI`**, а не создание двух различных семантических доменов.

Формально:

\[
:Category^C \subseteq \Delta_I
\]

\[
:Thing^C \subseteq \Delta_I
\]

\[
:Category^C \cap :Thing^C = \varnothing
\]

Это может быть полезной прикладной моделью, но не тем же самым, что строгое разделение классов и индивидов на уровне метасемантики OWL.

## 9. Как приблизить вашу модель в OWL 2 DL

Если вы хотите ввести явную метамодель:

```turtle
@prefix :    <http://example.org/> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

:Category rdf:type owl:Class .
:Thing    rdf:type owl:Class .

:Category owl:disjointWith :Thing .

:Book rdf:type :Category .
:book123 rdf:type :Thing .
```

Это моделирует:

```text
:Category — класс метаобъектов-категорий
:Thing    — класс объектов-вещей
:Category и :Thing — непересекающиеся классы
:Book     — индивид метакласса :Category
:book123  — индивид класса :Thing
```

Но тогда `:Book` уже используется как **индивид класса `:Category`**, а одновременно вы, возможно, хотите использовать его как OWL-класс:

```turtle
:Book rdf:type owl:Class .
```

Это приводит к punning:

```turtle
:Book rdf:type owl:Class ;
      rdf:type :Category .
```

В OWL 2 DL это может быть допустимо как ограниченное метамоделирование, но нужно понимать: роль `:Book` как OWL-класса и роль `:Book` как индивида `:Category` не объединяются автоматически.

Это не означает:

```text
owl:Class ≡ :Category
```

и не означает, что множество экземпляров `:Book` совпадает с множеством элементов `:Category`.

## 10. Что лучше использовать: OWL 2 DL или OWL 2 Full

| Требование | OWL 2 DL | OWL 2 Full |
|---|---:|---:|
| Строгое reasoning | Да | Нет общих гарантий |
| Классы и свойства как типизированные роли | Да | Более свободно |
| Ограниченное метамоделирование | Да, через punning | Да, свободнее |
| Один ресурс в нескольких ролях | Ограниченно | Практически свободно |
| Два независимых семантических домена | Нет напрямую | Тоже не автоматически |
| Отдельный класс категорий | Да, прикладным моделированием | Да |
| Использование классов как объектов | Ограниченно | Естественно |
| Полная RDF-совместимость | Ограниченнее | Да |

OWL 2 DL следует выбирать, если важны:

- проверяемость;
- предсказуемая семантика;
- decidability;
- reasoning;
- контроль над ролями сущностей.

OWL 2 Full имеет смысл, если основная цель — свободное RDF-метамоделирование, а строгая вычислимость не является обязательной. [w3](https://www.w3.org/TR/owl-features/)

## 11. Наиболее точный вывод для вашего концепта

Если ваша идея такова:

```text
вещь = индивидуальный объект
категория = класс, задающий множество вещей
```

то это естественная модель OWL 2 DL:

```turtle
:Book rdf:type owl:Class .
:book123 rdf:type :Book .
```

Если же идея такова:

```text
мир вещей и мир категорий — два непересекающихся типа объектов
```

то OWL 2 DL **не реализует это напрямую на уровне Direct Semantics**, потому что:

```text
есть ΔI и ΔD,
но нет стандартного ΔCategory.
```

Можно только ввести прикладные классы:

```turtle
:Category rdf:type owl:Class .
:Thing rdf:type owl:Class .

:Category owl:disjointWith :Thing .
```

Но это будет разделение двух категорий внутри общего домена `ΔI`, а не разделение фундаментальных семантических сортов.

## Рекомендуемая формулировка

Для вашей модели я бы сформулировал так:

> В OWL 2 DL индивиды являются объектами области интерпретации `ΔI`, а классы интерпретируются как подмножества этой области. Поэтому OWL 2 DL поддерживает типовое различение сущностей «класс» и «индивид», но не вводит отдельную семантическую область классов, противопоставленную области индивидов. Если категории должны рассматриваться как объекты метамодели, их следует моделировать отдельным классом `:Category`; при этом использование тех же IRI как OWL-классов является ограниченным метамоделированием/punning.

Иными словами:

```text
OWL 2 DL подходит для:
    класс :Book → множество вещей-книг
    индивид :book123 → одна конкретная вещь

OWL 2 DL не даёт напрямую:
    ΔCategory ∩ ΔThing = ∅
```

Для последнего нужна либо явная многосортная логика поверх OWL, либо собственная метамодель, либо прикладное моделирование через классы `:Category` и `:Thing`. [w3](https://www.w3.org/TR/owl2-new-features/)

### 2

Ниже — полностью переписанный предыдущий ответ в формате, который можно напрямую вставить в GitHub Markdown. Все формулы оформлены через `\( ... \)` или `\[ ... \]`.

***

Да, **OWL 2 DL подходит значительно лучше, чем OWL 2 Full**, если вы хотите строго разделить:

- мир вещей — индивиды;
- мир категорий — классы;
- свойства — отдельный вид сущностей.

Но есть важная поправка:

> В OWL 2 DL классы и индивиды разделены прежде всего как **роли сущностей и элементы словаря**, а не обязательно как два непересекающихся множества элементов одной предметной области.

OWL 2 DL обеспечивает **типовое разделение vocabulary**, но не означает автоматически, что `owl:Class` и `owl:Thing` — два класса, между которыми нужно задавать аксиому `DisjointClasses`.

## 1. Что именно разделяет OWL 2 DL

Упрощённо OWL 2 DL вводит такие категории имён:

```text
OWL 2 DL vocabulary
├── Class names
├── Object property names
├── Data property names
├── Annotation property names
├── Individual names
└── Datatype names
```

В классическом описании OWL DL это называлось **vocabulary separation**: классы, свойства и индивиды не должны свободно смешиваться. Однако OWL 2 DL допускает ограниченное метамоделирование через **punning** — один IRI может использоваться в разных ролях, например одновременно как класс и индивид, но эти роли имеют независимые интерпретации.

В целом:

```text
OWL 2 DL — типизированная логика описаний
OWL 2 Full — более свободная RDF-совместимая метамодель
```

## 2. Что такое мир вещей в OWL 2 DL

В Direct Semantics есть область объектов:

\[
\Delta_I
\]

Это **object domain**, то есть область индивидов — объектов, о которых говорит онтология.

Например:

```turtle
:book123 rdf:type :Book .
:tolstoy rdf:type :Person .
```

В Direct Semantics:

\[
\texttt{:book123}^{I} \in \Delta_I
\]

\[
\texttt{:tolstoy}^{I} \in \Delta_I
\]

То есть `:book123` и `:tolstoy` интерпретируются как элементы object domain.

`owl:Thing` имеет интерпретацию:

\[
\texttt{owl:Thing}^{C} = \Delta_I
\]

Поэтому `owl:Thing` можно считать универсальным классом всех объектов предметной области:

```text
owl:Thing — универсальная категория объектов
```

Но это не отдельное множество, внешнее по отношению к `ΔI`:

\[
\texttt{owl:Thing}^{C} = \Delta_I
\]

## 3. Что такое мир категорий

В Direct Semantics OWL 2 класс не является элементом отдельного домена `ΔClass`.

Класс интерпретируется как подмножество object domain:

\[
C^{C} \subseteq \Delta_I
\]

Например:

\[
\texttt{:Book}^{C} \subseteq \Delta_I
\]

Это означает:

```text
:Book интерпретируется множеством элементов из ΔI,
которые являются книгами.
```

Следовательно, в строгой Direct Semantics OWL 2:

```text
мир классов ≠ отдельный корневой домен категорий
```

Вместо этого:

```text
класс = множество объектов ΔI
```

Именно поэтому обычная логика OWL 2 DL не строится на двух доменах:

```text
ΔThing
ΔClass
```

Она строится на:

```text
ΔI — область объектов
ΔD — область значений данных
```

а классы являются подмножествами `ΔI`.

## 4. Где возникает `owl:Class`

Запись:

```turtle
:Book rdf:type owl:Class .
```

в RDF Mapping соответствует структурной OWL-декларации:

```owl
Declaration( Class( :Book ) )
```

Она означает:

```text
:Book используется как OWL-сущность типа Class.
```

Но она не означает, что Direct Semantics вводит отдельный объект:

```text
(:Book)^I ∈ ΔClass
```

Такого общего домена `ΔClass` в Direct Semantics OWL 2 DL нет.

Более точная схема:

```text
RDF vocabulary:
:Book rdf:type owl:Class

OWL structural syntax:
Declaration( Class( :Book ) )

Direct Semantics:
:Book^C ⊆ ΔI
```

Поэтому `owl:Class` — прежде всего элемент OWL/RDF vocabulary и средство представления декларации класса, а не «множество всех категорий» в том же смысле, в каком `owl:Thing` представляет область объектов.

## 5. Можно ли сказать, что классы и индивиды взаимоисключающие?

Только с уточнением уровня.

### В OWL 2 DL — да, в смысле ролей

В OWL 2 DL IRI обычно не может произвольно использоваться одновременно во всех категориях. Например, существуют ограничения на смешение:

```text
Class
Datatype
ObjectProperty
DataProperty
```

Один IRI не может свободно играть любую из этих ролей.

### Но не полностью в смысле ресурсов

OWL 2 DL допускает punning:

```turtle
:Eagle rdf:type owl:Class .
:Eagle rdf:type :Species .
```

Здесь `:Eagle` может использоваться:

- как класс птиц;
- как индивидуальный объект, представляющий вид Eagle.

Это не означает, что класс и индивид буквально отождествлены. OWL 2 DL рассматривает эти использования как разные семантические роли, несмотря на одинаковый IRI.

Поэтому правильная формулировка:

> OWL 2 DL обеспечивает ограниченное разделение ролей и допускает контролируемое метамоделирование, но не является строгой двухсортной логикой «мир классов против мира вещей».

## 6. Почему `owl:Class` и `owl:Thing` нельзя просто объявить непересекающимися

Можно попытаться записать:

```turtle
owl:Class owl:disjointWith owl:Thing .
```

Но это почти наверняка не выражает ваш замысел.

`owl:disjointWith` — это аксиома о классах индивидов:

```turtle
C owl:disjointWith D .
```

Она означает:

\[
C^{C} \cap D^{C} = \varnothing
\]

То есть ни один объект предметной области не может быть одновременно членом `C` и `D`.

Но:

\[
\texttt{owl:Thing}^{C} = \Delta_I
\]

Если объявить:

```turtle
owl:Class owl:disjointWith owl:Thing .
```

то получится:

\[
\texttt{owl:Class}^{C} \cap \Delta_I = \varnothing
\]

а так как:

\[
\texttt{owl:Class}^{C} \subseteq \Delta_I
\]

это приведёт к:

\[
\texttt{owl:Class}^{C} = \varnothing
\]

То есть вы фактически объявите `owl:Class` пустым классом. Это не разделит «мир классов» и «мир вещей», а создаст противоречивую или бессодержательную модель.

## 7. Почему `owl:Thing` не является миром вещей в смысле второго сорта

`owl:Thing` — это класс всех объектов `ΔI`:

\[
\texttt{owl:Thing}^{C} = \Delta_I
\]

Класс `:Book` является подмножеством `owl:Thing`:

\[
\texttt{:Book}^{C} \subseteq \texttt{owl:Thing}^{C}
\]

Индивид `:book123` принадлежит `owl:Thing`:

\[
\texttt{:book123}^{I} \in \texttt{owl:Thing}^{C}
\]

Но сам класс `:Book` не обязан быть отдельным элементом «мира классов» в Direct Semantics. Его интерпретация — множество:

\[
\texttt{:Book}^{C} \subseteq \Delta_I
\]

Поэтому более точная схема OWL 2 DL такова:

```text
ΔI — объекты интерпретации
│
├── :book123
├── :tolstoy
├── ...
│
├── :Book^C — подмножество ΔI
├── :Person^C — подмножество ΔI
└── :Publication^C — подмножество ΔI
```

Не следует рисовать `:Book` как объект, находящийся «над» `ΔI`, если вы описываете именно Direct Semantics.

## 8. Ваша концепция в двух возможных вариантах

Ваша идея может означать две разные модели.

### Вариант A: категории как классы, вещи как индивиды

Это обычное применение OWL 2 DL:

```turtle
:Book rdf:type owl:Class .

:book123 rdf:type :Book .
```

Модель:

```text
:Book    — класс, множество книг
:book123 — индивид, конкретная книга
```

Здесь OWL 2 DL подходит.

### Вариант B: категории как отдельные объекты метамодели

Вы хотите, чтобы:

```text
CategoryWorld ∩ ThingWorld = ∅
```

и чтобы категории сами были объектами отдельного типа:

```turtle
:Book rdf:type :Category .
:book123 rdf:type :Thing .
```

Тогда обычная модель OWL 2 DL недостаточна как естественная формализация, поскольку в ней нет двух независимых корневых доменов:

```text
ΔCategory
ΔThing
```

Можно моделировать это прикладными классами:

```turtle
:Category rdf:type owl:Class .
:Thing rdf:type owl:Class .

:Category owl:disjointWith :Thing .

:Book rdf:type :Category .
:book123 rdf:type :Thing .
```

Но это уже будет утверждение о **двух классах внутри общего `ΔI`**, а не создание двух различных семантических доменов.

Формально:

\[
\texttt{:Category}^{C} \subseteq \Delta_I
\]

\[
\texttt{:Thing}^{C} \subseteq \Delta_I
\]

\[
\texttt{:Category}^{C} \cap \texttt{:Thing}^{C} = \varnothing
\]

Это может быть полезной прикладной моделью, но не является тем же самым, что строгое разделение классов и индивидов на уровне метасемантики OWL.

## 9. Как приблизить вашу модель в OWL 2 DL

Если вы хотите ввести явную метамодель:

```turtle
@prefix :    <http://example.org/> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

:Category rdf:type owl:Class .
:Thing    rdf:type owl:Class .

:Category owl:disjointWith :Thing .

:Book rdf:type :Category .
:book123 rdf:type :Thing .
```

Это моделирует:

```text
:Category — класс метаобъектов-категорий
:Thing    — класс объектов-вещей
:Category и :Thing — непересекающиеся классы
:Book     — индивид метакласса :Category
:book123  — индивид класса :Thing
```

Но тогда `:Book` уже используется как **индивид класса `:Category`**, а одновременно вы, возможно, хотите использовать его как OWL-класс:

```turtle
:Book rdf:type owl:Class .
```

Это приводит к punning:

```turtle
:Book rdf:type owl:Class ;
      rdf:type :Category .
```

В OWL 2 DL это может быть допустимо как ограниченное метамоделирование, но нужно понимать: роль `:Book` как OWL-класса и роль `:Book` как индивида `:Category` не объединяются автоматически.

Это не означает:

```text
owl:Class ≡ :Category
```

и не означает, что множество экземпляров `:Book` совпадает с множеством элементов `:Category`.

## 10. Что лучше использовать: OWL 2 DL или OWL 2 Full

| Требование | OWL 2 DL | OWL 2 Full |
|---|---:|---:|
| Строгое reasoning | Да | Нет общих гарантий |
| Классы и свойства как типизированные роли | Да | Более свободно |
| Ограниченное метамоделирование | Да, через punning | Да, свободнее |
| Один ресурс в нескольких ролях | Ограниченно | Практически свободно |
| Два независимых семантических домена | Нет напрямую | Тоже не автоматически |
| Отдельный класс категорий | Да, прикладным моделированием | Да |
| Использование классов как объектов | Ограниченно | Естественно |
| Полная RDF-совместимость | Ограниченнее | Да |

OWL 2 DL следует выбирать, если важны:

- проверяемость;
- предсказуемая семантика;
- разрешимость;
- reasoning;
- контроль над ролями сущностей.

OWL 2 Full имеет смысл, если основная цель — свободное RDF-метамоделирование, а строгая вычислимость не является обязательной.

## 11. Наиболее точный вывод для вашего концепта

Если ваша идея такова:

```text
вещь = индивидуальный объект
категория = класс, задающий множество вещей
```

то это естественная модель OWL 2 DL:

```turtle
:Book rdf:type owl:Class .
:book123 rdf:type :Book .
```

Если же идея такова:

```text
мир вещей и мир категорий — два непересекающихся типа объектов
```

то OWL 2 DL **не реализует это напрямую на уровне Direct Semantics**, потому что:

```text
есть ΔI и ΔD,
но нет стандартного ΔCategory.
```

Можно только ввести прикладные классы:

```turtle
:Category rdf:type owl:Class .
:Thing rdf:type owl:Class .

:Category owl:disjointWith :Thing .
```

Но это будет разделение двух категорий внутри общего домена `ΔI`, а не разделение фундаментальных семантических сортов.

## Рекомендуемая формулировка

Для вашей модели я бы сформулировал так:

> В OWL 2 DL индивиды являются объектами области интерпретации `ΔI`, а классы интерпретируются как подмножества этой области. Поэтому OWL 2 DL поддерживает типовое различение сущностей «класс» и «индивид», но не вводит отдельную семантическую область классов, противопоставленную области индивидов. Если категории должны рассматриваться как объекты метамодели, их следует моделировать отдельным классом `:Category`; при этом использование тех же IRI как OWL-классов является ограниченным метамоделированием/punning.

Иными словами:

```text
OWL 2 DL подходит для:
    класс :Book → множество вещей-книг
    индивид :book123 → одна конкретная вещь

OWL 2 DL не даёт напрямую:
    ΔCategory ∩ ΔThing = ∅
```

Для последнего нужна либо явная многосортная логика поверх OWL, либо собственная метамодель, либо прикладное моделирование через классы `:Category` и `:Thing`.

