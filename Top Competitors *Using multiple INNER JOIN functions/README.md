# Top Competitors

# Problem

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard!
Write a query to print the respective `**hacker_id**` and `**name**` of hackers who archieved:
    - `Full scores` for more than one challenge.
    - Order your output in `descending order` by the **total number of challenges** in which the hacker earned a `full score`.
    - If more than one hacker received full scores in the **same number of challenges**, then sort them `ascending` `**hacker_id**`.
    
### Input Format

We have 4 tables that contain the needed data for our query.

- **Hackers** table contains: 
    * `hacker_id`
    * `name` of hacker
<br>

![1458526776-67667350b4-ScreenShot2016-03-21at7 45 59AM](https://user-images.githubusercontent.com/70767722/123672023-e64dd600-d80c-11eb-9275-62925e562232.png)
<br>

- **Difficulty** table contains:
    * `difficulty_level`
    * `score` of each level which is the full score that can achieve
    <br>
    
![1458526915-57eb75d9a2-ScreenShot2016-03-21at7 46 09AM](https://user-images.githubusercontent.com/70767722/123672039-e948c680-d80c-11eb-85df-6e0c8eba7c06.png)
<br>

- **Challenges** table contains:
    * `challenge_id` shows the unique challenge id for the corresponding difficulty level
    * `hacker_id`
    * `difficulty_level`
<br>

![1458527032-f9ca650442-ScreenShot2016-03-21at7 46 17AM](https://user-images.githubusercontent.com/70767722/123672051-ef3ea780-d80c-11eb-805a-15e591f5176c.png)
<br>

- **Submissions** table contains:
    * `submission_id` 
    * `hacker_id`
    * `challenge_id`
    * `score` shows the score that each hacker had from the challenge which is different from the `score` of table **Difficulty**
<br>

![1458527077-298f8e922a-ScreenShot2016-03-21at7 46 29AM](https://user-images.githubusercontent.com/70767722/123672075-f5348880-d80c-11eb-9630-282eb40c0743.png)
<br>

### Sample Input

below tables showing the sample of the actual tables look like:

- **Hackers**
<br>

![1458527241-6922b4ad87-ScreenShot2016-03-21at7 47 02AM](https://user-images.githubusercontent.com/70767722/123672100-fb2a6980-d80c-11eb-9783-88070c0388bb.png)
<br>
- **Difficulty** 
<br>

![1458527265-7ad6852a13-ScreenShot2016-03-21at7 46 50AM](https://user-images.githubusercontent.com/70767722/123672130-0087b400-d80d-11eb-8fc5-0afd0dfccaee.png)
<br>

- **Challenges** 
<br>

![1458527285-01e95eb6ec-ScreenShot2016-03-21at7 46 40AM](https://user-images.githubusercontent.com/70767722/123672157-067d9500-d80d-11eb-9dfb-c3eaf425f1e6.png)
<br>

- **Submissions**
<br>

![1458527812-479a74b99f-ScreenShot2016-03-21at8 06 05AM](https://user-images.githubusercontent.com/70767722/123672197-0f6e6680-d80d-11eb-9360-c61705b16a68.png)
<br>

### Sample Output

This is the desired output for the problem:

```
90411 Joe
```

### Explaination for the desired output

- Hacker `86870` got a score of 30 for challenge 71055 with a difficulty level of 2, so `86870` earned a full score for this challenge.

- Hacker `90411` got a score of 30 for challenge 71055 with a difficulty level of 2, so `90411` earned a full score for this challenge.

- Hacker `90411` got a score of 100 for challenge 66730 with a difficulty level of 6, so `90411` earned a full score for this challenge.

According to the problem requirements, only hacker `90411` managed to earn a full score for more than one challenge, so we **print** `**hacker_id**` and name as 2 space-separated values.

# SOLUTION FOR MYSQL

- First of all we want to get below information:
    * `hacker_id` who is got at least **full score** for more than one challenge.
    * `name` of the hackers that correspond the `hacker_id`
- In order to have the above information, we have to **join** these tables together with the **INNER JOIN** function. As below map:

1st JOIN:
**submissions** AS `s` <JOIN (ON c.challenge_id = d.challenge_id)>  **challenges** AS `c`

2nd JOIN:
**difficulty** AS `d` <JOIN (ON c.difficulty_level = d.difficulty_level)> 

3rd JOIN:
**hackers** AS `h` <JOIN (ON h.hacker_id = s.hacker_id)>

- We also need some support clauses to support our query as below:
    * `WHERE` d.score = s.score: because we want to match the hackers' score with the full score from table `Difficulty`.
    * `GROUP BY` h.hacker_id, h.name : because we want to aggregate the results from `hacker_id` and `name`. 
    * `HAVING` COUNT(s.hacker_id) > 1 : because want to find the hackers who did more than 1 challenge.
    * `ORDER BY` COUNT(s.hacker_id) DESC, s.hacker_id ASC : because we want to filter the Hacker who did the most challenges will be in the top and if we have the Hackers who did the same number of challenges, we will filter them by hacker_id in the Ascending order.
    
``` mysql

SELECT h.hacker_id, h.name
    FROM submissions s
    
INNER JOIN challenges c
    ON s.challenge_id = c.challenge_id
INNER JOIN difficulty d
    ON c.difficulty_level = d.difficulty_level
INNER JOIN hackers h
    ON s.hacker_id = h.hacker_id

WHERE d.score = s.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(s.hacker_id) > 1
ORDER BY COUNT(s.hacker_id) DESC, s.hacker_id ASC 
;
```

Link to the [Top Competitors challenge](https://www.hackerrank.com/challenges/full-score/problem)

Supplemental Readings for the problem:
- [GROUP BY](https://www.datacamp.com/community/tutorials/group-by-having-clause-sql?utm_source=adwords_ppc&utm_campaignid=898687156&utm_adgroupid=48947256715&utm_device=c&utm_keyword=&utm_matchtype=b&utm_network=g&utm_adpostion=&utm_creative=332602034349&utm_targetid=aud-299261629574:dsa-429603003980&utm_loc_interest_ms=&utm_loc_physical_ms=9061009&gclid=Cj0KCQjw5uWGBhCTARIsAL70sLJtJ9VL1miWgP8kG6dNfJzXqqlgEOJzQDoK6oH2dvQMOlMXUJO2NbQaAjgFEALw_wcB)
- [INNER JOIN](https://www.sqlshack.com/learn-sql-inner-join-vs-left-join/)
