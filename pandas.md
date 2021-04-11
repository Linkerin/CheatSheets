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
___
### Импорт/экспорт данных <a id="import-export"></a>
**pd.read_csv('source'*(, sep=' ')*)** - загрузка *.csv

**dataset.to_csv('source/name.csv'*(, sep=' ')*)** - выгрузка в *.csv

**dataset = pd.read_excel(data_location *(, index_col='column_1', dtype={'column_2': str})*)** - загрузка excel-файла, путь к которому прописан в переменной *data_location*

**dataset.to_excel('source/name.xlsx')** - выгрузка в Excel

**dataset.to_json('source/name.json'*(, orient='records', lines=True)*)** - выгрузка в JSON

**pd.read_json('source/name.json'*(, orient='records', lines=True)*)** - загрузка JSON  файлов

**dataset.to_sql('table_name', database *(, if_exists='replace')*)** - выгрузка в SQL БД

**pd.read_sql('table_name', database)** - загрузка таблицы *table_name* из БД SQL

**pd.read_sql_query('SELECT * FROM table_name', database)** - загрузка данных по SQL-запросу из таблицы *table_name* в SQL БД
___

### Отображение датасета <a id="dataset"></a>
**dataset.shape** - количество строк и столбцов  
**dataset.dtypes** - типы данных столбцов
**dataset.info()** - типы данных в стобцах  
**dataset.info** - столбцы, тип данных с превью строк  
**dataset.head(n)** - первые n строк  
**dataset.tail(n)** - последние n строк  
**pd.set_option()** - настройка отображения в pandas.  
Например: `pd.set_option('display.max_columns', 30)` - отображает максимально 30 столбцов // `display.max_rows`
___

### Ключевое поле и извлечение данных <a id="index"></a>
**dataset['Column_name']** - отобразить столбец по имени (series data type)

**dataset[['Column_1', 'Column_2']]** - отобразить несколько столбцов (dataframe)

**dataset.columns** - список наименований столбцов

**dataset.iloc[n(, m)]** - доступ к n-ой по счету строке (с опциональным отожением только m по счету столбца), можно передать список из строк (и столбцов)

**dataset.loc[index (, 'column_name')]** - отображение строки по заданному индексу(номер п/п, значение и т.д.) (с опциональным фильтром по столбцу), можно передать словарь

**dataset.set_index('column', inplace=True)** - указание, какой столбец должен быть уникальным в датафрейме (аналогично параметру `index_col=` при импорте)

**dataset.reset_index(inplace=True)** - вернуть ключ к дефолту

**dataset.index** - отображает все значения ключевого поля

**dataset.sort_index(inplace=True)** - сортировка по возрастанию `ascending=False` - атрибут для сортировки по убыванию

**dataset['column'].unique()** - список всех уникальных значений столбца
___

### Фильтры <a id="filters"></a>
**filter_var = dataset['column'] == 'value'** - шаблон для создания переменной, содержащей фильтр. Применяется следующим образом: `dataset[filter_var]` или `dataset.loc[filter_var(, 'some_column')]`

**~filter_var** - противоположный фильтр (поведение аналогичное !=)

**dataset['column'].isin(my_list)** - отфильтровывает значения столбца *column*, которые входят в *my_list*

**dataset['column'].str.contains('my_string'*(, na=False)*)** - отфильтровывает строковые значения столбца *column*, которые содержат в себе подстроку *my_string*. `na=False` - игнорировать пустые значения
___

### Изменение данных <a id="data-change"></a>
**dataset.columns = [x.upper() for x in dataset.columns]** - пример изменения названия столбцов на заглавные

**dataset.columns = dataset.columns.str.replace(' ', '_')** - пример замены пробелов на нижнее подчеркивание

**dataset.rename(columns={'col1': 'new_col_1', 'col2': 'new_col_2'}, inplace=True)** - переименовать наименование столбцов

**dataset.loc[2, ['name', 'age']] = ['Joe', 20]** - изменить значения полей *name* и *age* в третьей строке

**dataset.at[2, ['name', 'age']] = ['Joe', 20]** - аналогично строке выше

**dataset.loc[*filter_var*, 'name'] = 'Joe'** - изменение значения столбца *name* для данных по фильтру из *filter_var*

**dataset['column'] = dataset['column'].str.lower()** - пример приведения всех значений столбца *column* к строчному формату

**dataset['column'] = dataset['column'].apply(len)** - применяет функцию `len()` к значениям столбца *column*. Можно передавать любую функцию

**dataset.applymap(function)** - применить функцию в каждому отдельному элементу в dataframe

**dataset['name'] = dataset['name'].replace({'John': 'Jack', 'Mary': 'Marge'})** - замена значений в столбце *name*
___

### Удаление/добавление столбцов <a id="columns"></a>
**dataset['new_col'] = dataset['col_1'] + dataset['col_2']** - пример добавления нового столбца *new_col*, содержащего конкатенацию значений столбцов *col_1* и *col_2*

**dataset.drop(columns=['col_1, 'col_2'], inplace=True)** - удаление столбцов *col_1* и *col_2*

**dataset[['col1', 'col2']] = dataset['column'].str.split(' ', expand=True)** - разделить столбец *column* на столбцы *col1* и *col2*, в данном случае разделитель - пробел

**dataset.append({'name': 'Jack'}*(, ignore_index=True)*)** - добавляет новую строку со значением *Jack* в столбце *name*

**dataset = dataset.append(dataset_2, ignore_index=True)** - объединить два датасета

**dataset.drop(index=value, inplace=True)** - удаляет строку с указанным в *value* идентификатором

**dataset.drop(index=dataset[dataset['name'] == 'John'].index)** - определение идентификатора строки по условию (значение = *John*) и удаление строки
___

### Сортировка данных <a id="sorting"></a>
**dataset.sort_values(by='column'*(, ascending=False, inplace=True)*)** - сортировка данных по значениям *column*. Вместо *column* можно список столбцов

**dataset.sort_values(by=['col_1', 'col_2'], ascending=[False, True], inplace=True)** - пример, когда данные сортируются сначала по убыванию *col_1*, зато по возрастанию *col_2*

**dataset.sort_index()** - возвращает к сортировке по возрастанию идентификатора

**dataset['column'].sort_values()** - сортировка по возрастанию значений столбца *column*

**dataset['column'].nlargest(n)** - первые n максимальных значений столбца *column*

**dataset.nlargest(n, 'column')** - аналогично предыдущему, но отображает весь датасет

**dataset['column'].nsmallest(n)** - первые n минимальных значений столбца *column*
___

### Аггрегирование и анализ данных <a id="grouping"></a>
**dataset['column'].median()** - медиана значений столбца *column*

**dataset['column'].mean()** - среднее арифметическое значений столбца *column*

**dataset.describe()** - расчет основных числовых параметров набора данных

**dataset['column'].count()** - количество непустых значений в столбце

**dataset['column'].value_counts()** - подсчет уникальных значений в столбце. `normalize=True` - аргумент для перевода в проценты

**dataset.groupby(['column'])** - возвращает dataframe, сгрупированный по переданным в списке в качестве аргумента названиями полей. Далее вместо этой функции будет указана переменная `grouped_data`

**grouped_data.get_group('value')** - отображает данные, сгрубированные в отношении *value*. Аналогично можно использовать фильтр

**grouped_data['name'].value_counts().loc['value']** - пример отображения количества уникальных значений столбца *name* в разрезе группировки по *value*

**grouped_data['name'].agg(['median', 'mean'])** - пример использования нескольких аггрегирующих функций на сгрупированном масиве данных для значений столбца *name*

**grouped_data['name'].apply(lambda x: x.str.contains('Jack').sum())** - пример посчета всех значений, содержащих подстроку *Jack*, в поле *name* в разрезе сгрупированных по *value* данных

**concat_groups = pd.concat([grouped_data, grouped_data_2]*(, axis='columns', sort=False)*)** - объединенние двух массивов сгруппированных данных (DataSeries)
___

### Подготовка данных <a id="data-treatment"></a>
**dataset.dropna(inplace=True)** - удаляет всю строку, если хотя бы одно значение в ней None или NaN (дефолтно `how='any'`)  
`axis='index'` - анализирует данные построчно, `column` - по столбцам  
`how='all'` - если все значения строки/столбца отсутствуют  
`subset=['name']` - позволяет указать, в каких полях необходимо искать отсутствующие значения  

**dataset.replace('NA', None, inplace=true)** - заменит все строковые значения *NA* в массиве данных на *None*

**dataset.isna()** - отобразит маску, по которой можно увидеть, где отсутствуют данные

**dataset.fillna('NA', inplace=True)** - заменит все отсутвующие данные на *NA*

**dataset['column'] = dataset['column'].astype(int)** - переведет все значения столбца *column* в тип данных *integer*
___

### Работа с датами <a id="dates"></a>
**dataset['Date'] = pd.to_datetime(dataset['Date'], format='%Y-%m-%d %I-%p')** - перевод формата значений столбца *Date* к дате. Пример формата даты указан для ***2020-03-14 05-PM***

**dataset.loc[0, 'Date'].day_name()** - возвращает текстовый день недели для значения поля *Date* в первой строке

**dataset = pd.read_csv('my_file.csv', parse_dates=['Date_1', 'Date_2'], date_parser=d_parser)** - пример перевода данных в столбцах с данными на этапе загрузки собственной функцией `d_parser`:  
`d_parser = lambda x: pd.datetime.strptime(x, '%Y-%m-%d %I-%p')`

**dataset['Date'].dt.day_name()** - дни недели всех значений столбца *Date*

**dataset['Date'].min() / .max()** - поиск минимальной/максимальная даты столбца

**dataset['Date'] >= '2020'** - значения поля *Date* за 2020 год и позже. Можно использовать перевод строки в формат даты через `pd.to_datetime()`

**dataset['2020-01':'2020-02']** - пример среза периода, когда *Date* - ключевое поле

**dataset['column'].resample('W').max()** - отображает максимальное значение столбца *column*, агрегируя до недели (для других аргументов функции см. коды *date offset* в *pandas*). *Date* - ключевое поле

**dataset.resample('D').agg({'col_1': 'mean', 'col_2': 'max'})** - отобразит данные в разрезе по дням с применением соотвествующих функций к столбцам