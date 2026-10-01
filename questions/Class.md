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
