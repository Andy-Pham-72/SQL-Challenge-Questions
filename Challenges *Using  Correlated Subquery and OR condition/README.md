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
SELECT c.hacker_id, h.name ,COUNT(c.hacker_id) AS c_count

# this is the join we want to output them from 
FROM Hackers AS h
    INNER JOIN Challenges AS c ON c.hacker_id = h.hacker_id

# after they have been grouped by hacker 
GROUP BY c.hacker_id, h.name

# but we want to be selective about which hackers we output 
# having is required (instead of where) for filtering on groups 
HAVING 

# output anyone with a count that is equal to... 
c_count = 
    # the max count that anyone has 
    (SELECT MAX(temp1.cnt)
    FROM (SELECT COUNT(hacker_id) as cnt
         FROM Challenges
         GROUP BY hacker_id
         ORDER BY hacker_id) temp1)

# or anyone who's count is in... 
OR c_count IN
    # the set of counts... 
    (SELECT t.cnt
     FROM (SELECT COUNT(*) AS cnt 
           FROM challenges
           GROUP BY hacker_id) t
     # who's group of counts... 
     GROUP BY t.cnt
     # has only one element 
     HAVING COUNT(t.cnt) = 1)

# finally, the order the rows should be output
ORDER BY c_count DESC, c.hacker_id
;
```


- [LINK to the challenge](https://www.hackerrank.com/challenges/challenges/problem)

- Supplemetal Readings:
    * [Differences between `WHERE` and `HAVING` clause in SQL](https://www.java67.com/2019/06/difference-between-where-and-having-in-sql.html)
