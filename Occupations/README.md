Pivot the Occupation column in **OCCUPATIONS** so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, repsectively.

**Note**: Print **NULL** when there are no more names corresponding to an occupation.

**Input Format**

The **OCCUPATIONS** table is described as follows:

![1443816414-2a465532e7-1.png](attachment:81c5925e-1167-4498-9945-5386bc8c3418.png)

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample Input**

![1443817648-1b2b8add45-2.png](attachment:aabf5111-ab4a-4408-ae96-6b4d3903738a.png)

**Sample Output**

```
Jenny    Ashley     Meera  Jane
Samantha Christeen  Priya  Julia
NULL     Ketty      NULL   Maria
```

**Explanation**

The 1st column is an alphabetically ordered list of Doctor names.
The 2nd column is an alphabetically ordered list of Professor names.
The 3rd column is an alphabetically ordered of Single names.
The 4th column is an alphabetically ordered of Actor names.
The empty cell data for columns with less than the maximum number of names per occupation (in this case, the Professor and Actor columns) are filled with **NULL** values.


# <span style="color:blue">SOLUTION FOR MYSQL
</span>



In order to solve the problem, we will use **user-defined variables** to create a new table which we will group by the values produced from it to get the final result.
Let's visualize the table that we are going to create as below:

| <strong>NumLine</strong> |<strong>Doctor</strong> |<strong>Professor</strong> |<strong>Singer</strong> |<strong>Actor</strong> |
|-------------------|-----------------------|------|------|------|
|   1  |   NULL    |   Ashely | NULL| NULL|
|    2 |   NULL    | Christeen   | NULL |NULL |
|     1|  NULL     |   NULL | NULL| Jane|
|  1   | NULL       | NULL   | NULL | NULL |
|   2  |    NULL   |  NULL  |NULL | Julia|
|   3  |    NULL   |  Ketty  |NULL | NULL |
|    3 |  NULL     |  NULL  |NULL | Maria |
|    1 |    NULL   |  NULL  |Meera | NULL|
|    2 |   NULL    |  NULL  |Priya | NULL |
|    2 |   Samantha    | NULL   | NULL | NULL |

* The `NumLine` give us the line of where we will locate the name later after we group the value together.
* We also want to sort/filter the names alphabetically for each occupation so we need to sort **OCCUPATIONS** table by name that we will call as `new_table`.
* After we have the `new_table`, we can use this query 

```mysql
SELECT MIN(Doctor), MIN(Professor), MIN(Singer), MIN(Actor)
FROM new_table
GROUP BY Numline
```

* In order to get the `new_table`, we use **user-defined variables** and **CASE** operator. We will create 4 variables to record the line number of `NumLine` column, one for each occupation. We can use **CASE** to add variables according to occupation.

```mysql
SET @r1=0, @r2=0, @r3 =0, @r4=0;
SELECT MIN(Doctor), MIN(Professor), MIN(Singer), MIN(Actor) FROM
(SELECT CASE Occupation WHEN 'Doctor' THEN @r1:=@r1+1
                        WHEN 'Professor' THEN @r2:=@r2+1
                        WHEN 'Singer' THEN @r3:=@r3+1
                        WHEN 'Actor' THEN @r4:=@r4+1 END
       AS RowLine,
       CASE WHEN Occupation = 'Doctor' THEN Name END AS Doctor,
       CASE WHEN Occupation = 'Professor' THEN Name END AS Professor,
       CASE WHEN Occupation = 'Singer' THEN Name END AS Singer,
       CASE WHEN Occupation = 'Actor' THEN Name END AS Actor
       FROM OCCUPATIONS ORDER BY Name) AS t
GROUP BY RowLine;
```


```python

```
