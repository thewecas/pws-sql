##Part 1

1. Write a query to fetch the student details whose name ends with the character ”y”
    ```
    select * 
    from student
    where student_name like "%y";
    ```
2. Write a query to get the total number of students in each class 
    ```
    select class ,count(*)
    from student
    group by class;
    ```
3. Write a query to fetch Student name and the percentage of marks (maximum marks in each subject is 100)
    ```
    select s.student_name,sum(sm.mark)/count(sm.student_id) as percentage
    from student s,student_marks sm
    where s.student_id=sm.student_id
    group by sm.student_id;
    ```

4. Write a query to get Student name , Exam name , Subject name and marks
    ```
    select student_name, exam_name, subject_name, mark
    from student_marks sm, exam e, subject s, student st
    where sm.exam_id=e.exam_id AND
    sm.subject_id=s.subject_id AND
    st.student_id=sm.student_id;
    ```
5. Write a query to fetch the youngest student details from Class-3 
    ```
    select * from student
    where class="Class-3"
    and dob in (select min(dob) from student);
    ```
6. Write a query to get the student details who has got highest total marks in Half-Yearly exam 
    ```
    select s.*, sum(sm.mark) as total
    from student s, student_marks sm
    where s.student_id=sm.student_id
    and sm.exam_id = (select exam_id from exam where exam_name="Half-yearly")
    group by s.student_id
    order by total
    limit 1;
    ```
7. Fetch all the student details who are absent for any one of the Quarterly exam
    ```
    select s.* 
    from student s, student_marks sm
    where s.student_id=sm.student_id
    group by sm.student_id
    having count(sm.student_id)<3;
    ```
---
##Part 2

8. Fetch the team name and the corresponding captain name
    ```
    select team_name,player_name 
    from team, player
    where team.captain_id=player.player_id;
    ```
9. Display the player details, by suffixing player name with their team name : Ex:  Dravid-RCB 
    ```
    select concat(t.team_name,"-",p.player_name)
    from team t, player p, team_player tp
    where tp.player_id=p.player_id AND
    tp.team_id=t.team_id;
    ```
10. Fetch the current date and time in the following format: YYYY-MON-DD HH24:MI:SS 
    ```
    select now();
    ```
11. Create a  view Vw_TemPlayer  with team name and player name
    ```
    create view vw_TeamPlayer as
    select team_name , player_name
    from team t,player p, team_player tp
    where p.player_id=tp.player_id AND
    t.team_id=tp.team_id;
    ```
12. Write a SQL to increase player amount by 10% 
    ```
    update player
    set player_amount=player_amount*1.1;
    ```
13. Display all the Player details order by the player_name 
    ```
    select *
    from player
    order by player_name;
    ```
14. Write a statement to add team_location as a new column to Team table
    ```
    alter table team
    add  team_location varchar(20);
    ```
15. Fetch the total player amount of each team 
    ```
    select team_name, sum(player_amount) as total
    from team_player tp 
    left join team t on t.team_id=tp.team_id
    left join player p on p.player_id=tp.player_id
    group by team_name;
    ```
16. Fetch the number of occurrences of S in each player name by using a query
17. Display all the Player details order by team_name, player_name , by displaying the captain name as the first record for each team 
18. Display the highest paid captain name
    ```
    select player_name 
    from player 
    where player_id in (
        select captain_id 
        from team )
    order by player_amount desc 
    limit 1;
    ``` 
