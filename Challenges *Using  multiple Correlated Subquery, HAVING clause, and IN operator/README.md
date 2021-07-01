# Challenges

# Problem

Julia asked her students to create some coding challenges.
Write a query to print the `hacker_id` , `name`, and the **total number of challenges** created by each student.
- Sort your results by the total number of challenges in descending order.
- If more than one student created the same number of challenges, then sort the result by `hacker_id`.
- if more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

### Input Format
The following tables contain challenge data:
- **`Hackers`** table: has `hacker_id` and `name`

![1458521004-cb4c077dd3-ScreenShot2016-03-21at6 06 54AM](https://user-images.githubusercontent.com/70767722/124061250-903f8500-d9fc-11eb-8e49-1e2d1d5824a6.png)

- **`Challenges`** table: has `challenge_id` and `hacker_id`

![1458521079-549341d9ec-ScreenShot2016-03-21at6 07 03AM](https://user-images.githubusercontent.com/70767722/124061261-959ccf80-d9fc-11eb-816d-f5fc45315a17.png)


### Sample Input #1

- **`Hackers`** table:

![1458521384-34c6866dae-ScreenShot2016-03-21at6 07 15AM](https://user-images.githubusercontent.com/70767722/124061282-9c2b4700-d9fc-11eb-864b-601100b2e805.png)

- **`Challenges`** table:

![1458521410-befa8e1cd9-ScreenShot2016-03-21at6 07 25AM](https://user-images.githubusercontent.com/70767722/124061296-a0effb00-d9fc-11eb-86ff-e230351f2966.png)

### Sample Output #1
```
21283 Angela 6
88255 Patrick 5
96196 Lisa 1
```

### Explanation #1

For sample above, we can get the following details:

![1458521677-fd04c384c0-ScreenShot2016-03-21at6 07 38AM](https://user-images.githubusercontent.com/70767722/124061362-bb29d900-d9fc-11eb-98ad-39c884b7d44d.png)


- Students **5077** and **62743** both created **4** challenges, but the maximum number of challenges created is **6** so these students are excluded from the result.

### Sample Input #2:

![1458521469-87036deea3-ScreenShot2016-03-21at6 07 48AM](https://user-images.githubusercontent.com/70767722/124061385-c250e700-d9fc-11eb-95e3-440af3fa615e.png)

### Sample Output #2:

![1458521490-358215cf0b-ScreenShot2016-03-21at6 07 58AM](https://user-images.githubusercontent.com/70767722/124061393-c67d0480-d9fc-11eb-8a9f-972a2bd49dce.png)

### Explanation #2:

![1458521836-24039e7523-ScreenShot2016-03-21at6 08 08AM](https://user-images.githubusercontent.com/70767722/124061408-cbda4f00-d9fc-11eb-913a-02d628191f84.png)

- Students **12299** and **34865** both created **6** challenges because **6** is the maximum number of challenges created, then these students are included in the result.

# <span style="color:blue">SOLUTION FOR MYSQL</span>

```mysql
# these are the columns we want to output 
SELECT c.hacker_id, 
       h.name ,
       COUNT(c.hacker_id) AS c_count

# this is the join we want to output them from 
FROM Hackers AS h
    INNER JOIN Challenges AS c 
    ON c.hacker_id = h.hacker_id

# after they have been grouped by hacker 
GROUP BY c.hacker_id, h.name

# we have to filter the number of challenges according to the requirements 
# HAVING is required (instead of WHERE) for filtering on groups 
HAVING 

# output anyone with a count that is equal to the MAX count of challenges
c_count = 
    # the max count that anyone has 
    (SELECT MAX(temp1.cnt)  # "cnt" from "temp1"
    FROM (SELECT COUNT(hacker_id) AS cnt
         FROM Challenges
         GROUP BY hacker_id
         ORDER BY hacker_id) AS temp1)

# using OR to meet the 2nd requirement
OR c_count IN
    # the set of counts... 
    (SELECT temp2.cnt     # "cnt" from "t"
     FROM (SELECT COUNT(*) AS cnt 
           FROM Challenges
           GROUP BY hacker_id) AS temp2
     # who's group of counts... 
     GROUP BY temp2.cnt
     # has only one element 
     HAVING COUNT(temp2.cnt) = 1) # We only want to have the unique number of challenges completed

# finally, the order the rows should be output
ORDER BY c_count DESC, c.hacker_id
;
```
### Breaking down the query

```mysql
SELECT c.hacker_id, 
       h.name ,
       COUNT(c.hacker_id) AS c_count

# this is the join we want to output them from 
FROM Hackers AS h
    INNER JOIN Challenges AS c 
    ON c.hacker_id = h.hacker_id
```

=> We want to list the `hacker_id`, `name`, `total number of challenges completed` (which should meet the requirements) by **INNER JOIN/ JOIN** the 2 tables `Hackers` and `Challenges` **ON** `hacker_id` rows.

```mysql
HAVING 

# output anyone with a count that is equal to the MAX count of challenges
c_count = 
    # the max count that anyone has 
    (SELECT MAX(temp1.cnt)  # "cnt" from "temp1"
    FROM (SELECT COUNT(hacker_id) AS cnt
         FROM Challenges
         GROUP BY hacker_id
         ORDER BY hacker_id) AS temp1)
```

=> In the first condition of `HAVING` clause, we wanted to find the maximum number of challenges completed by the hackers. **(1st requirement)**
(we are using correlated subquerry for `c_count`)

```mysql
# using OR to meet the 2nd requirement
OR c_count IN
    # the set of counts... 
    (SELECT temp2.cnt     # "cnt" from "t"
     FROM (SELECT COUNT(*) AS cnt 
           FROM Challenges
           GROUP BY hacker_id) AS temp2
     # who's group of counts... 
     GROUP BY temp2.cnt
     # has only one element 
     HAVING COUNT(temp2.cnt) = 1) # We only want to have the unique number of challenges completed

# finally, the order the rows should be output
ORDER BY c_count DESC, c.hacker_id
```

=> In the second condition of `HAVING` clause, we wanted to find the unique number of challenges completed by the hackers. **(2nd requirement)**

**THE INTUITION**: this is all done to meet the requirements of the problem:

- If more than one student created the same number of challenges, only keep the students who uphold the maximum number of challenges.**(1st requirement)**
       
- **AND** if more than one student created the same number of challenges with the count is less than the maximum number of challenges, then exclude those students from the result. **(2nd requirement)**

------------------------

- [LINK to the challenge](https://www.hackerrank.com/challenges/challenges/problem)

- Supplemental Readings:
    * [Differences between `WHERE` and `HAVING` clause in SQL](https://www.java67.com/2019/06/difference-between-where-and-having-in-sql.html)
    * [IN Operator](https://www.w3schools.com/sql/sql_in.asp)
    * [Correlated Subquerry](https://www.w3resource.com/mysql/subqueries/index.php)
