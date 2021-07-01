# Type of Triangles

# Problem

Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:
* `Equilateral`: It's a triangle with  sides of equal length.
* `Isosceles`: It's a triangle with  sides of equal length.
* `Scalene`: It's a triangle with  sides of differing lengths.
* `Not A Triangle`: The given values of A, B, and C don't form a triangle.

**Input Format**

The TRIANGLES table is described as follows:

![1](https://user-images.githubusercontent.com/70767722/121915449-369f4100-cd01-11eb-8006-64bc77afd77c.png)

Each row in the table denotes the lengths of each of a triangle's three sides.

**Sample Input**

![2](https://user-images.githubusercontent.com/70767722/121915468-3a32c800-cd01-11eb-9d6a-ffd2cbf57b02.png)

**Sample Output**

```
Isosceles
Equilateral
Scalene
Not A Triangle

```

**Explanation**

* Values in the typle **(20,20,23)** form an **Isosceles triangel** because A = B

* Values in the tuple **(20,20,20)** form an **Equilateral triangle** because A = B = C. 

* Values in the tuple **(20,21,22)** form an **Scalene triangel** because A != B != C.

* Values in the tuple **(13,14,30)** cannot form a triangle because the combined value of sides A and B is not larger than the side C.

# <span style="color:blue">SOLUTION FOR MYSQL
</span>

We can use **IF() function** to solve this problem. The syntax is ‘IF(condition, A, B)’. When condition is satisfied, it will execute A otherwise B.
    
``` mysql
SELECT IF(A+B>C AND A+C>B AND B+C>A, 
       IF(A=B AND B=C, 'Equilateral', 
       IF(A=B OR B=C OR A=C, 'Isosceles', 'Scalene')), 'Not A Triangle') 
FROM TRIANGLES;
```
