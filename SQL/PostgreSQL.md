# PostgreSQL

> Все примеры используют следующую схему:
>
> **employees** (`id`, `name`, `department_id`, `salary`, `hired_at`, `manager_id`)<br>
> **departments** (`id`, `title`, `budget`)<br>
> **projects** (`id`, `name`, `started_at`, `ended_at`)<br>
> **employee_projects** (`employee_id`, `project_id`, `role`)<br>
> **bonuses** (`id`, `employee_id`, `amount`, `paid_at`)<br>

---

| № | Задача | Решение |
|---|--------|---------|
| 1 | Получить только имена и зарплаты сотрудников. | SELECT name, salary<br>FROM employees;<br> |
| 2 | Вывести уникальные значения `department_id`, встречающиеся в таблице сотрудников. | SELECT DISTINCT department_id<br>FROM employees;<br> |
| 3 | Отсортировать сотрудников по отделу (по возрастанию), а внутри отдела — по зарплате от большей к меньшей. | SELECT name, department_id, salary<br>FROM employees<br>ORDER BY department_id ASC, salary DESC;<br> |
| 4 | Показать трёх самых высокооплачиваемых сотрудников. | SELECT name, salary<br>FROM employees<br>ORDER BY salary DESC<br>LIMIT 3;<br> |
| 5 | Найти сотрудников, у которых нет менеджера и чей отдел — 1, 3 или 5. | SELECT name, department_id<br>FROM employees<br>WHERE manager_id IS NULL<br>AND department_id IN (1, 3, 5);<br> |
| 6 | Вывести имя сотрудника и название его отдела. | SELECT e.name, d.title<br>FROM employees e<br>INNER JOIN departments d ON d.id = e.department_id;<br> |
| 7 | Показать все отделы и количество сотрудников в каждом (включая пустые отделы). | SELECT d.title, COUNT(e.id) AS employee_count<br>FROM departments d<br>LEFT JOIN employees e ON e.department_id = d.id<br>GROUP BY d.title;<br> |
| 8 | Получить имя сотрудника, название отдела и название проекта, в котором он участвует. Включить сотрудников без проектов. | SELECT e.name, d.title AS department, p.name AS project<br>FROM employees e<br>INNER JOIN departments d ON d.id = e.department_id<br>LEFT JOIN employee_projects ep ON ep.employee_id = e.id<br>LEFT JOIN projects p ON p.id = ep.project_id;<br> |
| 9 | Для каждого отдела посчитать число сотрудников, среднюю, минимальную и максимальную зарплату. | SELECT d.title,<br>       COUNT(*) AS cnt,<br>       ROUND(AVG(e.salary), 2) AS avg_salary,<br>       MIN(e.salary) AS min_salary,<br>       MAX(e.salary) AS max_salary<br>FROM employees e<br>INNER JOIN departments d ON d.id = e.department_id<br>GROUP BY d.title;<br> |
| 10 | Показать отделы, в которых средняя зарплата превышает 80 000. | SELECT d.title, ROUND(AVG(e.salary), 2) AS avg_salary<br>FROM employees e<br>INNER JOIN departments d ON d.id = e.department_id<br>GROUP BY d.title<br>HAVING AVG(e.salary) > 80000;<br> |
| 11 | Найти сотрудников, чья зарплата выше средней по компании. | SELECT name, salary<br>FROM employees<br>WHERE salary > (SELECT AVG(salary) FROM employees);<br> |
| 12 | Вывести сотрудников, которым хотя бы раз начислялся бонус. | SELECT e.name<br>FROM employees e<br>WHERE EXISTS (<br>    SELECT 1<br>    FROM bonuses b<br>    WHERE b.employee_id = e.id<br>);<br> |
| 13 | Найти проекты, которые длились более 6 месяцев, и показать их длительность в днях. | SELECT name, started_at, ended_at,<br>       (ended_at - started_at) AS duration_days<br>FROM projects<br>WHERE ended_at - started_at > INTERVAL '6 months';<br> |
| 14 | Вывести имя сотрудника и имя его менеджера; если менеджера нет — подставить текст «Нет менеджера». | SELECT e.name AS employee,<br>       COALESCE(m.name, 'Нет менеджера') AS manager<br>FROM employees e<br>LEFT JOIN employees m ON m.id = e.manager_id;<br> |
| 15 | Для каждого сотрудника показать его зарплату и ранг внутри отдела по убыванию зарплаты. | SELECT name, department_id, salary,<br>       RANK() OVER (<br>           PARTITION BY department_id<br>           ORDER BY salary DESC<br>       ) AS salary_rank<br>FROM employees;<br> |
| 16 | Найти отдел с наибольшей суммарной зарплатой. | WITH dept_totals AS (<br>    SELECT department_id, SUM(salary) AS total_salary<br>    FROM employees<br>    GROUP BY department_id<br>)<br>SELECT d.title, dt.total_salary<br>FROM dept_totals dt<br>INNER JOIN departments d ON d.id = dt.department_id<br>ORDER BY dt.total_salary DESC<br>LIMIT 1;<br> |
| 17 | Разделить сотрудников на категории по зарплате: «Junior» (< 50 000), «Middle» (50 000–100 000), «Senior» (> 100 000). | SELECT name, salary,<br>       CASE<br>           WHEN salary < 50000 THEN 'Junior'<br>           WHEN salary <= 100000 THEN 'Middle'<br>           ELSE 'Senior'<br>       END AS grade<br>FROM employees;<br> |