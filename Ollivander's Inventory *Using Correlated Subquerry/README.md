# Ollivander's Invertory

# Problem scenario

Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.
Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age.
- Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in.
- Sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.

### Input Format

* **`Wands`** table:
    * `id`: the id of the wand
    * `code`: is the code of the wand
    * `coins_needed`: is the total number of gold galleons needed to buy the wand.
    * `power`: denotes the quality of the wand (the higher the power, the better the wand is)
    
|<strong>Column<strong>| <strong>Type<strong>|
|-------------|---------------|
|id| Interger| 
|code| Interger| 
|coins_needed| Interger| 
|power| Interger| 


* **`Wands_Property`** table:
    * `code`: is the code of the wand
    * `age`: is the age of the wand
    * `is_evil`: denotes whether the wand is good for the dark arts. If the value of `is_evil` is 0. it means that the wand is not evil. 

|<strong>Column<strong>| <strong>Type<strong>|
|-------------|---------------|
|code| Interger| 
|age| Interger| 
|is_evil| Interger|
    
    
### Sample Input

- **`Wands`** table:
    
![1458538559-51bf29644e-ScreenShot2016-03-21at10 34 41AM](https://user-images.githubusercontent.com/70767722/123890325-053f8b80-d925-11eb-9e29-05dc2a05b5af.png)
   
- **`Wands_Property`** table:
    
![1458538583-fd514566f9-ScreenShot2016-03-21at10 34 28AM](https://user-images.githubusercontent.com/70767722/123890338-0a043f80-d925-11eb-9d58-879eb90bf598.png)
   
### Sample Output
    
```
9 45 1647 10
12 17 9897 10
1 20 3688 8
15 40 6018 7
19 20 7651 6
11 40 7587 5
```
    
### Explanation:

- the data for the wands of age 45 (code 1):
   
![1458539700-2f319702ab-ScreenShot2016-03-21at11 23 06AM](https://user-images.githubusercontent.com/70767722/123890367-15f00180-d925-11eb-8943-afaac76e41b9.png)
   
    * The minimum number of galleons needed for wand(age = 45, power = 2)  = 6020
    * The minimum number of galleons needed for wand(age = 45, power = 10)  = 1647
    
- The data for wands of age 40 (code 2):
   
![1458539909-ab79f7ff95-ScreenShot2016-03-21at11 23 14AM](https://user-images.githubusercontent.com/70767722/123890387-1dafa600-d925-11eb-8c00-027f491982ad.png)
   
    * The minimum number of galleons needed for wand(age = 40, power = 1)  = 5408
    * The minimum number of galleons needed for wand(age = 40, power = 3)  = 3312
    * The minimum number of galleons needed for wand(age = 40, power = 5)  = 7587
    * The minimum number of galleons needed for wand(age = 40, power = 7)  = 6018

- The data of wands of age 17 (code 5):
   
![1458540132-79fd7b916b-ScreenShot2016-03-21at11 23 34AM](https://user-images.githubusercontent.com/70767722/123890418-2a33fe80-d925-11eb-965c-6b9ac077d507.png)
   
    * The minimum number of galleons needed for wand(age = 17, power = 3)  = 5689
    * The minimum number of galleons needed for wand(age = 17, power = 10) = 9897


# <span style="color:blue">SOLUTION FOR MYSQL</span>
    
- In this problem scenario, we have to create a JOIN (or INNER JOIN) to combine the data from 2 tables `Wands` and `Wands_Property` with the `code` column
- We must use **WHERE** clause to meet the requirements that:
    * The wand should be non-evil that `is_evil` = 0
    * The price should be the cheapest with the corresponding `power` and `age`
=> In order to do that we have to use the **[Correlated Subquery](https://dev.mysql.com/doc/refman/8.0/en/correlated-subqueries.html)** within the **WHERE** clause.
- Finally, we have to sort the data with the `power` and `age` in the Descending order.
    
```mysql
SELECT w.id, p.age, w.coins_needed, w.power
FROM Wands as w

 INNER JOIN Wands_Property as p
 ON w.code = p.code

WHERE p.is_evil = 0
      AND w.coins_needed = (SELECT MIN(coins_needed) 
                            FROM Wands as w1
                            INNER JOIN Wands_property as p1
                            ON w1.code = p1.code
                            WHERE w1.power = w.power
                                AND p1.age = p.age
                            )
ORDER BY w.power DESC, p.age DESC
;
```
   
 -----------------------------------  
   
   [LINK TO THE CHALLENGE](https://www.hackerrank.com/challenges/harry-potter-and-wands/problem)
    
### Suplemental Readings for Correlated Subquerry:
* https://www.javatpoint.com/mysql-subquery
* https://www.w3resource.com/mysql/subqueries/index.php


