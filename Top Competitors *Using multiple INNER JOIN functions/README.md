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

https://s3.amazonaws.com/hr-challenge-images/19504/1458526776-67667350b4-ScreenShot2016-03-21at7.45.59AM.png

- **Difficulty** table contains:
    * `difficulty_level`
    * `score` of each level which is the full score that can achieve
    
https://s3.amazonaws.com/hr-challenge-images/19504/1458526915-57eb75d9a2-ScreenShot2016-03-21at7.46.09AM.png

- **Challenges** table contains:
    * `challenge_id` shows the unique challenge id for the corresponding difficulty level
    * `hacker_id`
    * `difficulty_level`

https://s3.amazonaws.com/hr-challenge-images/19504/1458527032-f9ca650442-ScreenShot2016-03-21at7.46.17AM.png

- **Submissions** table contains:
    * `submission_id` 
    * `hacker_id`
    * `challenge_id`
    * `score` shows the score that each hacker had from the challenge which is different from the `score` of table **Difficulty**

https://s3.amazonaws.com/hr-challenge-images/19504/1458527077-298f8e922a-ScreenShot2016-03-21at7.46.29AM.png

### Sample Input

below tables showing the sample of the actual tables look like:

- **Hackers**
https://s3.amazonaws.com/hr-challenge-images/19504/1458527241-6922b4ad87-ScreenShot2016-03-21at7.47.02AM.png

- **Difficulty** 
https://s3.amazonaws.com/hr-challenge-images/19504/1458527265-7ad6852a13-ScreenShot2016-03-21at7.46.50AM.png

- **Challenges** 
https://s3.amazonaws.com/hr-challenge-images/19504/1458527285-01e95eb6ec-ScreenShot2016-03-21at7.46.40AM.png

- **Submissions**
https://s3.amazonaws.com/hr-challenge-images/19504/1458527812-479a74b99f-ScreenShot2016-03-21at8.06.05AM.png

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



```python

```
