Причина в том, что GitHub не использует `\[ ... \]` как надёжный разделитель математического блока. Для GitHub нужно использовать `$$ ... $$` для блочной формулы или `$ ... $` для формулы внутри строки. [docs.github](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)

## Исправьте так

### Было

```markdown
а так как:

[ \mathrm{owl{:}Class}^{C} \subseteq \Delta_I ]

это приведёт к:

[ \mathrm{owl{:}Class}^{C} = \varnothing ]
```

### Должно быть

```markdown
а так как:

$$
\mathrm{owl{:}Class}^{C} \subseteq \Delta_I
$$

это приведёт к:

$$
\mathrm{owl{:}Class}^{C} = \varnothing
$$
```

## Более надёжный вариант

Чтобы избежать проблем с форматированием имени `owl:Class`, можно обозначить его просто буквой \(K\), а расшифровку дать текстом:

```markdown
Обозначим интерпретацию класса `owl:Class` через \(K^C\).

Тогда:

$$
K^C \subseteq \Delta_I
$$

Поскольку `owl:Thing` интерпретируется как вся область объектов:

$$
\mathrm{owl{:}Thing}^{C} = \Delta_I
$$

при утверждении:

```turtle
owl:Class owl:disjointWith owl:Thing .
```

получается:

$$
K^C \cap \Delta_I = \varnothing
$$

Но одновременно:

$$
K^C \subseteq \Delta_I
$$

Следовательно:

$$
K^C = \varnothing
$$
```

## Рекомендуемый готовый фрагмент

Его можно непосредственно вставить в GitHub:

````markdown
Можно попытаться записать:

```turtle
owl:Class owl:disjointWith owl:Thing .
```

Но это не выражает разделение «мира классов» и «мира вещей».

Аксиома `owl:disjointWith` означает, что интерпретации двух классов не пересекаются:

$$
C^C \cap D^C = \varnothing
$$

При этом `owl:Thing` интерпретируется как вся область объектов:

$$
\mathrm{owl{:}Thing}^{C} = \Delta_I
$$

Обозначим интерпретацию `owl:Class` через \(K^C\). Тогда:

$$
K^C \subseteq \Delta_I
$$

Если объявить:

```turtle
owl:Class owl:disjointWith owl:Thing .
```

то получится:

$$
K^C \cap \Delta_I = \varnothing
$$

Но одновременно:

$$
K^C \subseteq \Delta_I
$$

Следовательно:

$$
K^C = \varnothing
$$

То есть `owl:Class` фактически будет объявлен пустым классом. Это не разделит «мир классов» и «мир вещей», а сделает класс `owl:Class` пустым.
````

## Правило для GitHub

Для отдельной формулы:

```markdown
$$
формула
$$
```

Для формулы внутри строки:

```markdown
\( формула \)
```

Но для GitHub наиболее надёжный вариант — именно `$...$` и `$$...$$`. Например:

```markdown
Интерпретация класса задаётся как \(C^C \subseteq \Delta_I\).
```

или:

```markdown
$$
C^C \subseteq \Delta_I
$$
```

Не используйте для GitHub в данном случае:

```markdown
\[ формула \]
```

и:

```markdown
[ формула ]
```

`[ ... ]` — это обычные квадратные скобки Markdown, а не математический разделитель.
