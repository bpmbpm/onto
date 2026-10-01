## 1
Прокомментируй фрагмент
```
onto:Person rdf2:type owl:Class .
onto:Person rdfs2:label "Person"@en .
onto:Person rdfs2:label "Персона"@ru .
onto:Person rdfs2:comment "Человек или вымышленный персонаж."@ru .
onto:Person owl:equivalentClass <http://schema.org/Person> .
```

## Разбор фрагмента

В фрагменте используются нестандартные префиксы `rdf2:` и `rdfs2:` — это просто другие имена для тех же пространств имён. Если развернуть их к стандартным URI, получится:

```turtle
@prefix rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix onto: <http://example.org/onto#> .

onto:Person rdf:type   owl:Class .
onto:Person rdfs:label  "Person"@en .
onto:Person rdfs:label  "Персона"@ru .
onto:Person rdfs:comment "Человек или вымышленный персонаж."@ru .
onto:Person owl:equivalentClass <http://schema.org/Person> .
```

---

## owl:Class vs rdfs:Class

Вместо `owl:Class` можно написать:

```turtle
onto:Person rdf:type rdfs:Class .
```

Или в сокращённой форме:

```turtle
onto:Person a rdfs:Class .
```

(`a` — это общепринятая сокращённая запись для `rdf:type`.)

### В чём отличие

| | `rdfs:Class` | `owl:Class` |
|---|---|---|
| **Уровень** | RDFS — базовая модель | OWL — надстройка над RDFS |
| **Иерархия** | `owl:Class` — это подкласс `rdfs:Class` | Наследует всё от RDFS |
| **Что поддерживает** | `rdfs:subClassOf`, `rdfs:subPropertyOf`, домены/ранги | Всё из RDFS **плюс**: ограничения (`owl:Restriction`), `owl:unionOf` / `owl:intersectionOf` / `owl:complementOf`, `owl:disjointWith`, `owl:equivalentClass`, кардинальности и т. д. |
| **Выведение фактов** | Простое: если X — подкласс Y, то экземпляры X — экземпляры Y | Богатое: логический вывод на основе OWL-семантики (DL-рассуждители: Pellet, HermiT и др.) |

**Коротко:** `owl:Class` — это уточнённая версия `rdfs:Class`. Если вы используете `owl:equivalentClass`, `owl:Restriction` и подобные конструкции, класс **должен** быть объявлен как `owl:Class`, иначе OWL-рассуждитель может не учесть эти свойства. Если нужна только простая иерархия подклассов с метками — `rdfs:Class` достаточно.

---

## Множественные значения `rdfs:label`

Да, `rdfs:label` в RDF — свойство без ограничения кардинальности, поэтому у одной сущности может быть сколько угодно значений. В вашем примере их два: на английском и на русском. Это абсолютно нормальная практика.

### Примеры обращения

**SPARQL — получить все метки:**

```sparql
PREFIX onto: <http://example.org/onto#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?label ?lang WHERE {
  onto:Person rdfs:label ?label .
  BIND(lang(?label) AS ?lang)
}
```

Результат:
| label | lang |
|---|---|
| Person | en |
| Персона | ru |

**SPARQL — получить только русскую метку:**

```sparql
SELECT ?label WHERE {
  onto:Person rdfs:label ?label .
  FILTER(lang(?label) = "ru")
}
```

**SPARQL — получить метку по языку с запасным вариантом (fallback):**

```sparql
SELECT ?label WHERE {
  onto:Person rdfs:label ?label .
  FILTER(lang(?label) = "ru" || lang(?label) = "en")
}
LIMIT 1
}
```

**В Python (rdflib):**

```python
from rdflib import Graph, URIRef
from rdflib.namespace import RDFS

g = Graph()
g.parse("ontology.ttl", format="turtle")

person = URIRef("http://example.org/onto#Person")

# Все метки
labels = list(g.objects(person, RDFS.label))
for l in labels:
    print(f"{l} ({l.language})")

# Только русская
ru_label = [l for l in labels if l.language == "ru"]
print(ru_label[0] if ru_label else "Нет русской метки")
```

---

### На что обратить внимание

- Языковые теги (`@en`, `@ru`) — это часть RDF-литералов (rdf:PlainLiteral / `xsd:string` с языком). Они **не** создают разных свойств — это одно и то же свойство `rdfs:label` с разными значениями.
- Если нужно явно указать, что меток может быть несколько, а не одна, — в OWL это можно задать через `owl:AnnotationProperty` с нужной кардинальностью, но на практике так не делают: `rdfs:label` по негласному соглашению мультизначен, и инструменты (включая rdflib, Apache Jena, Virtuoso) это нормально поддерживают.

## 1.1

обработка @en и @ru должна быть такой же как в rdfs - Что нужно добавить в описание rdfs2

## Префикс vs пространство имён: ключевой момент

Прежде всего нужно понять, что именно вы имеете в виду под «собственная `rdfs2`». Возможны два варианта, и ответ зависит от того, какой из них ваш.

---

### Вариант 1: `rdfs2` — просто другой префикс для того же URI

Если в начале онтологии написано:

```turtle
@prefix rdfs2: <http://www.w3.org/2000/01/rdf-schema#> .
```

то `rdfs2:label` и `rdfs:label` — это **одно и то же свойство** с одним и тем же URI. Никаких дополнительных объявлений не нужно: обработка языковых тегов, семантика, вывод — всё работает автоматически, потому что RDF работает с URI, а не с префиксами.

```turtle
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .
@prefix rdfs2: <http://www.w3.org/2000/01/rdf-schema#> .  # тот же URI!
@prefix onto:  <http://example.org/onto#> .

onto:Person rdfs2:label "Person"@en .
onto:Person rdfs:label  "Персона"@ru .

# Эти две записи абсолютно идентичны:
# rdfs2:label = rdfs:label = <http://www.w3.org/2000/01/rdf-schema#label>
```

Здесь `rdfs2` — это просто псевдоним для удобства, семантически ничего не меняется.

---

### Вариант 2: `rdfs2` — собственное пространство имён

Если `rdfs2` указывает на другой URI:

```turtle
@prefix rdfs2: <http://example.org/my-ns#> .
```

тогда `rdfs2:label` — это **другое свойство**, не имеющее отношения к `rdfs:label`. Здесь уже нужно явно объявить связь.

#### Что добавить в описание

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:   <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl:    <http://www.w3.org/2002/07/owl#> .
@prefix rdfs2:  <http://example.org/my-ns#> .
@prefix onto:   <http://example.org/onto#> .

# --- Объявление собственного пространства имён ---

rdfs2:label a owl:AnnotationProperty ;
    rdfs:label "label" ;
    rdfs:comment "Собственное свойство метки, эквивалентное rdfs:label." .

# Эквивалентность со стандартным rdfs:label
rdfs2:label owl:equivalentProperty rdfs:label .

# Дополнительно: rdfs2:label как подмножество rdfs:label
# (более слабое утверждение, но некоторые инструменты его лучше понимают)
rdfs2:label rdfs:subPropertyOf rdfs:label .
```

#### Использование в онтологии

```turtle
onto:Person rdf:type owl:Class ;
    rdfs2:label "Person"@en ;
    rdfs2:label "Персона"@ru ;
    rdfs2:comment "Человек или вымышленный персонаж."@ru ;
    owl:equivalentClass <http://schema.org/Person> .
```

---

### Важный момент про языковые теги

Языковые теги (`@en`, `@ru`) — это **встроенная возможность RDF-литералов**, а не свойство конкретного свойства. Любое RDF-свойство может принимать значения с языковыми тегами, независимо от того, объявлено оно или нет.

```turtle
# Это корректный RDF с языковым тегом для любого свойства:
onto:myCustomProp "Привет"@ru .
```

Так что «обработка `@en` и `@ru`» работает одинаково для любого свойства — это часть формата данных, а не часть семантики `rdfs:label`. Разница лишь в том, что инструменты (вотинки, редакторы, SPARQL-движки) «знают», что `rdfs:label` — это человеко-читаемое имя, и отображают его в интерфейсах. Если вы используете `rdfs2:label` с другим URI, нужно помочь им понять это через `owl:equivalentProperty`.

---

### Что дают разные объявления

| Объявление | Что говорит инструменту | Поддержка рассуждителями |
|---|---|---|
| `owl:equivalentProperty` | Свойства семантически эквивалентны: любое значение одного — значение другого | Полная (OWL DL) |
| `rdfs:subPropertyOf` | Значения `rdfs2:label` являются значениями `rdfs:label`, но не наоборот | Частичная (RDFS) |
| Оба сразу | Максимальная совместимость | Лучшая |

На практике я бы рекомендовала указать **оба** — `owl:equivalentProperty` для OWL-рассуждителей и `rdfs:subPropertyOf` для инструментов, которые понимают только RDFS. Это дёшево и покрывает больше случаев.

---

### Пример в SPARQL после объявления

Если `rdfs2:label` объявлен как `owl:equivalentProperty` для `rdfs:label`, то OWL-aware SPARQL-движок (например, с включённым reasoning) вернёт результаты по обоим:

```sparql
PREFIX rdfs:  <http://www.w3.org/2000/01/rdf-schema#>
PREFIX onto:  <http://example.org/onto#>

SELECT ?label WHERE {
  onto:Person rdfs:label ?label .
  FILTER(lang(?label) = "ru")
}
```

найдёт `"Персона"@ru`, даже если оно записано через `rdfs2:label`, — потому что рассуждитель выведет `rdfs:label` из `owl:equivalentProperty`.

Без reasoning (простой SPARQL без вывода) запрос найдёт только значения, явно записанные через `rdfs:label`. В этом случае нужно писать через `rdfs2:label` или использовать `UNION`:

```sparql
PREFIX rdfs2: <http://example.org/my-ns#>
PREFIX onto:  <http://example.org/onto#>

SELECT ?label WHERE {
  { onto:Person rdfs2:label ?label . }
  FILTER(lang(?label) = "ru")
}
```

## 1.2

В rdfs есть аналог owl:equivalentClass?

В RDFS нет прямого аналога `owl:equivalentClass`, так как RDFS — это более базовая модель, которая не поддерживает такие сложные отношения между классами. Однако, есть несколько способов выразить эквивалентность классов в RDFS:

1. **Использование rdfs:subClassOf и rdfs:superClassOf:**
   Если два класса эквивалентны, вы можете утверждать, что каждый из них является подклассом другого.

   ```turtle
   @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
   @prefix onto: <http://example.org/onto#> .

   onto:Person rdfs:subClassOf onto:SchemaPerson .
   onto:SchemaPerson rdfs:subClassOf onto:Person .
   ```

   Этот подход показывает, что оба класса содержат одни и те же экземпляры.

2. **Использование rdfs:label и rdfs:comment:**
   Хотя это не эквивалентность классов, вы можете использовать метки для обозначения того, что два класса имеют одинаковое значение.

   ```turtle
   onto:Person rdfs:label "Person"@en .
   onto:Person rdfs:label "Персона"@ru .
   
   onto:SchemaPerson rdfs:label "Person"@en .
   onto:SchemaPerson rdfs:label "Персона"@ru .
   ```

3. **Использование rdfs:seeAlso:**
   Это свойство можно использовать для связи двух классов, которые считаются эквивалентными.

   ```turtle
   onto:Person rdfs:seeAlso <http://schema.org/Person> .
   <http://schema.org/Person> rdfs:seeAlso onto:Person .
   ```

4. **Использование rdf:type для связи с другим пространством имён:**
   Если вы хотите явно указать, что ваш класс эквивалентен классу из другого пространства имён, вы можете использовать `rdf:type`.

   ```turtle
   onto:Person rdf:type <http://schema.org/Person> .
   ```

### Пример использования SPARQL для проверки эквивалентности

Если вы хотите проверить, что два класса считаются эквивалентными в вашей онтологии, вы можете использовать SPARQL:

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX onto: <http://example.org/onto#>

SELECT ?class1 ?class2 WHERE {
  ?class1 rdfs:subClassOf ?class2 .
  ?class2 rdfs:subClassOf ?class1 .
  FILTER (?class1 = onto:Person && ?class2 = <http://schema.org/Person>)
}
```

Этот запрос найдёт все пары классов, которые считаются эквивалентными через отношение `rdfs:subClassOf`.

Таким образом, хотя прямого аналога `owl:equivalentClass` в RDFS нет, вы можете использовать различные методы для выражения эквивалентности классов в вашей онтологии.

## 1.3

Как записать через rdfs  
onto:Person owl:equivalentClass <http://schema.org/Person> .

и что предикат label в rdfs2 (собственная онтология) эквивалент label в rdfs?

## Классы: эквивалентность через RDFS

В RDFS нет `owl:equivalentClass`, но эквивалентность двух классов выражается **взаимным `rdfs:subClassOf`** — каждый класс объявляется подклассом другого. Это даёт тот же логический результат: множества экземпляров совпадают.

### Исходная OWL-запись

```turtle
@prefix owl:  <http://www.w3.org/2002/07/owl#> .
@prefix onto: <http://example.org/onto#> .

onto:Person owl:equivalentClass <http://schema.org/Person> .
```

### Эквивалент в RDFS

```turtle
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix onto: <http://example.org/onto#> .

onto:Person                rdfs:subClassOf <http://schema.org/Person> .
<http://schema.org/Person> rdfs:subClassOf onto:Person .
```

**Почему это работает.** В RDFS-семантике правило вывода для `rdfs:subClassOf` гласит: если `A rdfs:subClassOf B` и `x rdf:type A`, то `x rdf:type B`. Когда обе стрелки направлены в обе стороны, экземпляры одного класса автоматически становятся экземплярами другого — ровно то, что делает `owl:equivalentClass`.

---

## Свойства: эквивалентность через RDFS

Аналогично, в RDFS нет `owl:equivalentProperty`, но есть `rdfs:subPropertyOf`. Взаимное подчинение даёт эквивалентность.

### Исходная OWL-запись

```turtle
@prefix owl:   <http://www.w3.org/2002/07/owl#> .
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .
@prefix rdfs2: <http://example.org/my-ns#> .

rdfs2:label owl:equivalentProperty rdfs:label .
```

### Эквивалент в RDFS

```turtle
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .
@prefix rdf:   <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs2: <http://example.org/my-ns#> .

# Объявляем rdfs2:label как свойство
rdfs2:label rdf:type rdf:Property ;
    rdfs:label "label" ;
    rdfs:comment "Собственное свойство метки, эквивалентное rdfs:label." .

# Взаимное подчинение = эквивалентность
rdfs2:label rdfs:subPropertyOf rdfs:label .
rdfs:label  rdfs:subPropertyOf rdfs2:label .
```

**Почему это работает.** Правило RDFS для `rdfs:subPropertyOf`: если `P rdfs:subPropertyOf Q` и `x P y`, то `x Q y`. Двустороннее подчинение гарантирует, что любое значение `rdfs2:label` становится значением `rdfs:label`, и наоборот.

---

## Полный пример онтологии на чистом RDFS

Объединим оба случая — классы и свойства — в одной онтологии без единого OWL-конструктора:

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:   <http://www.w3.org/2000/01/rdf-schema#> .
@prefix rdfs2:  <http://example.org/my-ns#> .
@prefix onto:   <http://example.org/onto#> .

# --- Пространство имён rdfs2 ---

rdfs2:label rdf:type rdf:Property ;
    rdfs:label "label" ;
    rdfs:comment "Метка, эквивалентная rdfs:label." .

rdfs2:comment rdf:type rdf:Property ;
    rdfs:label "comment" ;
    rdfs:comment "Комментарий, эквивалентный rdfs:comment." .

# Эквивалентность свойств через взаимный subPropertyOf
rdfs2:label   rdfs:subPropertyOf rdfs:label .
rdfs:label    rdfs:subPropertyOf rdfs2:label .

rdfs2:comment rdfs:subPropertyOf rdfs:comment .
rdfs:comment  rdfs:subPropertyOf rdfs2:comment .

# --- Класс Person ---

onto:Person rdf:type rdfs:Class ;
    rdfs2:label   "Person"@en ;
    rdfs2:label   "Персона"@ru ;
    rdfs2:comment "Человек или вымышленный персонаж."@ru .

# Эквивалентность классов через взаимный subClassOf
onto:Person                rdfs:subClassOf <http://schema.org/Person> .
<http://schema.org/Person> rdfs:subClassOf onto:Person .

# --- Экземпляр ---

onto:Alice rdf:type onto:Person ;
    rdfs2:label "Алиса"@ru .
```

---

## Что автоматически выведет RDFS-рассуждитель

Из этой онтологии RDFS-движок (например, встроенный в Apache Jena `RDFSRuleReasoner`) выведет следующие неявные факты:

| Правило | Вывод |
|---|---|
| `rdfs2:label` → `rdfs:label` (через subPropertyOf) | `onto:Person rdfs:label "Person"@en`, `"Персона"@ru` |
| `rdfs:label` → `rdfs2:label` (обратное подчинение) | и наоборот, любое `rdfs:label` станет `rdfs2:label` |
| `onto:Person` → `schema:Person` (через subClassOf) | `onto:Alice rdf:type <http://schema.org/Person>` |
| `schema:Person` → `onto:Person` (обратное подчинение) | любой экземпляр schema:Person станет экземпляром onto:Person |
| `rdfs2:comment` → `rdfs:comment` | `onto:Person rdfs:comment "Человек или вымышленный персонаж."@ru` |

### Проверка через SPARQL

```sparql
PREFIX rdfs:  <http://www.w3.org/2000/01/rdf-schema#>
PREFIX onto:  <http://example.org/onto#>

# Найдёт "Персона"@ru, хотя записано через rdfs2:label
SELECT ?label WHERE {
  onto:Person rdfs:label ?label .
  FILTER(lang(?label) = "ru")
}
```

```sparql
PREFIX schema: <http://schema.org/>
PREFIX onto:   <http://example.org/onto#>

# Найдёт onto:Alice как экземпляр schema:Person
SELECT ?person WHERE {
  ?person a schema:Person .
}
```

Оба запроса вернут результаты только при включённом RDFS-reasoning. Без него поиск ведётся строго по явно записанным триплетам.

---

## Сводка: OWL → RDFS

| OWL-конструкция | RDFS-эквивалент | Логика |
|---|---|---|
| `A owl:equivalentClass B` | `A rdfs:subClassOf B` + `B rdfs:subClassOf A` | Взаимное включение экземпляров |
| `P owl:equivalentProperty Q` | `P rdfs:subPropertyOf Q` + `Q rdfs:subPropertyOf P` | Взаимное наследование значений |
| `A owl:Class` | `A rdfs:Class` | OWL-класс — это подкласс RDFS-класса, но для базовой иерархии `rdfs:Class` достаточно |

### Ограничения RDFS по сравнению с OWL

- Нет `owl:disjointWith` (нельзя сказать, что классы не пересекаются)
- Нет `owl:Restriction` (нельзя задать кардинальность, `allValuesFrom`, `someValuesFrom`)
- Нет `owl:unionOf` / `owl:intersectionOf` / `owl:complementOf` (нельзя строить классы из других классов логическими операциями)
- Нет `owl:InverseObjectProperty` (нельзя объявить обратное свойство)
- Нет `owl:TransitiveProperty`, `owl:SymmetricProperty`, `owl:FunctionalProperty` (хотя `rdfs:subPropertyOf` частично компенсирует)

Если онтология использует только иерархию классов, метки, комментарии и эквивалентности — чистого RDFS достаточно. Если нужны ограничения, кардинальности, непересекающиеся классы — придётся переходить на OWL.
