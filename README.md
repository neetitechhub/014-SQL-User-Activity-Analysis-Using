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

Resource - https://www.youtube.com/watch?v=5kbuhoEw1Xg&t=769s
