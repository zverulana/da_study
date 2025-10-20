 [1] Задача (16 октября)
 ```sql
-- Найдите топ-3 сотрудников в каждом отделе по зарплате, выведите их имена, отделы и ранг. Исключите сотрудников с зарплатой ниже 30000

with tmp as (select first_name, last_name, department_name, salary,
DENSE_RANK() over (partition by department_name order by salary desc) as rank
from sandbox.departments_zvereva d inner join sandbox.employees_zvereva e ON d.department_id = e.department_id and salary > 30000)

select first_name, last_name, department_name, salary from tmp
where rank <= 3
 ```
# [2] Задача (17 октября)

 ```sql
with tmp as (select department_id, department_name,
AVG(salary) as average
from sandbox.employees_zvereva e
inner join sandbox.departments_zvereva d using (department_id)
group by department_name,department_id)

select first_name, last_name, department_name, s.salary -  t.average as diff 
from sandbox.employees_zvereva s inner join tmp t on s.department_id = t.department_id 
and s.salary > t.average
order by diff desc
 ```
# [3] Задача (18 октября)

 ```sql
-- Для каждого отдела найдите сотрудника с самой высокой зарплатой и выведите его имя, отдел и зарплату. Исключите отделы с менее чем 3 сотрудниками

with tmp AS(
select first_name, last_name, department_name, salary,
rank() over (partition by department_id order by salary) as rank,
count(e.id) over (partition by department_id) as cnt
from sandbox.employees_zvereva e inner join sandbox.departments_zvereva d using (department_id)
)

select  first_name, last_name, department_name, salary from tmp
where rank = 1 and cnt >= 3

 ```
# [4] Задача (19 октября)

  ```sql
-- Выведите клиентов и их заказы, добавив ранг заказов по сумме для каждого клиента. Показать только заказы с суммой > 1000

select customer_name, order_id,
rank () over (partition by customer_id order by amount) as rank
from sandbox.customers_zvereva c join sandbox.orders_zvereva o using (customer_id)
where amount > 1000

 ```

 # [5] Задача (20 октября)

  ```sql
-- Выведите клиентов и их заказы, добавив ранг заказов по сумме для каждого клиента. Показать только заказы с суммой > 1000

select customer_name, order_id,
rank () over (partition by customer_id order by amount) as rank
from sandbox.customers_zvereva c join sandbox.orders_zvereva o using (customer_id)
where amount > 1000

 ```

  # [6] Задача (21 октября)

  ```sql
-- 

 ```




