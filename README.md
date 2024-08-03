# User-Activity-Analysis-Using-SQL
User Activity Analysis Using SQL
This project focuses 𝐨𝐧 𝐚𝐧𝐚𝐥𝐲𝐳𝐢𝐧𝐠 𝐮𝐬𝐞𝐫 𝐚𝐜𝐭𝐢𝐯𝐢𝐭𝐲 𝐝𝐚𝐭𝐚 𝐟𝐫𝐨𝐦 𝐭𝐰𝐨 𝐭𝐚𝐛𝐥𝐞𝐬, '𝐮𝐬𝐞𝐫𝐬' 𝐚𝐧𝐝 '𝐥𝐨𝐠𝐢𝐧𝐬'. The goal is 𝐭𝐨 𝐩𝐫𝐨𝐯𝐢𝐝𝐞 𝐯𝐚𝐥𝐮𝐚𝐛𝐥𝐞 𝐢𝐧𝐬𝐢𝐠𝐡𝐭𝐬 𝐢𝐧𝐭𝐨 𝐮𝐬𝐞𝐫 𝐞𝐧𝐠𝐚𝐠𝐞𝐦𝐞𝐧𝐭, 𝐚𝐜𝐭𝐢𝐯𝐢𝐭𝐲 𝐩𝐚𝐭𝐭𝐞𝐫𝐧𝐬, 𝐚𝐧𝐝 𝐨𝐯𝐞𝐫𝐚𝐥𝐥 𝐮𝐬𝐚𝐠𝐞 𝐭𝐫𝐞𝐧𝐝𝐬 𝐨𝐯𝐞𝐫 𝐭𝐢𝐦𝐞.

What i have learned :

1- Joins, Set Operations
2- Date functions: DATEDIFF, DATEPART, DATEFROMPARTS, DATEADD
3- CTE(common table expression)
4- GROUP BY, Window functions
5- Recursive CTE
6- Aggregate Window 

𝐐𝐮𝐞𝐬𝐭𝐢𝐨𝐧𝐬:

1. Which users did not log in during the past 5 months? 
return: username.

2. How many users and sessions were there in each quarter, ordered from newest to oldest?
return: first_day_of_quarter, no_of_users, no_of_sessions

3. Which users logged in during January 2024 but did not log in during November 2023?
return: user_id

4. What is the percentage change in sessions from the last quarter?
return: first_day_of_quarter, no_of_users, no_of_sessions, sessions_cnt_prev, session_percent_change

5. Which user had the highest session score each day?
return: login_date, userid, score

6. Which users had a session every single day since their first login?
return: user_id

7. On what dates there were no logins at all?
return: login_date

**SOLUTIONS :**

SELECT * FROM LOGINS

SELECT * FROM USERS

TODAYS 12 JULY 2024 // 5 MONTHS BACK 12 FEB 2024

**1. MANAGEMENT WANTS TO SEE ALL THE USERS THAT DID NOT LOGIN IN THE PAST 5 MONTHS.**

SELECT USER_ID, MAX(LOGIN_TIMESTAMP) -- DATEADD(MONTH, -5, GETDATE())
FROM LOGINS
GROUP BY USER_ID
HAVING MAX(LOGIN_TIMESTAMP) < DATEADD(MONTH, -5, GETDATE())

**2. HOW MANY USERS AND SESSIONS WERE THERE IN EACH QUARTER, ORDERED FROM NEWEST TO OLDEST?_**
RETURN: FIRST_DAY_OF_QUARTER, NO_OF_USERS, NO_OF_SESSIONS 

SELECT DATETRUNC(QUARTER, MIN(LOGIN_TIMESTAMP)) AS [FIRST QUARTER DATE]
 ,COUNT(*) AS [SESSION COUNT]
 ,COUNT (DISTINCT USER_ID) AS [USER COUNT]
FROM LOGINS
GROUP BY DATEPART(QUARTER, LOGIN_TIMESTAMP)

**3. WHICH USERS LOGGED IN DURING JANUARY 2024 BUT DID NOT LOG IN DURING NOVEMBER 2023?**
RETURN: USER_ID 

SELECT *
FROM LOGINS
--WHERE LOGIN_TIMESTAMP BETWEEN '2024-01-01' AND '2024-01-31'
--1 2 3 5
WHERE LOGIN_TIMESTAMP
BETWEEN '2023-11-01' AND '2023-11-30'
--2 4 6 7

SELECT *
FROM LOGINS
WHERE LOGIN_TIMESTAMP
      BETWEEN '2024-01-01' AND '2024-01-31'
      AND USER_ID NOT IN (
                             SELECT USER_ID
                             FROM LOGINS
                             WHERE LOGIN_TIMESTAMP
                             BETWEEN '2023-11-01' AND '2023-11-30'
                         )

**4. WHAT IS THE PERCENTAGE CHANGE IN SESSIONS FROM THE LAST QUARTER?**
   RETURN: FIRST_DAY_OF_QUARTER, NO_OF_USERS, NO_OF_SESSIONS, SESSIONS_CNT_PREV, SESSION_PERCENT_CHANGE

WITH CTE
AS (SELECT DATETRUNC(QUARTER, MIN(LOGIN_TIMESTAMP)) AS [FIRST QUARTER DATE],
           COUNT(*) AS [SESSION COUNT],
           COUNT(DISTINCT USER_ID) AS [USER COUNT]
    FROM LOGINS
    GROUP BY DATEPART(QUARTER, LOGIN_TIMESTAMP)
   --ORDER   BY [FIRST QUARTER DATE]
   )
SELECT *,
       LAG([SESSION COUNT], 1) OVER (ORDER BY [FIRST QUARTER DATE]) AS [PREV SESSION COUNT],
       ([SESSION COUNT] - (LAG([SESSION COUNT], 1) OVER (ORDER BY [FIRST QUARTER DATE]))) * 100.0
       / (LAG([SESSION COUNT], 1) OVER (ORDER BY [FIRST QUARTER DATE]))
FROM CTE


**5. WHICH USER HAD THE HIGHEST SESSION SCORE EACH DAY?**
   RETURN: LOGIN_DATE, USERID, SCORE

WITH CTE
AS (SELECT USER_ID,
           CAST(LOGIN_TIMESTAMP AS DATE) AS [LOGIN DATE],
           SUM(SESSION_SCORE) AS SCORE
    FROM LOGINS
    GROUP BY USER_ID,
             CAST(LOGIN_TIMESTAMP AS DATE)
   )
SELECT * FROM (
    SELECT *, ROW_NUMBER() OVER (PARTITION BY [LOGIN DATE] ORDER BY SCORE DESC) AS RN FROM CTE
) A
WHERE RN = 1


--(6) WHICH USERS HAD A SESSION EVERY SINGLE DAY SINCE THEIR FIRST LOGIN?

--RETURN: USER_ID


SELECT USER_ID,
       MIN(CAST(LOGIN_TIMESTAMP AS DATE)) AS [FIRST LOGIN],
       DATEDIFF(DAY, MIN(CAST(LOGIN_TIMESTAMP AS DATE)), GETDATE()) + 1 AS [LOGIN DAYS],
	   COUNT(DISTINCT CAST(LOGIN_TIMESTAMP AS DATE )) AS [LOGIN DAYS]
FROM LOGINS
GROUP BY USER_ID
HAVING DATEDIFF(DAY, MIN(CAST(LOGIN_TIMESTAMP AS DATE )), GETDATE())+1=COUNT(DISTINCT CAST(LOGIN_TIMESTAMP AS DATE ))
ORDER BY USER_ID


Resource - https://www.youtube.com/watch?v=5kbuhoEw1Xg&t=769s
