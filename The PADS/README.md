Generate the following two result sets:

* Query an alphabetically ordered list of all names in **OCCUPATIONS**, immediately followed by the first letter of each profession as a **parenthetical** (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
* Query the number of **ocurrences** of each occupation in **OCCUPATIONS**. Sort the occurrences in **ascending order**, and output them in the following format:

```
There are a total of [occupation_count] [occupation]s.
```

where [occupation_count] is the number of occurrences of an occupation in **OCCUPATIONS** and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered **alphabetically**.

**NOTE**: There will at least two entries in the table for each type of occupation.

**Input Format**

![1](https://user-images.githubusercontent.com/70767722/121921851-65b8b100-cd07-11eb-9f03-1e623611b5a4.png)

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample input**

An **Occupations** table that contains the following records:

![2](https://user-images.githubusercontent.com/70767722/121921877-6b15fb80-cd07-11eb-8740-774ebcb9b26d.png)

**Sample Output**

```
Ashely(P)
Christeen(P)
Jane(A)
Jenny(D)
Julia(A)
Ketty(P)
Maria(A)
Meera(S)
Priya(S)
Samantha(D)
There are a total of 2 doctors.
There are a total of 2 singers.
There are a total of 3 actors.
There are a total of 3 professors.
```

**Explaination**

The results of the first query are formatted to the problem description's specifications.
The results of the second query are ascendingly ordered first by number of names corresponding to each profession ( 2 =< 2 =< 3 =< 3), and then alphabetically by profession (doctor =< singer, and actor =< professor).

# <span style="color:blue">SOLUTION FOR MYSQL
</span>

For this question, we can use **CONCAT() function** to solve the problem. The syntax should be `CONCAT(expression1, expression2, expression3,...)` that can add two or more expressions together, it can return the results from concatinating the arguments.

Then we can use either **SUBSTR()** or **LEFT()** functions to get the first letter of `Occupation`. The syntax is pretty intuitive that can return the very first letter with the length of 1 character from the string.

```mysql

SELECT CONCAT( Name, '(', SUBSTR(Occupation,1,1),')') 
FROM OCCUPATIONS 
    ORDER BY Name;
    
SELECT CONCAT("There are a total of ", COUNT(Occupation), ' ', LOWER(Occupation), 's.')
FROM OCCUPATIONS
    GROUP BY Occupation
    ORDER BY COUNT(Occupation), Occupation;

```
