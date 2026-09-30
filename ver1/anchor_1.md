# Правила обработки якорей на GitHub

GitHub автоматически генерирует `id` для каждого заголовка Markdown при рендеринге. Алгоритм основан на библиотеке `github-slugger`.

## Основные правила

1. Приведение к нижнему регистру.  
2. Пробелы заменяются на дефисы.  
3. Дефисы сохраняются.  
4. Знаки препинания удаляются.  
5. Кириллица сохраняется и приводится к нижнему регистру.  
6. Коллизии разрешаются суффиксом `-1`, `-2` и т.д.  

## Примеры

### Person  

Сгенерированный id: person  
Ссылка: #person  

### PostalAddress  

Сгенерированный id: postaladdress  
Ссылка: #postaladdress  

### hasAddress  

Сгенерированный id: hasaddress  
Ссылка: #hasaddress  

### Exe1-1  

Сгенерированный id: exe1-1  
Ссылка: #exe1-1  

### Алиса  

Сгенерированный id: алиса  
Ссылка: #алиса  

## Обработка дубликатов

Если в файле два заголовка дают одинаковый id, первый получает id без суффикса, второй — с `-1`.

Пример:

### exe  

Первый раздел.  

### Exe  

Второй раздел.  

GitHub сгенерирует `id="exe"` для первого и `id="exe-1"` для второго. Ссылка `#Exe` нормализуется в `#exe` и откроет первый раздел. Чтобы попасть во второй — `#exe-1`.

## Кириллица и percent-encoding

Кириллические символы в URL должны быть percent-encoded. Браузеры обычно делают это автоматически.

Пример:  
Заголовок `### Алиса` → id `алиса`.  
Ссылка `#алиса` работает.  
Percent-encoded: `#%D0%B0%D0%BB%D0%B8%D1%81%D0%B0`.  

## Ссылки

- GitHub: создание якорей — https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#section-links  
- github-slugger (npm) — https://www.npmjs.com/package/github-slugger  
- https://github.com/bpmbpm/mdld-test/blob/main/ver2/doc/easy/alice3d2.md#-5-anchormd
