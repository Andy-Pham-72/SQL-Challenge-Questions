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

- **`Challenges`** table: has `challenge_id` and `hacker_id`

### Sample Input #1

- **Hackers** table:

- **Challenges** table:

### Sample Output #1
```
21283 Angela 6
88255 Patrick 5
96196 Lisa 1
```

### Explanation #1

For sample above, we can get the following details:

- Students **5077** and **62743** both created **4** challenges, but the maximum number of challenges created is **6** so these students are excluded from the result.

### Sample Input #2:


### Sample Output #2:


### Explanation #2:

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


```python

```
