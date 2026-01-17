# SQL 

## Фундаментальные концепции и принципы SQL 
# SQL (Structured Query Language)

**SQL** (Structured Query Language) — это язык структурированных запросов, созданный для управления реляционными базами данных. Его можно представить как «инструкцию» для работы с упорядоченными таблицами данных.

## Назначение и концепция

SQL предназначен для:
- **Хранения информации**
- **Обработки данных**
- **Извлечения информации**

Язык тесно связан с системами управления базами данных (СУБД), и его существование вне СУБД невозможно.

## Архитектура клиент-сервер

Развитие SQL плотно переплеталось с парадигмой **клиент/серверных вычислений** задолго до того, как этот термин стал широко распространенным.

### Роль клиента
- Знать, как подключиться к серверу
- Формулировать запросы к данным
- Представлять результаты пользователю в удобном формате

### Роль сервера (СУБД)
- Интерпретировать запросы клиента
- Возвращать соответствующие данные
- Поддерживать данные в оптимальном состоянии для:
  - Производительности обработки запросов
  - Целостности данных
  - Защиты информации

## Абстракция и реализация

### Простота на поверхности
Сложность низкоуровневой реализации (перевод запросов на машинный язык) скрыта под покровом простых и понятных инструкций:

- **SELECT** — выборка данных
- **INSERT** — вставка данных
- **UPDATE** — обновление данных
- **DELETE** — удаление данных

### Ответственность СУБД
Всю задачу преобразования этих инструкций в реальные компьютерные команды берет на себя **СУБД**.

## Стандартизация и совместимость

### Гибкость стандартов
В стандартах SQL определены только строительные блоки инструкций, без указаний на способы их реализации. Это предоставляет производителям СУБД значительную свободу:

1. **Выбор реализуемых функций** — производители сами решают, какие части стандарта SQL реализовывать
2. **Интерпретация стандартов** — разные производители могут по-разному интерпретировать одни и те же элементы стандарта

### Проблема совместимости
**Расширения**, вводимые производителями СУБД, усложняют вопрос совместимости между разными системами. Однако важно отметить, что иногда именно эти расширения становятся основой для следующих версий стандарта SQL.

## Ключевые особенности

- **Декларативный характер** — описывается "что" нужно получить, а не "как"
- **Платформонезависимость** (в теории) — один SQL-запрос может работать на разных СУБД
- **Мощность и выразительность** — возможность выполнения сложных операций с данными
- **Многоуровневая абстракция** — от простых запросов до сложных транзакционных операций

---

*Примечание: Несмотря на существование стандартов ANSI SQL, разные СУБД (MySQL, PostgreSQL, Oracle, SQL Server и др.) имеют свои диалекты и расширения, что требует внимания при миграции или разработке кроссплатформенных решений.*


## 🔗 Подключение к базе данных

```sql
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo -u postgres psql

-- Подключение через psql
psql -h localhost -p 5432 -U username database_name

-- Внутри psql подключение к другой БД
\c database_name

-- Просмотр всех баз данных
\l
```

## 📊 Основные команды для работы с данными

### 1. Создание таблицы
```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    department VARCHAR(50),
    salary DECIMAL(10, 2),
    hire_date DATE DEFAULT CURRENT_DATE,
    is_active BOOLEAN DEFAULT TRUE
);
```
<img width="1202" height="349" alt="image" src="https://github.com/user-attachments/assets/a2575233-5009-44e3-8ed6-f49733b1fd00" />

### Так-же возможна генерация данных и их последующее импортирование. [Сайт][https://www.mockaroo.com/]
**Импортирование**
```sql
\i расположение файла.sql
```
<img width="730" height="413" alt="image" src="https://github.com/user-attachments/assets/da1a4de1-88f0-4ce3-9c7c-896e334294ee" />
<img width="1265" height="756" alt="image" src="https://github.com/user-attachments/assets/6dd2df70-b38b-4948-9fc7-d5291f57b9c0" />


### 2. Основные операции с данными (CRUD)

**INSERT** - добавление данных:
```sql
-- Добавление одной записи
INSERT INTO employees (first_name, last_name, email, department, salary)
VALUES ('Иван', 'Иванов', 'ivanov@company.com', 'IT', 75000);

-- Добавление нескольких записей
INSERT INTO employees (first_name, last_name, department, salary)
VALUES 
    ('Мария', 'Петрова', 'HR', 60000),
    ('Алексей', 'Сидоров', 'Sales', 65000),
    ('Ольга', 'Кузнецова', 'IT', 80000);

-- Обновление значений
UPDATE employees set email = 'valeraboyko2002@yandex.ru' where id = 2;
-- Увеличить всем зарплату на 10 %
UPDATE employees set salary = salary*1.1 where department = 'IT';
```
<img width="1031" height="344" alt="image" src="https://github.com/user-attachments/assets/f2519a88-04a1-43cd-86b7-adb75a678b61" />
<img width="1086" height="590" alt="image" src="https://github.com/user-attachments/assets/d4ac18d8-5048-4ee1-9e1a-8de8c209e143" />


**SELECT** - выборка данных:
```sql
-- Выборка всех столбцов
SELECT * FROM employees;

-- Выборка конкретных столбцов
SELECT first_name, last_name, department FROM employees;

-- С лимитом записей
SELECT * FROM employees LIMIT 10;

-- Уникальные значения
SELECT DISTINCT department FROM employees;
```
<img width="546" height="361" alt="image" src="https://github.com/user-attachments/assets/f648af91-e59d-4cdd-b7cf-20da81396fd6" />



**WHERE** - фильтрация данных:
```sql
-- Простые условия
SELECT * FROM employees WHERE department = 'IT';
SELECT * FROM employees WHERE salary > 70000;

-- Несколько условий
SELECT * FROM employees 
WHERE department = 'IT' AND salary > 70000;

SELECT * FROM employees 
WHERE department = 'IT' OR department = 'Sales';
<img width="1516" height="269" alt="image" src="https://github.com/user-attachments/assets/1881820b-fe93-4717-8f6c-9055a3f18ce2" />

-- LIKE для поиска по шаблону
SELECT * FROM employees WHERE first_name LIKE 'Ив%';  -- начинается на "Ив"
SELECT * FROM employees WHERE email LIKE '%@company.com';

-- BETWEEN для диапазона
SELECT * FROM employees WHERE salary BETWEEN 60000 AND 80000;

-- IN для списка значений
SELECT * FROM employees WHERE department IN ('IT', 'HR', 'Sales');
```
<img width="1051" height="237" alt="image" src="https://github.com/user-attachments/assets/e35aae43-257a-4633-bc52-faab4c3009b8" />
<img width="753" height="133" alt="image" src="https://github.com/user-attachments/assets/60ed9b25-9bf1-4d66-badf-f420b8104ade" />



**ORDER BY** - сортировка:
```sql
-- По возрастанию (ASC по умолчанию)
SELECT * FROM employees ORDER BY salary;

-- По убыванию
SELECT * FROM employees ORDER BY salary DESC;

-- Сортировка по нескольким полям
SELECT * FROM employees ORDER BY department ASC, salary DESC;
```
<img width="1014" height="196" alt="image" src="https://github.com/user-attachments/assets/81becee2-adb6-4828-8ab6-f07854b1ba79" />
<img width="1023" height="453" alt="image" src="https://github.com/user-attachments/assets/29221893-01e1-45d8-bf2d-defc259cbb72" />


**UPDATE** - обновление данных:
```sql
-- Обновление одной записи
UPDATE employees 
SET salary = 80000 
WHERE id = 1;

-- Обновление нескольких полей
UPDATE employees 
SET salary = salary * 1.1,  -- увеличение на 10%
    department = 'IT Senior'
WHERE department = 'IT' AND salary < 70000;
```

**DELETE** - удаление данных:
```sql
-- Удаление конкретной записи
DELETE FROM employees WHERE id = 5;

-- Удаление по условию
DELETE FROM employees WHERE is_active = FALSE;

-- Осторожно: удаление всех записей
-- DELETE FROM employees;  -- ОПАСНО!
```

### 3. Агрегатные функции
```sql
-- COUNT - подсчет записей
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department) FROM employees;

-- SUM - сумма
SELECT SUM(salary) AS total_salary FROM employees;
SELECT department, SUM(salary) FROM employees GROUP BY department;

-- AVG - среднее значение
SELECT AVG(salary) AS average_salary FROM employees;
SELECT department, AVG(salary) FROM employees GROUP BY department;

-- MIN/MAX - минимальное/максимальное значение
SELECT MIN(salary), MAX(salary) FROM employees;
SELECT department, MIN(salary), MAX(salary) FROM employees GROUP BY department;
```
<img width="655" height="375" alt="image" src="https://github.com/user-attachments/assets/77b5e566-010b-42c5-a7c6-79ad1d2ea9ff" />

### 4. GROUP BY и HAVING
```sql
-- Группировка с агрегатами
SELECT 
    department,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary,
    SUM(salary) as total_salary
FROM employees
GROUP BY department;

-- HAVING для фильтрации групп
SELECT 
    department,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING COUNT(*) > 2 AND AVG(salary) > 65000;
```

### 5. JOIN - соединение таблиц
```sql
-- INNER JOIN (только совпадающие записи)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name,
    d.location
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;

-- LEFT JOIN (все записи из левой таблицы)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;

-- RIGHT JOIN (все записи из правой таблицы)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;

-- FULL OUTER JOIN (все записи из обеих таблиц)
SELECT 
    e.first_name,
    e.last_name,
    d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```

### 6. Подзапросы (Subqueries)
```sql
-- В условии WHERE
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);

-- В SELECT
SELECT 
    first_name,
    last_name,
    salary,
    (SELECT AVG(salary) FROM employees) as company_avg
FROM employees;

-- В FROM (как таблица)
SELECT 
    department,
    avg_salary
FROM (SELECT 
        department,
        AVG(salary) as avg_salary
      FROM employees
      GROUP BY department) as dept_stats
WHERE avg_salary > 70000;
```

### 7. Аналитические функции (оконные функции)
```sql
-- ROW_NUMBER - нумерация строк
SELECT 
    first_name,
    last_name,
    salary,
    department,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as salary_rank
FROM employees;

-- RANK и DENSE_RANK
SELECT 
    first_name,
    last_name,
    salary,
    department,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_dense_rank
FROM employees;

-- LAG/LEAD - доступ к предыдущей/следующей строке
SELECT 
    first_name,
    last_name,
    salary,
    LAG(salary) OVER (ORDER BY salary) as prev_salary,
    LEAD(salary) OVER (ORDER BY salary) as next_salary
FROM employees;

-- SUM OVER - накопительная сумма
SELECT 
    first_name,
    last_name,
    salary,
    department,
    SUM(salary) OVER (PARTITION BY department ORDER BY hire_date) as cumulative_salary
FROM employees;
```

### 8. Работа с датами
```sql
-- Текущая дата и время
SELECT CURRENT_DATE, CURRENT_TIME, NOW();

-- Извлечение частей даты
SELECT 
    hire_date,
    EXTRACT(YEAR FROM hire_date) as hire_year,
    EXTRACT(MONTH FROM hire_date) as hire_month,
    EXTRACT(DAY FROM hire_date) as hire_day
FROM employees;

-- Разница между датами
SELECT 
    first_name,
    last_name,
    hire_date,
    CURRENT_DATE - hire_date as days_employed
FROM employees;

-- Форматирование дат
SELECT 
    first_name,
    last_name,
    TO_CHAR(hire_date, 'DD.MM.YYYY') as formatted_date,
    TO_CHAR(hire_date, 'Month YYYY') as month_year
FROM employees;
```

### 9. CASE - условные выражения
```sql
-- Простой CASE
SELECT 
    first_name,
    last_name,
    salary,
    CASE 
        WHEN salary < 60000 THEN 'Junior'
        WHEN salary BETWEEN 60000 AND 90000 THEN 'Middle'
        ELSE 'Senior'
    END as level,
    CASE department
        WHEN 'IT' THEN 'Технический отдел'
        WHEN 'HR' THEN 'Отдел кадров'
        ELSE 'Другой отдел'
    END as department_ru
FROM employees;
```

### 10. Практические примеры для анализа
```sql
-- Топ-5 самых высокооплачиваемых сотрудников
SELECT 
    first_name,
    last_name,
    department,
    salary
FROM employees
ORDER BY salary DESC
LIMIT 5;

-- Распределение сотрудников по отделам
SELECT 
    department,
    COUNT(*) as employee_count,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM employees), 2) as percentage
FROM employees
GROUP BY department
ORDER BY employee_count DESC;

-- Динамика найма по месяцам
SELECT 
    EXTRACT(YEAR FROM hire_date) as year,
    EXTRACT(MONTH FROM hire_date) as month,
    COUNT(*) as hires
FROM employees
GROUP BY year, month
ORDER BY year, month;

-- Зарплатная вилка по отделам
SELECT 
    department,
    COUNT(*) as total_employees,
    MIN(salary) as min_salary,
    MAX(salary) as max_salary,
    AVG(salary) as avg_salary,
    MAX(salary) - MIN(salary) as salary_range
FROM employees
GROUP BY department
HAVING COUNT(*) > 1;
```

## 🚨 Важные предупреждения

```sql
-- Никогда не выполняйте эти команды без необходимости:

-- 1. Удаление базы данных
-- DROP DATABASE production_db;  -- ПОТЕРЯ ВСЕХ ДАННЫХ!

-- 2. Удаление таблицы
-- DROP TABLE important_table;  -- ПОТЕРЯ ТАБЛИЦЫ!

-- 3. Обновление без WHERE
-- UPDATE users SET password = '123';  -- ИЗМЕНИТ ВСЕ ЗАПИСИ!

-- 4. Удаление без WHERE
-- DELETE FROM orders;  -- УДАЛИТ ВСЕ ЗАПИСИ!

-- Всегда проверяйте что делаете:
SELECT * FROM table WHERE условие;  -- Сначала посмотреть
UPDATE table SET поле = значение WHERE условие;  -- Потом изменить
```

## 📝 Лучшие практики

1. **Используйте псевдонимы (aliases)** для читаемости
2. **Комментируйте** сложные запросы
3. **Тестируйте** на маленьких наборах данных
4. **Используйте транзакции** при изменении данных
5. **Делайте бэкапы** перед массовыми изменениями
6. **Используйте EXPLAIN** для анализа производительности

## 🔧 Полезные команды psql
```bash
# Подключение
psql -U username -d database -h host -p port

# Команды внутри psql:
\?              # Справка по командам
\l              # Список баз данных
\c dbname       # Подключение к БД
\dt             # Список таблиц
\d tablename    # Описание таблицы
\q              # Выход
```

## 🎯 Быстрый старт
```sql
-- 1. Создайте копию для тестирования
CREATE TABLE employees_copy AS SELECT * FROM employees;

-- 2. Проверьте структуру
\d employees_copy

-- 3. Поэкспериментируйте с данными
SELECT * FROM employees_copy LIMIT 5;

-- 4. Попробуйте агрегаты
SELECT department, COUNT(*), AVG(salary) 
FROM employees_copy 
GROUP BY department;

-- 5. Очистите после тестов
DROP TABLE employees_copy;
```
