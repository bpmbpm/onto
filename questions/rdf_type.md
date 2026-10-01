## 1

` :Book rdf:type owl:Class .` — это утверждение о том, что `:Book` является **классом OWL**. В Turtle его эквивалентная сокращённая форма:

```turtle
:Book a owl:Class .
```

Здесь `a` — сокращение Turtle для `rdf:type`.

## Эквивалентные записи

### 1. Turtle

```turtle
@prefix :    <http://example.org/> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .

:Book rdf:type owl:Class .
```

или:

```turtle
:Book a owl:Class .
```

Эти две записи полностью эквивалентны.

### 2. RDF/XML

```xml
<owl:Class rdf:about="http://example.org/Book"/>
```

При наличии объявления пространств имён:

```xml
<rdf:RDF
    xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
    xmlns:owl="http://www.w3.org/2002/07/owl#"
    xml:base="http://example.org/">

    <owl:Class rdf:about="Book"/>
</rdf:RDF>
```

Это соответствует тройке:

```turtle
<http://example.org/Book> rdf:type owl:Class .
```

### 3. OWL 2 Functional-Style Syntax

```owl
Declaration( Class( :Book ) )
```

В OWL 2 это декларация класса. В RDF-графе она сериализуется как:

```turtle
:Book rdf:type owl:Class .
```

Иными словами:

\[
\operatorname{Declaration}(\operatorname{Class}(:Book))
\quad\equiv\quad
:Book\ \texttt{rdf:type}\ \texttt{owl:Class}
\]

## Важное различие: класс и экземпляр

Следует различать два утверждения:

```turtle
:Book rdf:type owl:Class .
```

и

```turtle
:myBook rdf:type :Book .
```

Их смысл различен:

| Тройка | Смысл |
|---|---|
| `:Book rdf:type owl:Class` | `:Book` — класс |
| `:myBook rdf:type :Book` | `:myBook` — экземпляр класса `:Book` |

Полный пример:

```turtle
@prefix :    <http://example.org/> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .

:Book rdf:type owl:Class .

:myBook rdf:type :Book .
```

## `owl:Class` или `rdfs:Class`?

Для OWL-онтологии обычно используется:

```turtle
:Book a owl:Class .
```

В RDF Schema допустима запись:

```turtle
:Book a rdfs:Class .
```

`owl:Class` является подклассом `rdfs:Class`:

```turtle
owl:Class rdfs:subClassOf rdfs:Class .
```

Поэтому `owl:Class` выражает, что ресурс является классом в языке OWL, а `rdfs:Class` — более общее понятие класса RDF Schema. Для OWL 2-декларации класса нормативной формой является:

```turtle
:Book rdf:type owl:Class .
```

## Если требуется задать равенство двух классов

Если под «равнозначным утверждением» имеется в виду **равенство классов**, например `:Book` и `schema:Book`, тогда используется не `rdf:type`, а `owl:equivalentClass`:

```turtle
@prefix :       <http://example.org/> .
@prefix schema: <http://schema.org/> .
@prefix owl:    <http://www.w3.org/2002/07/owl#> .

:Book a owl:Class ;
      owl:equivalentClass schema:Book .
```

Смысл:

\[
\operatorname{Book}^{I}
=
\operatorname{schema:Book}^{I}
\]

То есть оба класса имеют одно и то же множество экземпляров. `owl:equivalentClass` симметрично: из

```turtle
:Book owl:equivalentClass schema:Book .
```

следует также:

```turtle
schema:Book owl:equivalentClass :Book .
```

Но это **не означает**, что два URI являются буквально одним и тем же ресурсом. Для идентичности ресурсов используется:

```turtle
:Book owl:sameAs schema:Book .
```

Однако для классов предпочтительно применять именно `owl:equivalentClass`, поскольку оно выражает равенство **расширений классов**, а не обязательно идентичность самих URI.

## Ссылки на спецификации

- [OWL 2 Web Ontology Language — Structural Specification and Functional-Style Syntax](https://www.w3.org/TR/owl2-syntax/) — декларации классов `Declaration( Class( ... ) )` и аксиома `EquivalentClasses`.
- [OWL 2 Web Ontology Language — Mapping to RDF Graphs](https://www.w3.org/TR/owl2-mapping-to-rdf/) — отображение OWL 2 в RDF-графы, включая `rdf:type owl:Class`.
- [OWL 2 Web Ontology Language — RDF-Based Semantics](https://www.w3.org/TR/owl2-rdf-based-semantics/) — семантика `owl:Class` и `owl:equivalentClass`.
- [RDF 1.1 Turtle](https://www.w3.org/TR/turtle/) — синтаксис Turtle и сокращение `a` для `rdf:type`.
- [RDF Schema 1.1](https://www.w3.org/TR/rdf-schema/) — смысл `rdf:type`, `rdfs:Class` и классов RDF Schema.
- [OWL Web Ontology Language Reference](https://www.w3.org/TR/owl-ref/) — описание `owl:Class` и `owl:equivalentClass`. Сама спецификация указывает, что утверждение вида `x rdf:type owl:Class` объявляет `x` классом OWL. [w3](https://www.w3.org/TR/owl-ref/)

- ## 2

- Я хотел бы использовать иной предикат вместо rdf:type  
Т.е. rdf:type оставить для указания принадлежности индивида к классу, а вот классу, например, :Book указывать принадлежность к owl:Class (сказать ,что :Book это категория) иным предикатом.

Понял: вы хотите **разделить два отношения**:

1. принадлежность индивида классу:

```turtle
:myBook rdf:type :Book .
```

2. принадлежность самого ресурса `:Book` к метаклассу `owl:Class`, но через иной предикат, например:

```turtle
:Book :isCategoryOf owl:Class .
```

Короткий ответ: **для стандартной OWL-семантики так сделать нельзя**, если требуется, чтобы `:Book` распознавался OWL-инструментами как класс. Нормативное представление декларации OWL-класса — именно:

```turtle
:Book rdf:type owl:Class .
```

Специального альтернативного предиката OWL для этой цели нет. [w3](https://www.w3.org/TR/owl2-mapping-to-rdf/)

## Почему `rdf:type` используется дважды

В RDF `rdf:type` выражает общее отношение экземпляра к классу:

```turtle
x rdf:type C .
```

В данном выражении:

- `x` — ресурс;
- `C` — класс;
- смысл: `x` является экземпляром `C`.

Поэтому обе записи имеют одну и ту же общую форму:

```turtle
:myBook rdf:type :Book .
:Book   rdf:type owl:Class .
```

Но уровни разные:

```text
:myBook  — экземпляр класса :Book
:Book    — экземпляр метакласса owl:Class
```

Это не ошибка модели, а обычное RDF/OWL-метамоделирование. В RDF классы сами являются ресурсами и могут быть экземплярами других классов. Спецификация RDF прямо определяет `rdf:type` как отношение принадлежности ресурса классу, а классы — как ресурсы, которые сами могут описываться RDF-свойствами. [w3](https://www.w3.org/TR/rdf12-schema/)

## Что произойдёт при замене предиката

Допустим, вы введёте:

```turtle
@prefix :    <http://example.org/> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .

:Book :isCategoryOf owl:Class .
```

Без дополнительной семантики это будет просто произвольная RDF-тройка. Для стандартного OWL reasoner она **не будет эквивалентна**:

```turtle
:Book rdf:type owl:Class .
```

То есть reasoner не обязан сделать вывод:

```turtle
:Book rdf:type owl:Class .
```

Чтобы такой вывод появился, нужно явно определить ваш предикат как эквивалентный `rdf:type`. Например, в OWL можно было бы записать:

```turtle
:isCategoryOf owl:equivalentProperty rdf:type .
```

Но это практически не решает задачу: после такой аксиомы ваш предикат получает ту же семантику, что и `rdf:type`, включая его применение к обычным экземплярам. Кроме того, это уже не «отдельное отношение» в смысловом отношении — вы объявили его эквивалентным `rdf:type`.

В OWL 2 функционально-стилевом синтаксисе это выглядело бы как:

```owl
EquivalentObjectProperties(
    :isCategoryOf
    rdf:type
)
```

Однако такой вариант обычно нежелателен для читаемости и моделирования.

## Рекомендуемый вариант

Если `:Book` — OWL-класс, оставьте стандартную декларацию:

```turtle
:Book rdf:type owl:Class .
```

А для выражения дополнительной идеи «это категория», «это концепт», «это тип сущностей» используйте отдельный аннотационный или доменный предикат:

```turtle
:Book rdf:type owl:Class ;
      :isCategory true ;
      rdfs:label "Book"@en .
```

Например:

```turtle
@prefix :    <http://example.org/> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix xsd:  <http://www.w3.org/2001/XMLSchema#> .

:Book rdf:type owl:Class ;
      :isCategory true .

:myBook rdf:type :Book .
```

Здесь:

- `rdf:type` сохраняет стандартное OWL-значение;
- `:isCategory` выражает вашу прикладную классификацию;
- `:myBook rdf:type :Book` задаёт принадлежность индивида классу.

Но `:isCategory` сам по себе не заменяет декларацию OWL-класса.

## Если нужен именно отдельный метамодельный предикат

Можно ввести метакласс и собственный предикат:

```turtle
@prefix :    <http://example.org/> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

:Category rdf:type owl:Class .

:categoryOf rdf:type owl:ObjectProperty ;
            rdfs:domain owl:Class ;
            rdfs:range  :Category .

:Book rdf:type owl:Class ;
      :categoryOf :BookCategory .

:BookCategory rdf:type :Category .
```

Однако это добавляет **дополнительное утверждение**, а не заменяет:

```turtle
:Book rdf:type owl:Class .
```

Можно также назвать предикат более естественно:

```turtle
:Book :hasMetaCategory :Concept .
```

или:

```turtle
:Book :classifiedAs :Category .
```

Пример:

```turtle
:Category rdf:type owl:Class .

:classifiedAs rdf:type owl:ObjectProperty ;
              rdfs:domain owl:Class ;
              rdfs:range :Category .

:Book rdf:type owl:Class ;
      :classifiedAs :Category .
```

Здесь `:Category` — объект вашего метамодельного утверждения, а `owl:Class` — стандартная декларация языка OWL.

## Если требуется только собственный синтаксис

Если ваша цель — не изменить семантику, а просто писать данные в собственной форме, можно использовать SHACL, ShEx или собственный профиль данных. Например:

```turtle
:Book :kind owl:Class .
```

А затем SPARQL-конструкцией преобразовывать это в OWL:

```sparql
CONSTRUCT {
    ?resource a owl:Class .
}
WHERE {
    ?resource :kind owl:Class .
}
```

Но до такого преобразования запись:

```turtle
:Book :kind owl:Class .
```

остаётся только RDF-фактом и не является OWL-декларацией класса.

## Итог

Для стандартной OWL-онтологии правильный вариант только такой:

```turtle
:Book rdf:type owl:Class .
```

А принадлежность индивида классу задаётся тем же предикатом:

```turtle
:myBook rdf:type :Book .
```

Если нужно дополнительно выразить ваше понятие «категория», используйте отдельный предикат, но не вместо `rdf:type`:

```turtle
:Book rdf:type owl:Class ;
      :classifiedAs :Category .
```

Предикат `rdf:type` не является специальным только для индивидов: в RDF/OWL он универсально выражает отношение «ресурс является экземпляром класса», включая метауровень. Поэтому `:Book rdf:type owl:Class` — нормативное и семантически корректное утверждение. [w3](https://www.w3.org/TR/owl2-mapping-to-rdf/)

## 3

получается, что вводится путаница: экземпляр - в одном случае это вещь (owl:Thing) в другом - экземпляр класса (owl:Class).  
В обоих случаях используется термин экземпляр, но они принципиально разного характера.  
Также приведи применительно к owl термины: Категория, класс, тип, сущность, объект и т.п. и их взаимоотношение (с примера из спецификации owl).

rdf:type - и принадлежность категории к суперкатегории owl:Class (т.е. введение новой категории) так и принадлежность объекта (вещи) к категории. 

Да, ваша претензия обоснованна: слово **«экземпляр»** здесь используется для отношений разных уровней. Однако в самой формальной модели OWL это не два разных логических понятия, а одно отношение членства, применяемое к разным элементам — индивидам и классам.

Главное уточнение:

> В OWL `owl:Thing` — это не «вещь» и не синоним индивида. Это универсальный класс всех объектов предметной области.  
> `owl:Class` — не класс всех обычных вещей, а метакласс, экземплярами которого являются OWL-классы.

## 1. Базовая терминология OWL

В OWL 2 используются четыре основные категории сущностей:

| Термин OWL | Смысл | Пример |
|---|---|---|
| **Individual** | конкретный объект предметной области | `:Mary`, `:book123` |
| **Class** | категория или множество индивидов | `:Person`, `:Book` |
| **Object property** | отношение между двумя индивидами | `:hasAuthor` |
| **Data property** | отношение индивида к значению данных | `:hasAge` |
| **Datatype** | множество значений данных | `xsd:integer` |

W3C OWL 2 Primer прямо формулирует это так: объекты называются **individuals**, категории — **classes**, отношения — **properties**. При этом класс обычно интерпретируется как множество индивидов. [w3](https://www.w3.org/TR/owl2-primer/)

Пример из спецификации:

```turtle
:Mary rdf:type :Person .
```

Это означает:

```text
Mary — individual;
Person — class;
Mary является членом класса Person.
```

В функциональном синтаксисе OWL это записывается как:

```owl
ClassAssertion( :Person :Mary )
```

Формальная семантика:

\[
\operatorname{Mary}^{I} \in \operatorname{Person}^{C}
\]

То есть интерпретация индивида `Mary` является элементом интерпретации класса `Person`. [w3](https://www.w3.org/TR/owl2-primer/)

## 2. `owl:Thing`, `owl:Class` и `:Book`

Ваш пример можно разложить так:

```turtle
:book123 rdf:type :Book .
:Book   rdf:type owl:Class .
```

Логически:

```text
:book123 — индивид класса :Book
:Book    — класс, то есть экземпляр метакласса owl:Class
```

Но `:book123` и `:Book` — не экземпляры одного и того же типа в понятийном смысле:

- `:book123` — объект моделируемой предметной области;
- `:Book` — элемент словаря или модели, задающий категорию объектов;
- `owl:Class` — метакласс, описывающий такие категории.

Именно здесь полезно различать **уровень предметной области** и **уровень языка моделирования**.

### Уровень предметной области

```turtle
:book123 rdf:type :Book .
```

| Элемент | Роль |
|---|---|
| `:book123` | конкретный объект |
| `:Book` | категория объектов |
| `rdf:type` | принадлежность объекта категории |

### Уровень метамодели OWL

```turtle
:Book rdf:type owl:Class .
```

| Элемент | Роль |
|---|---|
| `:Book` | объект метамодели, именующий класс |
| `owl:Class` | метакласс классов |
| `rdf:type` | принадлежность элемента метамодели метаклассу |

Таким образом, одно и то же RDF-отношение `rdf:type` применяется:

1. к объекту и предметному классу;
2. к предметному классу и метаклассу OWL.

Это действительно создаёт терминологическую нагрузку, но не логическое противоречие.

## 3. Что такое `owl:Thing`

В прямой семантике OWL 2 задаётся непустая область объектов:

\[
\Delta_I
\]

`owl:Thing` интерпретируется как вся эта область:

\[
(\texttt{owl:Thing})^C = \Delta_I
\]

Следовательно:

```text
owl:Thing — универсальный класс всех объектов объектной области.
```

В обычном RDF/OWL-трактовании любой индивид относится к `owl:Thing`:

```turtle
:Mary rdf:type owl:Thing .
:book123 rdf:type owl:Thing .
```

Чаще эти утверждения явно не записываются, поскольку они следуют из семантики OWL.

Важно:

```text
owl:Thing ≠ «индивид»
```

`owl:Thing` — **класс**, а не конкретная вещь.

В спецификации Direct Semantics `owl:Thing` определяется как класс, интерпретация которого совпадает со всей object domain. Индивиды, напротив, интерпретируются отдельной функцией как элементы этой области. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

## 4. Что такое `owl:Class`

`owl:Class` — класс, предназначенный для описания OWL-классов:

```turtle
:Book rdf:type owl:Class .
:Person rdf:type owl:Class .
:Woman rdf:type owl:Class .
```

Здесь `:Book`, `:Person` и `:Woman` являются экземплярами `owl:Class` в RDF-графе.

Но это не означает, что `:Book` является обычным экземпляром класса `owl:Class` в предметной области. Это экземпляр **метакласса**, то есть элемент метамодели.

Упрощённая схема:

```text
:book123 ──rdf:type──> :Book ──rdf:type──> owl:Class
```

В терминах уровней:

```text
уровень 0: :book123 — конкретный объект
уровень 1: :Book    — класс объектов
уровень 2: owl:Class — класс классов
```

Но эту схему не следует превращать в бесконечное жёсткое расслоение. В RDF/OWL ресурсы могут участвовать в нескольких ролях одновременно.

## 5. Почему `:Book` всё ещё может быть классом

Ресурс `:Book` имеет две связанные, но разные характеристики:

```turtle
:Book rdf:type owl:Class .
```

говорит:

```text
:Book — класс OWL.
```

А выражение:

```turtle
:book123 rdf:type :Book .
```

использует `:Book` как класс, членами которого являются индивиды.

В прямой семантике OWL это отражается не через «множество уровней экземпляров», а через разные интерпретационные функции:

- класс `C` интерпретируется множеством объектов:

\[
C^C \subseteq \Delta_I
\]

- индивид `a` интерпретируется одним объектом:

\[
a^I \in \Delta_I
\]

Для:

```turtle
:book123 rdf:type :Book .
```

условие имеет вид:

\[
:book123^I \in :Book^C
\]

Прямой семантике OWL 2 не требуется дополнительно интерпретировать `:Book` как предметный объект через утверждение:

```turtle
:Book rdf:type owl:Class .
```

Это RDF-представление декларации класса. В структурной модели OWL `:Book` уже используется как **Class**, а `rdf:type owl:Class` — его RDF-отображение. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

## 6. Термин «сущность» (`entity`)

В OWL слово **entity** имеет специальное техническое значение.

Сущность — это именованный примитивный элемент OWL-онтологии, обычно обозначенный IRI:

```text
Class             :Book
ObjectProperty    :hasAuthor
DataProperty      :hasISBN
NamedIndividual   :book123
Datatype          xsd:string
```

Поэтому:

```text
entity — родовой термин;
class, property, individual, datatype — виды сущностей.
```

Важно различать:

| Термин | Что означает |
|---|---|
| **Entity** | элемент словаря OWL |
| **Individual** | конкретный объект, на который ссылается онтология |
| **Class** | категория, интерпретируемая множеством индивидов |
| **Class expression** | описание множества индивидов, возможно без имени |
| **Axiom** | утверждение, связывающее сущности и выражения |
| **Ontology** | совокупность аксиом, сущностей и аннотаций |

Например:

```turtle
:Book rdf:type owl:Class ;
      rdfs:subClassOf :Publication .
```

Здесь:

- `:Book` — сущность типа Class;
- `:Publication` — сущность типа Class;
- `rdfs:subClassOf` — отношение между классами;
- вся конструкция — часть аксиом онтологии.

OWL 2 Structural Specification подчёркивает, что сущности, выражения и аксиомы — разные синтаксические категории. Сущности являются примитивными терминами, выражения образуют сложные описания, а аксиомы являются утверждениями, объявленными истинными в описываемой области. [w3](https://www.w3.org/TR/owl2-syntax/)

## 7. Класс и класс-выражение

Это важное различие.

### Именованный класс

```turtle
:Book rdf:type owl:Class .
```

В функциональном синтаксисе:

```owl
Declaration( Class( :Book ) )
```

` :Book` — именованный класс, то есть сущность.

### Сложное класс-выражение

Например:

```owl
ObjectIntersectionOf( :Book :PrintedPublication )
```

Это означает множество индивидов, одновременно принадлежащих обоим классам.

В RDF:

```turtle
[] a owl:Class ;
   owl:intersectionOf ( :Book :PrintedPublication ) .
```

Такое выражение может не иметь собственного IRI. Оно является не именованной сущностью, а **class expression**.

Формально:

\[
(:Book \sqcap :PrintedPublication)^C
=
:Book^C \cap :PrintedPublication^C
\]

Спецификация приводит аналогичные примеры с `ObjectUnionOf`, `ObjectIntersectionOf` и другими конструкторами классов. [w3](https://www.w3.org/TR/owl2-direct-semantics/)

## 8. Тип и принадлежность типу

В OWL термин **type** обычно означает результат применения `rdf:type` или аксиомы `ClassAssertion`.

```turtle
:Mary rdf:type :Person .
```

Читается как:

```text
Mary has type Person;
Mary is an instance of Person;
Mary is a member of Person;
Mary belongs to class Person.
```

В OWL-терминологии наиболее точно:

```text
Mary is an instance of the class Person.
```

Но фраза «тип объекта» может быть опасной, если переносить её из языков программирования.

В Java, C# или UML «тип» часто понимается как структура или ограничение допустимых операций. В OWL класс прежде всего интерпретируется как **множество индивидов**:

```text
Person = множество всех индивидов, являющихся людьми
```

Поэтому:

```turtle
:Mary rdf:type :Person .
```

означает не обязательно, что `:Person` — «тип» в программном смысле. Точнее:

\[
\text{:Mary}^I \in \text{:Person}^C
\]

## 9. Категория

Термин **категория** не является отдельным базовым конструктивным элементом OWL 2.

В OWL 2 Primer слово *category* используется в неформальном объяснении:

```text
categories are represented by classes
```

То есть практическое соответствие такое:

```text
категория ≈ OWL class
```

Например:

```text
Person
Woman
Book
Publication
```

могут быть категориями, которые в OWL моделируются классами.

Но «категория» может обозначать разные вещи в естественном языке:

- класс объектов;
- таксономический узел;
- тип;
- понятие;
- концепт;
- значение классификатора;
- фасет или аспект классификации.

Поэтому в OWL-онтологии лучше использовать точный термин по ситуации:

| Если имеется в виду | Рекомендуемый термин |
|---|---|
| множество индивидов | `owl:Class` |
| конкретный объект | individual |
| связь между классами | `rdfs:subClassOf` |
| принадлежность индивида классу | `rdf:type` |
| область классификатора | отдельный class или vocabulary |
| описание класса | annotation |
| типизация в бизнес-модели | domain-specific class |

## 10. Объект

Слово **объект** в OWL неоднозначно.

### Объект предметной области

Обычно это individual:

```turtle
:book123 rdf:type :Book .
```

` :book123` — конкретный объект, моделируемый онтологией.

### Объект RDF-тройки

В RDF объектом называется третий элемент тройки:

```turtle
:book123 :hasAuthor :Tolstoy .
```

Здесь:

- `:book123` — RDF subject;
- `:hasAuthor` — RDF predicate;
- `:Tolstoy` — RDF object.

Но RDF object не обязательно является «объектом предметной области»: им может быть литерал:

```turtle
:book123 :title "Война и мир"@ru .
```

Здесь строка — RDF object, но не OWL individual.

### OWL object property

Термин `ObjectProperty` означает свойство, связывающее **индивиды с индивидами**:

```turtle
:book123 :hasAuthor :Tolstoy .
```

В отличие от `DatatypeProperty`:

```turtle
:book123 :pageCount 1225 .
```

Следовательно, выражение «объект» следует уточнять:

```text
RDF object;
OWL individual;
объект предметной области;
значение object property.
```

## 11. Сущность и объект — не одно и то же

Рассмотрим:

```turtle
:Book rdf:type owl:Class .
:book123 rdf:type :Book .
```

| Ресурс | В роли сущности OWL | В роли объекта моделирования |
|---|---|---|
| `:Book` | Class entity | понятие или категория |
| `:book123` | NamedIndividual entity | конкретный объект |
| `owl:Class` | vocabulary class | метакласс OWL |
| `owl:Thing` | vocabulary class | универсальная категория объектов |

И `:Book`, и `:book123` являются RDF-ресурсами и могут называться сущностями в широком RDF-смысле. Но в OWL они принадлежат разным синтаксическим категориям:

```text
:Book    — Class
:book123 — NamedIndividual
```

В OWL 2 DL это различие должно соблюдаться на уровне использования сущностей. Спецификация отдельно определяет ограничения типизации сущностей и разделяет классы, свойства, индивиды и дататипы. [w3](https://www.w3.org/TR/owl2-syntax/)

## 12. Почему это не обычная иерархия «объект → класс → метакласс»

Полезная учебная схема:

```text
:book123 rdf:type :Book .
:Book    rdf:type owl:Class .
```

Но она может создать ложное впечатление, будто:

```text
:book123 — экземпляр :Book
:Book    — экземпляр owl:Class
owl:Class — экземпляр ещё какого-то метакласса
```

и так далее до бесконечности.

В OWL это не требуется. `owl:Class` не обязан быть представлен как экземпляр отдельного пользовательского метакласса. Кроме того:

- OWL-класс интерпретируется множеством объектов;
- имя класса является сущностью языка;
- RDF-сериализация показывает декларацию класса как `rdf:type owl:Class`;
- метамоделирование в OWL допускается, но OWL 2 DL ограничивает смешение ролей.

То есть корректнее говорить не о бесконечной цепочке экземпляров, а о **разных ролях ресурса в разных слоях описания**.

## 13. Почему ваше ощущение путаницы возникает

Причина в том, что смешиваются три языка:

### Естественный язык

```text
экземпляр, объект, тип, категория, сущность
```

Термины многозначны.

### RDF

```text
rdf:type — общее отношение ресурса к классу
```

RDF не требует жёсткого разделения между «классами» и «индивидами».

### OWL 2 Direct Semantics

```text
Class — множество объектов
Individual — элемент object domain
ClassAssertion(C a) — a принадлежит C
```

Именно Direct Semantics снимает часть двусмысленности:

\[
a^I \in C^C
\]

Здесь `a` — индивидуальный объект, а `C` — класс, интерпретируемый множеством.

Запись:

```turtle
:Book rdf:type owl:Class .
```

в RDF-ориентированном представлении фиксирует, что `:Book` используется как класс OWL. Но это не следует читать так, будто `:Book` является обычной книгой или обычным объектом класса `owl:Class` в предметной области.

## 14. Практическая терминологическая схема

Для вашей документации я бы рекомендовал следующую систему:

```text
Индивид — конкретный объект предметной области.
Класс — категория, интерпретируемая множеством индивидов.
Тип индивида — класс, членом которого является индивид.
Принадлежность классу — отношение rdf:type / ClassAssertion.
Сущность OWL — именованный класс, property, individual или datatype.
Класс-выражение — возможно анонимное описание множества индивидов.
Метакласс — класс, экземплярами которого являются элементы, используемые как классы.
owl:Class — стандартный OWL-класс для декларации классов.
owl:Thing — универсальный класс всех объектов предметной области.
Объект RDF — третий член RDF-тройки; не обязательно OWL individual.
Категория — неформальный термин, обычно отображаемый на OWL class.
```

## 15. Рекомендуемая запись для примера `Book`

```turtle
@prefix :    <http://example.org/> .
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

# Термин онтологии: категория книг
:Book rdf:type owl:Class ;
      rdfs:subClassOf owl:Thing ;
      rdfs:label "Book"@en .

# Конкретный объект предметной области
:book123 rdf:type :Book .
```

Чтение:

```text
:Book — именованный OWL-класс,
          представляющий категорию книг.

:book123 — именованный индивид,
           являющийся членом категории :Book.

owl:Thing — универсальный класс объектов,
            к которому относится любой OWL-индивид.
```

Строго говоря, `rdfs:subClassOf owl:Thing` здесь избыточно: любой OWL-класс является подклассом `owl:Thing` по семантике OWL. Её можно оставить только как явную документационную запись.

## Вывод

Ваша критика справедлива на уровне терминологии: слово **«экземпляр»** действительно используется и для конкретного объекта, и для элемента метамодели. Чтобы избежать путаницы, лучше говорить:

- для `:book123 rdf:type :Book` — **индивид является членом класса `:Book`**;
- для `:Book rdf:type owl:Class` — **`:Book является OWL-классом** или **ресурс `:Book` декларирован как класс**;
- не называть оба случая без уточнения «экземплярами».

Формально же OWL различает их через роли:

\[
:book123^I \in :Book^C
\]

а `:Book rdf:type owl:Class` — это RDF-представление того, что `:Book` является сущностью-классом OWL, а не утверждение о том, что `:Book` является обычной вещью предметной области. [w3](https://www.w3.org/TR/owl2-primer/)


## 5

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

## 6

Может быть для моего концепта (разделение на мир категорий - owl:Class и вещей owl:Thing) подходит OWL DL? Там ведь они взаимоисключающие множества.э

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

https://www.perplexity.ai/search/78cb006c-319f-4501-8b8f-ec5dd22b7913

