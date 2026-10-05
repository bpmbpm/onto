link
- https://repository.publisso.de/resource/frl:6432267/data
- https://book.validatingrdf.com/bookHtml008.html		book Validating RDF data
- https://www.w3.org/TR/2013/WD-rdf11-primer-20131217/
- https://www.w3.org/TR/2013/WD-rdf11-primer-20131217/
- https://gist.github.com/jim-ecker/93a3b77eebdb88c770c8c31462cb68b8

## 1
Может быть лучше так:
«Алиса есть экземпляр типа Person»
вместо 
ex:alice a foaf:Person .
пишем:
ex:alice o2p:isInstance foaf:Person . 
При этом, мы говорим, что o2p:isInstance схож с rdf:type (сокращение a) тем-то, но отличие в том-то. 
Раз использован предикат o2p:isInstance, то мы можем утверждать, что сущность (обозначение) ex:alice принадлежит к Миру вещей.
Аналогично:
В мире идей есть иерархия классов, поэтому:
Мы можем выделить в foaf:Person подкласс студентов, через o2p:isSubClass 
Т.е. вместо 
ex:student rdf:type foaf:Person .
пишем:
ex:Student o2p:isSubClass foaf:Person .
При этом понятно по o2p:isSubClass, что ex:Student – объект из мира идей, т.е. категория. 
Давай, что из мира идей называть категориями (мир категория), а из мира вещей – экземплярами категорий. 

2
Исключи belongsToWorld, o2p:IdeaWorld, o2p:ThingWorld, o2p:Idea, o2p:Thing

3
Не понятно, почему o2p:hasProperty «отсутствует точный аналог».
Мы вначале говорим в мире Идей (категорий), что объект типа Person должен (может) иметь имя (предикат) и фамилию, например,
o2p:Person o2p:hasProperty o2p:hasName .
o2p:Person o2p:hasProperty o2p:hasSurName .
Далее
ex:alice o2p:hasName «Алисе» .  
ex:alice o2p:hasSurName «Петрова» . 

Покажи исправленную табличку Что заимствовать, а что переименовать. 

---

Да, это улучшение. В O2P следует заменить прежнее `o2p:isInst` на более естественное:

```turtle
o2p:isInstance
```

А для иерархии категорий использовать:

```turtle
o2p:isSubClass
```

При этом нужно исправить один важный момент: в примере

```turtle
ex:Student o2p:isSubClass foaf:Person .
```

`ex:Student` не должен быть конкретным студентом. Это IRI категории из пространства имён, предназначенного для категорий. Поэтому лучше использовать:

```turtle
o2p:Student o2p:isSubClass foaf:Person .
```

Конкретные люди остаются в `ex:`:

```turtle
ex:alice o2p:isInstance o2p:Student .
```

Или, если сохраняется готовая категория FOAF:

```turtle
ex:alice o2p:isInstance foaf:Person .
```

# 1. Согласованная терминология O2P

В O2P используются только два основных мира:

```text
мир категорий
мир экземпляров категорий
```

## Мир категорий

Мир категорий содержит:

```text
Person
Student
Book
Publication
```

Категория — это элемент, который используется для классификации экземпляров.

В RDF/OWL категория обычно обозначается IRI. В O2P категории должны находиться в пространствах имён онтологий:

```turtle
o2p:Person
o2p:Student
foaf:Person
schema:Person
```

## Мир экземпляров категорий

Мир экземпляров содержит конкретные сущности:

```turtle
ex:alice
ex:bob
ex:book123
```

Эти ресурсы находятся в пространстве примера `ex:`, а не в `o2p:`:

```turtle
@prefix ex: <https://example.org/> .
```

## Основные отношения

```turtle
ex:alice o2p:isInstance o2p:Person .
o2p:Student o2p:isSubClass o2p:Person .
```

Их смысл:

```text
ex:alice — экземпляр категории o2p:Person.
o2p:Student — подкатегория o2p:Person.
```

# 2. `o2p:isInstance` и `rdf:type`

Стандартная RDF-запись:

```turtle
ex:alice a foaf:Person .
```

эквивалентна:

```turtle
ex:alice rdf:type foaf:Person .
```

Символ `a` — это сокращение Turtle для `rdf:type`. В RDF `rdf:type` выражает отношение принадлежности ресурса классу. [w3](https://www.w3.org/TR/rdf-schema/diff.html)

В O2P вместо этого используется:

```turtle
ex:alice o2p:isInstance foaf:Person .
```

## Сходство

`o2p:isInstance` и `rdf:type` сходны в том, что оба выражают:

```text
экземпляр связан с категорией или классом
```

Сравнение:

| Свойство | `rdf:type` | `o2p:isInstance` |
|---|---|---|
| Стандарт | RDF/RDFS/OWL | O2P |
| Синтаксическое сокращение | `a` | Сокращения нет |
| Смысл | ресурс является экземпляром класса | экземпляр относится к категории |
| Субъект | RDF-ресурс | экземпляр категории |
| Объект | RDF-класс | категория |
| Ограничение миров | не задаётся явно | предполагается: субъект — экземпляр, объект — категория |
| Выражает двухмировую модель | Нет | Да, в модели O2P |
| Стандартный OWL reasoning | Да | Нет автоматически |
| Нужен O2P reasoner или правила | Нет | Да |

## Отличие

`rdf:type` универсален и не различает явно:

```text
экземпляр категории
категорию
метакласс
RDF-ресурс
```

В O2P:

```turtle
ex:alice o2p:isInstance foaf:Person .
```

сразу интерпретируется как утверждение определённого типа:

```text
субъект — экземпляр;
объект — категория.
```

Но это будет гарантировано только правилами O2P или валидатором O2P. Сам RDF не знает, что `o2p:isInstance` означает именно Thing–Category.

## Формальная сигнатура

```text
o2p:isInstance: Instance × Category → Truth
```

В обозначениях множеств:

```text
o2p:isInstance ⊆ ΔInstance × ΔCategory
```

или:

```text
x o2p:isInstance C
```

означает:

```text
x ∈ ΔInstance
C ∈ ΔCategory
x является экземпляром категории C
```

# 3. `o2p:isSubClass` и `rdfs:subClassOf`

Стандартная запись RDFS:

```turtle
foaf:Student rdfs:subClassOf foaf:Person .
```

В O2P:

```turtle
o2p:Student o2p:isSubClass foaf:Person .
```

Смысл:

```text
o2p:Student — подкатегория foaf:Person.
```

В RDF Schema `rdfs:subClassOf` связывает класс с его надклассом, а экземпляры подкласса рассматриваются также как экземпляры надкласса. Свойство является транзитивным в соответствующей семантике RDFS. [w3](https://www.w3.org/TR/rdf-schema/diff.html)

## Сходство

```turtle
o2p:Student o2p:isSubClass foaf:Person .
```

и:

```turtle
foaf:Student rdfs:subClassOf foaf:Person .
```

обе выражают иерархию категорий.

## Отличие

`rdfs:subClassOf` предполагает стандартную модель RDF Schema:

```text
субъект — RDF-класс;
объект — RDF-класс.
```

`o2p:isSubClass` в O2P означает:

```text
субъект — категория;
объект — категория.
```

Формальная сигнатура:

```text
o2p:isSubClass: Category × Category → Truth
```

или:

```text
o2p:isSubClass ⊆ ΔCategory × ΔCategory
```

# 4. Исправленная модель пространств имён

## 4.1. Категории

Категории могут заимствоваться из существующих онтологий:

```turtle
foaf:Person
schema:Person
schema:Book
```

или определяться в O2P:

```turtle
o2p:Student
o2p:Book
o2p:Publication
```

## 4.2. Конкретные экземпляры

Все конкретные индивиды находятся в `ex:`:

```turtle
ex:alice
ex:bob
ex:book123
```

Таким образом:

```text
o2p:Student — категория
ex:alice     — конкретный экземпляр категории
```

Не следует писать:

```turtle
ex:Student o2p:isSubClass foaf:Person .
```

если `ex:` используется только для экземпляров.

Правильно:

```turtle
o2p:Student o2p:isSubClass foaf:Person .
```

# 5. `o2p:hasProperty`

Да, `o2p:hasProperty` необходим для O2P и должен быть включён в ядро.

Предыдущая формулировка о том, что у него «отсутствует точный аналог», была неточной. Нужно было сказать:

> В стандартных RDF, RDFS и OWL нет полностью эквивалентного универсального предиката, который выражает именно требование O2P: категория задаёт или допускает наличие свойства у своих экземпляров.

## 5.1. Идея свойства

```turtle
o2p:Person o2p:hasProperty o2p:hasName .
o2p:Person o2p:hasProperty o2p:hasSurname .
```

Смысл:

```text
категория Person предусматривает свойство hasName;
категория Person предусматривает свойство hasSurname.
```

Оба ресурса относятся к миру категорий:

```text
o2p:Person — категория;
o2p:hasName — предикат или идея свойства;
o2p:hasSurname — предикат или идея свойства.
```

## 5.2. Значения свойств экземпляров

```turtle
ex:alice o2p:hasName "Алиса"@ru .
ex:alice o2p:hasSurname "Петрова"@ru .
```

Смысл:

```text
экземпляр ex:alice имеет значение свойства hasName;
экземпляр ex:alice имеет значение свойства hasSurname.
```

Вся модель:

```turtle
o2p:Person o2p:hasProperty o2p:hasName .
o2p:Person o2p:hasProperty o2p:hasSurname .

ex:alice o2p:isInstance o2p:Person ;
        o2p:hasName "Алиса"@ru ;
        o2p:hasSurname "Петрова"@ru .
```

## 5.3. Это не то же самое, что `rdfs:domain`

Можно использовать `rdfs:domain`:

```turtle
o2p:hasName rdfs:domain foaf:Person .
```

Но смысл будет другим:

```text
если ресурс используется как субъект свойства hasName,
то он относится к классу foaf:Person.
```

`rdfs:domain` не выражает непосредственно:

```text
Person задаёт свойство hasName
```

Поэтому в O2P нужны оба отношения:

```turtle
o2p:Person o2p:hasProperty o2p:hasName .
o2p:hasName rdfs:domain foaf:Person .
```

Их функции различны:

| Предикат | Смысл |
|---|---|
| `o2p:hasProperty` | категория предусматривает свойство |
| `rdfs:domain` | использование свойства указывает на принадлежность субъекта домену |
| `rdfs:range` | значение свойства принадлежит указанному классу |
| `owl:Restriction` | логическое ограничение количества, типа или значения |
| `sh:property` | проверяемое ограничение формы RDF-графа |

## 5.4. Обязательное и допустимое свойство

`o2p:hasProperty` само по себе лучше трактовать как:

```text
категория допускает или предусматривает свойство
```

Оно не должно автоматически означать обязательность.

Для различения можно ввести:

```turtle
o2p:hasProperty
o2p:hasRequiredProperty
o2p:hasOptionalProperty
```

Пример:

```turtle
o2p:Person o2p:hasRequiredProperty o2p:hasName .
o2p:Person o2p:hasOptionalProperty o2p:hasSurname .
```

Смысл:

```text
имя обязательно;
фамилия допускается, но не обязательна.
```

Если нужно выразить кардинальность:

```turtle
o2p:Person o2p:hasProperty o2p:hasName .
o2p:hasName o2p:minCardinality "1"^^xsd:nonNegativeInteger .
o2p:hasName o2p:maxCardinality "1"^^xsd:nonNegativeInteger .
```

Для проверки графа предпочтительнее SHACL:

```turtle
o2p:PersonShape
    sh:targetClass o2p:Person ;
    sh:property [
        sh:path o2p:hasName ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
    ] .
```

Но это уже требует отдельного SHACL-слоя.

# 6. Исправленная таблица заимствования

| Термин O2P | Возможный прототип | Что заимствуется | Что изменяется в O2P | Рекомендация |
|---|---|---|---|---|
| `o2p:isInstance` | `rdf:type` / `a` | Связь экземпляра с категорией | Явно закрепляется направление Instance → Category | Использовать в ядре O2P |
| `o2p:isSubClass` | `rdfs:subClassOf` | Иерархия классов | Обе стороны трактуются именно как категории O2P | Использовать в ядре O2P |
| `o2p:hasProperty` | частично `rdfs:domain`, OWL restrictions, SHACL property shape | Связь категории со свойством | Выражает, что категория предусматривает свойство | Использовать в ядре O2P |
| `o2p:hasRequiredProperty` | SHACL `sh:minCount`, OWL restriction | Обязательное свойство | Отдельно выражает обязательность | Использовать при необходимости |
| `o2p:hasOptionalProperty` | Прямого аналога нет | Допустимое свойство | Явно фиксирует необязательность | Собственный термин O2P |
| `o2p:hasName` | `foaf:name`, `schema:name` | Имя | Можно использовать совместно с O2P | Предпочтительно заимствовать |
| `o2p:hasSurname` | `foaf:familyName` | Фамилия | `foaf:familyName` предпочтительнее собственного имени | Заимствовать `foaf:familyName` |
| `o2p:hasAuthor` | `schema:author`, `dcterms:creator` | Авторство | В O2P субъектом является экземпляр | Заимствовать `schema:author` или создать адаптер |
| `o2p:hasColor` | `schema:color` | Цвет | Нужно различать категорию свойства и значение | Заимствовать `schema:color` для внешнего слоя |
| `o2p:hasColorValue` | `schema:color` | Значение цвета | Явно указывает на литеральное значение | Использовать, если нужен O2P-специфичный термин |
| `o2p:Person` | `foaf:Person`, `schema:Person` | Категория человека | Идея/категория O2P | Использовать внешний термин или адаптер |
| `o2p:Book` | `schema:Book` | Категория книги | Идея/категория O2P | Заимствовать `schema:Book` при совместимости |
| `o2p:Idea` | `owl:Class` | Никакой полной эквивалентности | O2P-метатермин, не стандартный OWL-класс | Не вводить в ядро согласно вашему решению |
| `o2p:Thing` | `owl:Thing` | Никакой полной эквивалентности | O2P-метатермин, исключён из ядра | Не вводить в ядро |
| `o2p:IdeaWorld` | Нет точного прототипа | Нет | Исключено по вашему решению | Не использовать |
| `o2p:ThingWorld` | Нет точного прототипа | Нет | Исключено по вашему решению | Не использовать |
| `o2p:belongsToWorld` | Нет точного прототипа | Нет | Исключено по вашему решению | Не использовать |
| `o2p:hasMethod` | Нет стандартного аналога в OWL/RDFS | Нет | Отдельное доменное отношение | Использовать только если методы нужны |
| `o2p:hasValue` | Частично `schema:value` | Связь со значением | В O2P нужно различать литерал и экземпляр | Использовать осторожно |

Главные исправления:

```text
o2p:hasProperty не исключается;
o2p:Idea, o2p:Thing, o2p:IdeaWorld, o2p:ThingWorld исключаются;
o2p:isInstance становится основным отношением экземпляра;
o2p:isSubClass становится отношением категорий;
конкретные экземпляры находятся только в ex:;
категории находятся в o2p: или внешних онтологиях.
```

# 7. Исправленная базовая онтология O2P

Ниже — ядро без:

```turtle
o2p:Idea
o2p:Thing
o2p:IdeaWorld
o2p:ThingWorld
o2p:belongsToWorld
```

```turtle
@prefix o2p: <https://github.com/bpmbpm/ontology/o2p#> .
@prefix ex:  <https://example.org/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:<http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix schema: <https://schema.org/> .

#################################################################
# O2P predicates
#################################################################

o2p:isInstance
    rdfs:label "является экземпляром категории"@ru ;
    rdfs:comment
        "Связывает конкретный экземпляр с категорией."@ru .

o2p:isSubClass
    rdfs:label "является подклассом категории"@ru ;
    rdfs:comment
        "Связывает подкатегорию с надкатегорией."@ru .

o2p:hasProperty
    rdfs:label "категория предусматривает свойство"@ru ;
    rdfs:comment
        "Связывает категорию со свойством, допустимым для её экземпляров."@ru .

o2p:hasRequiredProperty
    rdfs:label "категория требует свойство"@ru ;
    rdfs:comment
        "Связывает категорию с обязательным свойством."@ru .

o2p:hasOptionalProperty
    rdfs:label "категория допускает свойство"@ru ;
    rdfs:comment
        "Связывает категорию с необязательным свойством."@ru .

#################################################################
# O2P category terms
#################################################################

o2p:Student
    rdfs:label "Студент"@ru .

o2p:Book
    rdfs:label "Книга"@ru .

o2p:Publication
    rdfs:label "Публикация"@ru .

#################################################################
# Category hierarchy
#################################################################

o2p:Student
    o2p:isSubClass foaf:Person .

o2p:Book
    o2p:isSubClass o2p:Publication .

#################################################################
# Category properties
#################################################################

o2p:Person
    o2p:hasProperty foaf:name ;
    o2p:hasProperty foaf:familyName .

o2p:Book
    o2p:hasProperty schema:color ;
    o2p:hasProperty schema:author .

#################################################################
# Optional external domain/range metadata
#################################################################

foaf:name
    rdfs:domain foaf:Person .

foaf:familyName
    rdfs:domain foaf:Person .

schema:author
    rdfs:domain schema:Book ;
    rdfs:range foaf:Person .
```

В этом фрагменте:

```text
o2p:Student
o2p:Book
o2p:Publication
```

являются категориями по контексту использования `o2p:isSubClass` и `o2p:hasProperty`.

Конкретные экземпляры не определяются в пространстве `o2p:`.

# 8. Пример с Алисой и Бобом

```turtle
@prefix o2p: <https://github.com/bpmbpm/ontology/o2p#> .
@prefix ex:  <https://example.org/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix schema: <https://schema.org/> .

#################################################################
# Category hierarchy
#################################################################

o2p:Student
    o2p:isSubClass foaf:Person .

o2p:Book
    o2p:isSubClass schema:CreativeWork .

#################################################################
# Category property declarations
#################################################################

foaf:Person
    o2p:hasProperty foaf:name ;
    o2p:hasProperty foaf:familyName .

schema:Book
    o2p:hasProperty schema:author ;
    o2p:hasProperty schema:color .

#################################################################
# Instances from ex:
#################################################################

ex:alice
    o2p:isInstance o2p:Student ;
    foaf:name "Алиса"@ru ;
    foaf:familyName "Петрова"@ru ;
    foaf:knows ex:bob .

ex:bob
    o2p:isInstance foaf:Person ;
    foaf:name "Боб"@ru ;
    foaf:familyName "Смит"@ru ;
    foaf:knows ex:alice .

ex:book123
    o2p:isInstance schema:Book ;
    schema:author ex:alice ;
    schema:color "red"@en .
```

Смысл:

```text
o2p:Student — категория.
foaf:Person — категория.
schema:Book — категория.

ex:alice — экземпляр категории o2p:Student.
ex:bob — экземпляр категории foaf:Person.
ex:book123 — экземпляр категории schema:Book.

o2p:Student — подкатегория foaf:Person.
foaf:Person предусматривает свойства foaf:name и foaf:familyName.
ex:alice имеет имя «Алиса» и фамилию «Петрова».
ex:book123 имеет автора ex:alice и цвет «red».
```

# 9. Почему `ex:alice` принадлежит миру экземпляров

В новой модели это определяется не `belongsToWorld`, а ролью субъекта в отношении:

```turtle
ex:alice o2p:isInstance o2p:Student .
```

Если ресурс является субъектом `o2p:isInstance`, он используется как экземпляр категории.

В O2P действует правило:

```text
x o2p:isInstance C
→ x принадлежит миру экземпляров
→ C принадлежит миру категорий
```

Формально:

```text
o2p:isInstance ⊆ ΔInstance × ΔCategory
```

Поэтому из:

```turtle
ex:alice o2p:isInstance o2p:Student .
```

следует по O2P-семантике:

```text
ex:alice ∈ ΔInstance
o2p:Student ∈ ΔCategory
```

А из:

```turtle
o2p:Student o2p:isSubClass foaf:Person .
```

следует:

```text
o2p:Student ∈ ΔCategory
foaf:Person ∈ ΔCategory
```

Это и есть требуемая типизация без `rdf:type`.

# 10. Как `o2p:hasProperty` работает вместе с экземпляром

Имеем:

```turtle
foaf:Person
    o2p:hasProperty foaf:name ;
    o2p:hasProperty foaf:familyName .
```

И:

```turtle
ex:alice
    o2p:isInstance foaf:Person ;
    foaf:name "Алиса"@ru ;
    foaf:familyName "Петрова"@ru .
```

Смысл:

```text
foaf:Person — категория людей.
foaf:name — свойство, предусмотренное категорией Person.
foaf:familyName — свойство, предусмотренное категорией Person.

ex:alice — экземпляр Person.
ex:alice имеет значение свойства foaf:name.
ex:alice имеет значение свойства foaf:familyName.
```

Можно выразить правило:

```text
x o2p:isInstance C
∧ C o2p:hasProperty P
→ x может иметь свойство P
```

В символической форме Unicode:

```text
x o2p:isInstance C ∧ C o2p:hasProperty P
→ x может иметь P
```

Для обязательности нужна отдельная связь:

```turtle
foaf:Person
    o2p:hasRequiredProperty foaf:name ;
    o2p:hasOptionalProperty foaf:familyName .
```

Тогда:

```text
x o2p:isInstance foaf:Person
→ x должен иметь foaf:name
```

а наличие `foaf:familyName` допускается, но не обязательно.

# 11. Обновлённая таблица типов триплетов

| Тип триплета | Субъект | Предикат | Объект | Пример |
|---|---|---|---|---|
| Category–Category | Категория | Предикат категорий | Категория | `o2p:Student o2p:isSubClass foaf:Person` |
| Category–Property | Категория | Предикат описания | Свойство | `foaf:Person o2p:hasProperty foaf:name` |
| Instance–Category | Экземпляр | `o2p:isInstance` | Категория | `ex:alice o2p:isInstance o2p:Student` |
| Instance–Instance | Экземпляр | Предикат вещей | Экземпляр | `ex:alice foaf:knows ex:bob` |
| Instance–Literal | Экземпляр | Предикат данных | Литерал | `ex:alice foaf:name "Алиса"` |
| Instance–Instance | Экземпляр | Предикат объекта | Экземпляр | `ex:book123 schema:author ex:alice` |
| Category–Literal | Категория | Предикат аннотации | Литерал | `foaf:Person rdfs:label "Person"` |

## Исправленная классификация

```text
Category × Category
Category × Property
Instance × Category
Instance × Instance
Instance × Literal
Category × Literal
```

При этом предикаты являются терминами словаря O2P или заимствованных онтологий.

# 12. Обновлённая таблица заимствования

| O2P-предикат или термин | Прототип | Решение |
|---|---|---|
| `o2p:isInstance` | `rdf:type`, `a` | Использовать собственный предикат O2P для Instance–Category |
| `o2p:isSubClass` | `rdfs:subClassOf` | Использовать собственный предикат O2P для Category–Category |
| `o2p:hasProperty` | Прямого полного аналога нет; частично `rdfs:domain`, OWL restrictions, SHACL shapes | Использовать в O2P для объявления свойства категории |
| `o2p:hasRequiredProperty` | SHACL `sh:minCount`, OWL cardinality restriction | Использовать для обязательности |
| `o2p:hasOptionalProperty` | Прямого стандартного аналога нет | Использовать для необязательных свойств |
| `o2p:Student` | Нет обязательного прототипа | Собственная O2P-категория |
| `foaf:Person` | FOAF | Заимствовать как категорию человека |
| `schema:Book` | Schema.org | Заимствовать как категорию книги |
| `foaf:name` | FOAF | Заимствовать для имени |
| `foaf:familyName` | FOAF | Заимствовать для фамилии |
| `schema:author` | Schema.org | Заимствовать для авторства |
| `schema:color` | Schema.org | Заимствовать для цвета |
| `o2p:hasColorValue` | Частично `schema:color` | Не вводить без необходимости; использовать `schema:color` для обычного значения |
| `o2p:belongsToWorld` | Собственный предыдущий термин | Исключить |
| `o2p:IdeaWorld` | Собственный предыдущий термин | Исключить |
| `o2p:ThingWorld` | Собственный предыдущий термин | Исключить |
| `o2p:Idea` | Аналог `owl:Class` | Исключить из ядра |
| `o2p:Thing` | Аналог `owl:Thing` | Исключить из ядра |

# 13. Важное различие между `o2p:hasProperty` и обычным свойством

В выражении:

```turtle
foaf:Person o2p:hasProperty foaf:name .
```

`foaf:name` выступает как объект утверждения о категории.

В выражении:

```turtle
ex:alice foaf:name "Алиса"@ru .
```

`foaf:name` выступает как предикат утверждения о конкретном экземпляре.

Один и тот же IRI `foaf:name` имеет две роли:

```text
1. имя свойства как объекта метаутверждения;
2. предикат фактического утверждения об экземпляре.
```

Это допустимо в RDF и удобно для O2P, но семантически роли следует различать:

```text
foaf:Person o2p:hasProperty foaf:name
```

означает:

```text
категория Person допускает свойство name
```

а:

```text
ex:alice foaf:name "Алиса"
```

означает:

```text
конкретный экземпляр Alice имеет значение name
```

Это не две разные сущности `foaf:name`, а два способа использования одного термина.

# 14. Рекомендуемая базовая схема O2P

```turtle
@prefix o2p: <https://github.com/bpmbpm/ontology/o2p#> .
@prefix ex:  <https://example.org/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix schema: <https://schema.org/> .

# Иерархия категорий

o2p:Student
    o2p:isSubClass foaf:Person .

o2p:Book
    o2p:isSubClass schema:CreativeWork .

# Свойства категорий

foaf:Person
    o2p:hasProperty foaf:name ;
    o2p:hasProperty foaf:familyName .

schema:Book
    o2p:hasProperty schema:author ;
    o2p:hasProperty schema:color .

# Экземпляры категорий

ex:alice
    o2p:isInstance o2p:Student ;
    foaf:name "Алиса"@ru ;
    foaf:familyName "Петрова"@ru ;
    foaf:knows ex:bob .

ex:bob
    o2p:isInstance foaf:Person ;
    foaf:name "Боб"@ru ;
    foaf:familyName "Смит"@ru ;
    foaf:knows ex:alice .

ex:book123
    o2p:isInstance schema:Book ;
    schema:author ex:alice ;
    schema:color "red"@en .
```

# Итог

В обновлённой версии O2P:

```text
o2p:isInstance
```

заменяет:

```text
rdf:type
```

и выражает:

```text
экземпляр принадлежит категории
```

```text
o2p:isSubClass
```

заменяет:

```text
rdfs:subClassOf
```

и выражает:

```text
категория является подкатегорией другой категории
```

```text
o2p:hasProperty
```

выражает:

```text
категория предусматривает или допускает свойство
```

А конкретные утверждения записываются обычными предикатами:

```turtle
ex:alice foaf:name "Алиса"@ru .
ex:alice foaf:familyName "Петрова"@ru .
```

При этом:

```text
o2p:Student
o2p:Book
foaf:Person
schema:Book
```

используются как категории, а:

```text
ex:alice
ex:bob
ex:book123
```

— как конкретные экземпляры этих категорий.
