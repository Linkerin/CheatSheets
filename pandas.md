# Pandas Cheat Sheet

## Содержание

[Импорт/экспорт данных](#import-export)  
[Отображение датасета](#dataset)  
[Ключевое поле и извлечение данных](#index)  
[Фильтры](#filters)  
[Изменение данных](#data-change)  
[Удаление/добавление столбцов](#columns)  
[Сортировка данных](#sorting)  
[Аггрегирование и анализ данных](#grouping)  
[Подготовка данных](#data-treatment)  
[Работа с датами](#dates)

---

### Импорт/экспорт данных <a id="import-export"></a>

- Загрузка \*.csv файла

  ```python
  pd.read_csv('source/filename.csv'(, sep=' '))
  ```

- Выгрузка в \*.csv

  ```python
  dataset.to_csv('source/name.csv'(, sep=' '))
  ```

- Загрузка excel-файла, путь к которому прописан в переменной `data_location`

  ```python
  dataset = pd.read_excel(data_location (, index_col='column_1', dtype={'column_2': str}))
  ```

- Выгрузка в Excel

  ```python
  dataset.to_excel('source/name.xlsx')
  ```

- Выгрузка в JSON

  ```python
  dataset.to_json('source/name.json'(, orient='records', lines=True))
  ```

- Загрузка JSON файлов

  ```python
  pd.read_json('source/name.json'(, orient='records', lines=True))
  ```

- Выгрузка в SQL БД

  ```python
  dataset.to_sql('table_name', database (, if_exists='replace'))
  ```

- Загрузка таблицы `table_name` из БД SQL

  ```python
  pd.read_sql('table_name', database)
  ```

- Загрузка данных по SQL-запросу из таблицы `table_name` в SQL БД
  ```python
  pd.read_sql_query('SELECT * FROM table_name', database)
  ```

---

### Отображение датасета <a id="dataset"></a>

`dataset.shape` - количество строк и столбцов  
`dataset.dtypes` - типы данных столбцов
`dataset.info()` - типы данных в стобцах  
`dataset.info` - столбцы, тип данных с превью строк  
`dataset.head(n)` - первые n строк  
`dataset.tail(n)` - последние n строк  
`pd.set_option()` - настройка отображения в pandas.  
`display.max_rows` - максимальное число строк для отображения  
Например: `pd.set_option('display.max_columns', 30)` - отображает максимально 30 столбцов

---

### Ключевое поле и извлечение данных <a id="index"></a>

- Отобразить столбец по имени (series data type)

  ```python
  dataset['Column_name']
  ```

- Отобразить несколько столбцов (dataframe)

  ```python
  dataset[['Column_1', 'Column_2']]
  ```

- Cписок наименований столбцов

  ```python
  dataset.columns
  ```

- Доступ к n-ой по счету строке (с опциональным отожением только m по счету столбца), можно передать список из строк (и столбцов)

  ```python
  dataset.iloc[n(, m)]
  ```

- Отображение строки по заданному индексу(номер п/п, значение и т.д.) (с опциональным фильтром по столбцу), можно передать словарь

  ```python
  dataset.loc[index (, 'column_name')]
  ```

- Указание, какой столбец должен быть уникальным в датафрейме (аналогично параметру `index_col=` при импорте)

  ```python
  dataset.set_index('column', inplace=True)
  ```

- Вернуть ключ к дефолтному значению

  ```python
  dataset.reset_index(inplace=True)
  ```

- Отображает все значения ключевого поля

  ```python
  dataset.index
  ```

- Сортировка по возрастанию; `ascending=False` - атрибут для сортировки по убыванию

  ```python
  dataset.sort_index(inplace=True)
  ```

- Список всех уникальных значений столбца

  ```python
  dataset['column'].unique()
  ```

---

### Фильтры <a id="filters"></a>

- Шаблон для создания переменной, содержащей фильтр. Применяется следующим образом: `dataset[filter_var]` или `dataset.loc[filter_var(, 'some_column')]`

  ```python
  filter_var = dataset['column'] == 'value'
  ```

- противоположный фильтр (поведение аналогичное !=)

  ```python
  ~filter_var
  ```

- Отфильтровывает значения столбца `column`, которые входят в `my_list`

  ```python
  dataset['column'].isin(my_list)
  ```

- Отфильтровывает строковые значения столбца `column`, которые содержат в себе подстроку `my_string`. `na=False` - игнорировать пустые значения
  ```python
  dataset['column'].str.contains('my*string' (, na=False))
  ```

---

### Изменение данных <a id="data-change"></a>

- Пример изменения названия столбцов на заглавные

  ```python
  dataset.columns = [x.upper() for x in dataset.columns]
  ```

- Пример замены пробелов на нижнее подчеркивание

  ```python
  dataset.columns = dataset.columns.str.replace(' ', '_')
  ```

- Переименовать наименование столбцов

  ```python
  dataset.rename(columns={'col1': 'new_col_1', 'col2': 'new_col_2'}, inplace=True)
  ```

- Изменить значения полей `name` и `age` в третьей строке

  ```python
  dataset.loc[2, ['name', 'age']] = ['Joe', 20]
  ```

- Аналогично строке выше

  ```python
  dataset.at[2, ['name', 'age']] = ['Joe', 20]
  ```

- Изменение значения столбца `name` для данных по фильтру из `filter_var`

  ```python
  dataset.loc[filter_var, 'name'] = 'Joe'
  ```

- Пример приведения всех значений столбца `column` к строчному формату

  ```python
  dataset['column'] = dataset['column'].str.lower()
  ```

- Применяет функцию `len()` к значениям столбца `column`. Можно передавать любую функцию

  ```python
  dataset['column'] = dataset['column'].apply(len)
  ```

- Применить функцию в каждому отдельному элементу в dataframe

  ```python
  dataset.applymap(function)
  ```

- Замена значений в столбце `name`
  ```python
  dataset['name'] = dataset['name'].replace({'John': 'Jack', 'Mary': 'Marge'})
  ```

---

### Удаление/добавление столбцов <a id="columns"></a>

- Пример добавления нового столбца `new_col`, содержащего конкатенацию значений столбцов `col_1` и `col_2`

  ```python
  dataset['new_col'] = dataset['col_1'] + dataset['col_2']
  ```

- Удаление столбцов `col_1` и `col_2`

  ```python
  dataset.drop(columns=['col_1', 'col_2'], inplace=True)
  ```

- Разделить столбец `column` на столбцы `col1` и `col2`, в данном случае разделитель - пробел

  ```python
  dataset[['col1', 'col2']] = dataset['column'].str.split(' ', expand=True)
  ```

- Добавляет новую строку со значением `Jack` в столбце `name`

  ```python
  dataset.append({'name': 'Jack'} (, ignore_index=True))
  ```

- Объединить два датасета

  ```python
  dataset = dataset.append(dataset_2, ignore_index=True)
  ```

- Удаляет строку с указанным в `value` идентификатором

  ```python
  dataset.drop(index=value, inplace=True)
  ```

- Определение идентификатора строки по условию (значение = `John`) и удаление строки
  ```python
  dataset.drop(index=dataset[dataset['name'] == 'John'].index)
  ```

---

### Сортировка данных <a id="sorting"></a>

- Сортировка данных по значениям `column`. Вместо `column` можно список столбцов

  ```python
  dataset.sort*values(by='column' (, ascending=False, inplace=True))
  ```

- Пример, когда данные сортируются сначала по убыванию `col_1`, зато по возрастанию `col_2`

  ```python
  dataset.sort_values(by=['col_1', 'col_2'], ascending=[False, True], inplace=True)
  ```

- Возвращает к сортировке по возрастанию идентификатора

  ```python
  dataset.sort_index()
  ```

- Сортировка по возрастанию значений столбца `column`

  ```python
  dataset['column'].sort_values()
  ```

- Первые _n_ максимальных значений столбца `column`

  ```python
  dataset['column'].nlargest(n)
  ```

- Аналогично предыдущему, но отображает весь датасет

  ```python
  dataset.nlargest(n, 'column')
  ```

- Первые _n_ минимальных значений столбца `column`
  ```python
  dataset['column'].nsmallest(n)
  ```

---

### Аггрегирование и анализ данных <a id="grouping"></a>

- Медиана значений столбца `column`

  ```python
  dataset['column'].median()
  ```

- Среднее арифметическое значений столбца `column`

  ```python
  dataset['column'].mean()
  ```

- Расчет основных числовых параметров набора данных

  ```python
  dataset.describe()
  ```

- Количество непустых значений в столбце

  ```python
  dataset['column'].count()
  ```

- Подсчет уникальных значений в столбце. `normalize=True` - аргумент для перевода в проценты

  ```python
  dataset['column'].value_counts()
  ```

- Возвращает dataframe, сгрупированный по переданным в списке в качестве аргумента названиями полей. Далее вместо этой функции будет указана переменная `grouped_data`

  ```python
  dataset.groupby(['column'])
  ```

- Отображает данные, сгрубированные в отношении `value`. Аналогично можно использовать фильтр

  ```python
  grouped_data.get_group('value')
  ```

- Пример отображения количества уникальных значений столбца `name` в разрезе группировки по `value`

  ```python
  grouped_data['name'].value_counts().loc['value']
  ```

- Пример использования нескольких аггрегирующих функций на сгрупированном масиве данных для значений столбца `name`

  ```python
  grouped_data['name'].agg(['median', 'mean'])
  ```

- Пример посчета всех значений, содержащих подстроку `Jack`, в поле `name` в разрезе сгрупированных по `value` данных

  ```python
  grouped_data['name'].apply(lambda x: x.str.contains('Jack').sum())
  ```

- Объединенние двух массивов сгруппированных данных (DataSeries)
  ```python
  concat_groups = pd.concat([grouped_data, grouped_data_2] (, axis='columns', sort=False))
  ```

---

### Подготовка данных <a id="data-treatment"></a>

- Удаляет всю строку, если хотя бы одно значение в ней None или NaN (дефолтно `how='any'`)  
  `axis='index'` - анализирует данные построчно, `column` - по столбцам  
  `how='all'` - если все значения строки/столбца отсутствуют  
  `subset=['name']` - позволяет указать, в каких полях необходимо искать отсутствующие значения

  ```python
  dataset.dropna(inplace=True)
  ```

- Заменит все строковые значения `'NA'` в массиве данных на `None`

  ```python
  dataset.replace('NA', None, inplace=true)
  ```

- Отобразит маску, по которой можно увидеть, где отсутствуют данные

  ```python
  dataset.isna()
  ```

- Заменит все отсутвующие данные на `'NA'`

  ```python
  dataset.fillna('NA', inplace=True)
  ```

- Переведет все значения столбца `column` в тип данных `integer`
  ```python
  dataset['column'] = dataset['column'].astype(int)
  ```

---

### Работа с датами <a id="dates"></a>

- Перевод формата значений столбца `Date` к дате. Пример формата даты указан для **_2020-03-14 05-PM_**

  ```python
  dataset['Date'] = pd.to_datetime(dataset['Date'], format='%Y-%m-%d %I-%p')
  ```

- Возвращает текстовый день недели для значения поля `Date` в первой строке

  ```python
  dataset.loc[0, 'Date'].day_name()
  ```

- Пример перевода данных в столбцах с данными на этапе загрузки собственной функцией `d_parser`:

  ```python
  d_parser = lambda x: pd.datetime.strptime(x, '%Y-%m-%d %I-%p')
  dataset = pd.read_csv('my_file.csv', parse_dates=['Date_1', 'Date_2'], date_parser=d_parser)
  ```

- Дни недели всех значений столбца `Date`

  ```python
  dataset['Date'].dt.day_name()
  ```

- Поиск минимальной (максимальная) даты столбца

  ```python
  dataset['Date'].min() # .max()
  ```

- Значения поля `Date` за 2020 год и позже. Можно использовать перевод строки в формат даты через `pd.to_datetime()`

  ```python
  dataset['Date'] >= '2020'
  ```

- Пример среза периода, когда `Date` - ключевое поле

  ```python
  dataset['2020-01':'2020-02']
  ```

- Отображает максимальное значение столбца `column`, агрегируя до недели (для других аргументов функции см. коды `date offset` в [_pandas_](https://pandas.pydata.org/docs/). `Date` - ключевое поле

  ```python
  dataset['column'].resample('W').max()
  ```

- Отобразит данные в разрезе по дням с применением соотвествующих функций к столбцам
  ```python
  dataset.resample('D').agg({'col_1': 'mean', 'col_2': 'max'})
  ```
