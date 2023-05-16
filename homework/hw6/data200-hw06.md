# Data 200: Database Systems and Data Management for Data Analytics


# Homework 6: Inner and Outer Joins in SQL


<font color='red'>**Due Date:** April 9 at 7pm. </font>
---
Enter your name in the markdown cell below.

# Name: Maia Vachon

# Tasks

- **Make sure you have installed SQL Magic before beginning.**
- Review pages 356-368 in Bressoud & White textbook.
- Commit and push your completed `.ipynb` and `.md` files

# Exercises

This homework will make use of the`school.db` database, which is available under `/data` folder on course Github page. I suggest that you review pages 225-227 in the Course Notes to familiarize yourself with the tables in this database.  You may also want to open the `school.db` database in SQLiteStudio to examine the tables as you work on the queries below, or run

<!---
SELECT name FROM sqlite_master
WHERE type='table'
-->

**Download the `school.db` database from Github and save it to the same directory as this notebook.**

**Then, run the code cells below to load the SQL Magic module and connect to the `school.db` database.**


```python
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql



```python
%sql sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
```

<div class="exercise"><b>Exercise 1:</b></div> 

Write a query to obtain the department chairs for Modern Language (`'LANG'`), Philosophy (`'PHIL'`), and Mathematics and Computer Science (`'MATH'`).  Note that you will need to use an inner join of the `departments` and `instructors` tables.  Project the department name, the chair's last name, and the chair's first name.

Below is the output from my solution:<br>
<code>
departmentname	              instructorlast	instructorfirst
Modern Language	             Brown	         Danielle
Mathematics & Computer Science  Bradley       	Betty
Philosophy	                  Singhal	       Aarav
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT dp.departmentname, it.instructorlast, it.instructorfirst 
FROM departments AS dp INNER JOIN instructors AS it
ON dp.departmentid = it.departmentid
WHERE (dp.departmentchair=it.instructorid) AND (dp.departmentid='MATH' OR dp.departmentid='PHIL' OR dp.departmentid='LANG') 
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>departmentname</th>
            <th>instructorlast</th>
            <th>instructorfirst</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Modern Language</td>
            <td>Brown</td>
            <td>Danielle</td>
        </tr>
        <tr>
            <td>Mathematics &amp; Computer Science</td>
            <td>Bradley</td>
            <td>Betty</td>
        </tr>
        <tr>
            <td>Philosophy</td>
            <td>Singhal</td>
            <td>Aarav</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 2:</b></div> 

Write a query to list all of the students who are mathematics (`'MATH'`) majors.  Include the student's last and first name, along with the name of the department.  Order the results by last name, first name.  Limit the results to 6 rows.  Note that you will need to use an inner join on the `students` and `subjects` tables.

Below is the output from my solution:<br>
<code>
studentlast	studentfirst	subjectname
Barnett	    Larry	       Mathematics
Campbell	   Gloria	      Mathematics
Chapman	    Robert	      Mathematics
Colombo	    Giulia	      Mathematics
Davis	      Crystal	     Mathematics
Edwards    	Gary	        Mathematics
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT students.studentlast, students.studentfirst, subjects.subjectname
FROM students INNER JOIN subjects
ON students.studentmajor=subjects.departmentid
WHERE subjects.subjectid='MATH'
ORDER BY students.studentlast
LIMIT 6

```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>studentlast</th>
            <th>studentfirst</th>
            <th>subjectname</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Barnett</td>
            <td>Larry</td>
            <td>Mathematics</td>
        </tr>
        <tr>
            <td>Campbell</td>
            <td>Gloria</td>
            <td>Mathematics</td>
        </tr>
        <tr>
            <td>Chapman</td>
            <td>Robert</td>
            <td>Mathematics</td>
        </tr>
        <tr>
            <td>Colombo</td>
            <td>Giulia</td>
            <td>Mathematics</td>
        </tr>
        <tr>
            <td>Davis</td>
            <td>Crystal</td>
            <td>Mathematics</td>
        </tr>
        <tr>
            <td>Edwards</td>
            <td>Gary</td>
            <td>Mathematics</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 3:</b></div> 

Write a query to list all of the students who are taking Math 123-03 in the fall (this corresponds to `classid=40326`). Please use a `USING` clause on `studentid` (to give you practice with it).  Order the results by last name, first name.  Limit the results to 6 rows.  

Below is the output from my solution:<br>
<code>
studentlast	studentfirst
Brewer	     Scott
Castillo	   Stephen
Chapman	    Robert
Coleman	    Billy
Dunn	       Diane
Fox	        Luke
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT studentlast, studentfirst
FROM students JOIN student_class USING(studentid)
WHERE classid='40326'
ORDER BY studentlast
LIMIT 6
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>studentlast</th>
            <th>studentfirst</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Brewer</td>
            <td>Scott</td>
        </tr>
        <tr>
            <td>Castillo</td>
            <td>Stephen</td>
        </tr>
        <tr>
            <td>Chapman</td>
            <td>Robert</td>
        </tr>
        <tr>
            <td>Coleman</td>
            <td>Billy</td>
        </tr>
        <tr>
            <td>Dunn</td>
            <td>Diane</td>
        </tr>
        <tr>
            <td>Fox</td>
            <td>Luke</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 4:</b></div> 

Write a query to find which instructors (last and first names) are teaching in the spring semester.  Your result should not contain any duplicates.  Order the results by last name, first name.  Limit the results to 10 rows. **Hint:** You will need to use a three-table join of the `classes`, `instructor_class`, and `instructors` tables.

Below is the output from my solution:<br>
<code>
instructorlast	instructorfirst
Aguilar	       Stephen
Anderson	      Philip
Arnaud	        Antoine
Austin	        Stephanie
Bailey	        Jayden
Balasubramanium   Hemant
Balasubramanium   Vishal
Banks	         Carolyn
Banks	         David
Barnes   	     Deborah
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT instructorlast, instructorfirst
FROM classes INNER JOIN instructor_class USING(classid) INNER JOIN instructors USING(instructorid)
GROUP BY instructorid
ORDER BY instructorlast
LIMIT 10
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>instructorlast</th>
            <th>instructorfirst</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Aguilar</td>
            <td>Stephen</td>
        </tr>
        <tr>
            <td>Anderson</td>
            <td>Philip</td>
        </tr>
        <tr>
            <td>Arnaud</td>
            <td>Antoine</td>
        </tr>
        <tr>
            <td>Austin</td>
            <td>Stephanie</td>
        </tr>
        <tr>
            <td>Bailey</td>
            <td>Jayden</td>
        </tr>
        <tr>
            <td>Balasubramanium</td>
            <td>Hemant</td>
        </tr>
        <tr>
            <td>Balasubramanium</td>
            <td>Vishal</td>
        </tr>
        <tr>
            <td>Banks</td>
            <td>Carolyn</td>
        </tr>
        <tr>
            <td>Banks</td>
            <td>David</td>
        </tr>
        <tr>
            <td>Barnes</td>
            <td>Deborah</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 5:</b></div> 

Write a query to list all courses (subject and number) that were *not* taught in either the fall or spring semesters.  **Hint:** Recall that LEFT JOIN can be used to compute *set differences*.  In particular, you will need to use a left join of the `courses` and `classes` tables.  Furthermore, in order to match up the course subject and number, you may want to use `USING (coursesubject, coursenum)`. Order your results by the course subject and limit your results to six rows.

The output from my solution is:<br>
<code>
coursesubject	coursenum
ARTH	         363
ARTH	         452
BIOL	         363
BIOL	         364
BLST	         265
BLST	         361
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT coursesubject, coursenum
FROM courses LEFT JOIN classes USING(coursesubject,coursenum)
WHERE classterm IS NULL
ORDER BY coursesubject
LIMIT 6
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>coursesubject</th>
            <th>coursenum</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ARTH</td>
            <td>363</td>
        </tr>
        <tr>
            <td>ARTH</td>
            <td>452</td>
        </tr>
        <tr>
            <td>BIOL</td>
            <td>363</td>
        </tr>
        <tr>
            <td>BIOL</td>
            <td>364</td>
        </tr>
        <tr>
            <td>BLST</td>
            <td>265</td>
        </tr>
        <tr>
            <td>BLST</td>
            <td>361</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 6:</b></div> 

Write a query to find all English courses (subject, number, and title) that were *not* offered in either the fall or spring semesters.  *This is very similar to the previous exercise*.

The output from my solution is:<br>
<code>
coursesubject	coursenum	coursetitle
ENGL	         340	      Contemporary Drama
ENGL	         349	      Studies in European Lit
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT coursesubject, coursenum, coursetitle
FROM courses LEFT JOIN classes USING(coursesubject,coursenum)
WHERE classterm IS NULL AND coursesubject='ENGL'
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>coursesubject</th>
            <th>coursenum</th>
            <th>coursetitle</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ENGL</td>
            <td>340</td>
            <td>Contemporary Drama</td>
        </tr>
        <tr>
            <td>ENGL</td>
            <td>349</td>
            <td>Studies in European Lit</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 7:</b></div> 

Write a query to list instructors (first and last names) and the *number of students* they taught during the year (both fall and spring semesters).  Only include instructors who were actually teaching.  Please name the new column `total_taught` and order your results from smallest to largest.  Limit the results to 10 rows. **Hint:** You will need to join three different tables and use a GROUP BY to accomplish this task.

The output from my solution is:<br>
<code>
instructorfirst	instructorlast	total_taught
Margaux	        Gillet            1
Lisa	           Fuller	        1
Bobby	          Hall	          1
Harrison	       Gray	          1
Aarav	          Jhadav	        2
Kyle	           Stevens	       4
Rose	           James	         5
Philip	         Anderson	      7
Paul	           Dixon	         7
Michelle	       Young	         8
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT instructorfirst, instructorlast, COUNT(status) AS total_taught
FROM instructors INNER JOIN instructor_class USING(instructorid)
INNER JOIN classes USING(classid) INNER JOIN student_class USING(classid)
WHERE classterm IS NOT NULL
GROUP BY instructorid
ORDER BY total_taught ASC
LIMIT 10
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>instructorfirst</th>
            <th>instructorlast</th>
            <th>total_taught</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Margaux</td>
            <td>Gillet</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Lisa</td>
            <td>Fuller</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Bobby</td>
            <td>Hall</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Harrison</td>
            <td>Gray</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Aarav</td>
            <td>Jhadav</td>
            <td>2</td>
        </tr>
        <tr>
            <td>Kyle</td>
            <td>Stevens</td>
            <td>4</td>
        </tr>
        <tr>
            <td>Rose</td>
            <td>James</td>
            <td>5</td>
        </tr>
        <tr>
            <td>Philip</td>
            <td>Anderson</td>
            <td>7</td>
        </tr>
        <tr>
            <td>Paul</td>
            <td>Dixon</td>
            <td>7</td>
        </tr>
        <tr>
            <td>Michelle</td>
            <td>Young</td>
            <td>8</td>
        </tr>
    </tbody>
</table>



<div class="exercise"><b>Exercise 8:</b></div> 

Note that in the last exercise we learned that the instructor Kyle Stevens only taught a total of 4 students during the year (which is strange--let's investigate).  Write a query to list the student IDs of these four students, along with the course subject, course number, and course title.  **Hint 1:** In my solution, I joined together five different tables (using four INNER JOIN statements)!  **Hint 2:** You may want to first see if you can list the course subject, course number, and student ID.  Then join the fifth table to get the course title.  That is, sometimes it's easier to build your query in stages to make it more manageable.

The output from my solution is (we can see that Kyle only taught senior research and directed studies):<br>
<code>
coursesubject	coursenum	coursetitle	   studentid
BIOL	         452	      Senior Research   61724
BIOL	         451	      Senior Research   61724
BIOL	         362	      Directed Study    62419
BIOL	         362	      Directed Study    62925
</code>


```sql
%%sql

/* Enter your SQL query below */
SELECT coursesubject, coursenum, studentid
FROM instructors INNER JOIN instructor_class USING(instructorid) 
INNER JOIN classes USING(classid) 
INNER JOIN student_class USING(classid)
WHERE instructorid=9260
GROUP BY studentid,classid
```

     * sqlite:////Users/maiavachon/Documents/GitHub/Data200_Spring23/data/school.db
       sqlite:///school.db
    Done.





<table>
    <thead>
        <tr>
            <th>coursesubject</th>
            <th>coursenum</th>
            <th>studentid</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>BIOL</td>
            <td>452</td>
            <td>61724</td>
        </tr>
        <tr>
            <td>BIOL</td>
            <td>451</td>
            <td>61724</td>
        </tr>
        <tr>
            <td>BIOL</td>
            <td>362</td>
            <td>62419</td>
        </tr>
        <tr>
            <td>BIOL</td>
            <td>362</td>
            <td>62925</td>
        </tr>
    </tbody>
</table>




```python

```
