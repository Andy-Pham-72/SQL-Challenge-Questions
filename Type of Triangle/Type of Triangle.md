Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:
* `Equilateral`: It's a triangle with  sides of equal length.
* `Isosceles`: It's a triangle with  sides of equal length.
* `Scalene`: It's a triangle with  sides of differing lengths.
* `Not A Triangle`: The given values of A, B, and C don't form a triangle.

**Input Format**

The TRIANGLES table is described as follows:

![1.png](attachment:0df86f35-b858-42b7-9d8f-255d8bf21e5a.png)

Each row in the table denotes the lengths of each of a triangle's three sides.

**Sample Input**

![2.png](attachment:c33a63c9-6c45-499c-a46e-54fd95a4c63c.png)

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

# <span style="color:blue">SOLUTION
</span>


    
``` mysql
SELECT IF(A+B>C AND A+C>B AND B+C>A, 
       IF(A=B AND B=C, 'Equilateral', 
       IF(A=B OR B=C OR A=C, 'Isosceles', 'Scalene')), 'Not A Triangle') FROM TRIANGLES;
```
