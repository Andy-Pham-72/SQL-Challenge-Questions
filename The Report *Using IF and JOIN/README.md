# The Report

You are given two tables

### Students

![1443818166-a5c852caa0-1](https://user-images.githubusercontent.com/70767722/123565068-9f6acc80-d789-11eb-8e7f-681a123426f6.png)

### Grades

![1443818137-69b76d805c-2](https://user-images.githubusercontent.com/70767722/123565072-a265bd00-d789-11eb-99a0-fa3264c8272f.png)

**Scenario**: 
- Ketty gives Eve a task to generate a report containing three columns: `Name`, `Grade`, and `Mark`.
- Ketty doesn't want the `NAMES` of those students who recieved a grade lower than 8. 
    - The report must be in `DESCENDING` order by grade. i.e.: Highers grades are entered first.
    - If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically.
- Finally, if the grade is lower than 8, use **`NULL`** as their name and list them by their grades in `DESCENDING` order.
    - If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.


### Sample Input

![1443818093-b79f376ec1-3](https://user-images.githubusercontent.com/70767722/123565134-ce813e00-d789-11eb-9db2-3cee8292cb42.png)

### Sample Output

```
Maria   10 99
Jane    9  81
Julia   9  88
Scarlet 8 78
NULL    7 63
NULL    7 68
```

### NOTE: Print "NULL" as the name if the grade is less than 8

### Explanation:

Consider the following table with the grades assigned to the students:

![1443818026-0b3af8db30-4](https://user-images.githubusercontent.com/70767722/123565130-c9bc8a00-d789-11eb-9c13-8ce28ccdec41.png)

So, the following students got 8,9, and 10 grades:

    - Maria (grade 10)
    - Jane  (grade 9)
    - Julia (grade 9)
    - Scarlet (grade 8)
    
    
# Solution for MySQL

In order to solve this problem, we can use:
* **`IF`** to assign all the Grade that smaller than 8 as `**Null*`
* **`JOIN`** to combine 2 tables `STUDENTS` and `GRADES`
* **`WHERE`** **`MARKS`** and **`BETWEEN`** **`MIN_MARK`** AND **`MAX_MARK`**  in order to assign the Marks within a particular range to a corresponding grade.
* **`ORDER BY`** `GRADE` DESC AND NAME (by default it will be in ASCENDING order) in order to organize the result as the requirements.

``` mysql

SELECT IF(GRADE < 8, NULL, NAME), GRADE, MARKS
    FROM STUDENTS 
    JOIN GRADES
WHERE MARKS BETWEEN MIN_MARK AND MAX_MARK
ORDER BY GRADE DESC, NAME ;

```
