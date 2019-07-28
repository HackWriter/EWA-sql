## Let's join

One of the most powerful tools in SQL is the ability to join two or more tables. For this exercise, we'll be using the *txschools* and *txstaff* files.

Look at *txschools*, which has academic ratings and student data for all 762 public schools in Dallas County, Texas.

Each school has a nine-digit code, campus. This is what we call a "unique identifier," and we need it to join tables accurately.

(You'll also notice that each school district has a six-digit code, but because there are multiple schools per district, it's not unique.)

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts1.png)

Now look at the *txstaff* table, which has data on teacher experience and average teacher and administrator salaries. The school names aren't included, but we do have the campus field, with those 9-digit schools codes.

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts2.png)

First, let's do a simple query with our first table, *txschools*. 
```
SELECT distname, campus, campname, type
FROM txschools
```
![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts3.png)

Type is the school level - E for elementary, M for middle, S for secondary/high and B is blended grades.

Now let's get the average teacher salary for each school. That's in the tchr_sal field of the *txstaff* table.

When we join two tables, we need to put the table name in our SELECT query. The syntax is tablename.fieldname, like this:

```
SELECT txschools.distname, txschools.campus, txschools.campname, txschools.type, txstaff.tchr_sal
```

We do the FROM like before - in this case from our first table, txschools

```
SELECT txschools.distname, txschools.campus, txschools.campname, txschools.type, txstaff.tchr_sal
FROM txschools
```

Now we use the JOIN statement:

```
SELECT txschools.distname, txschools.campus, txschools.campname, txschools.type, txstaff.tchr_sal
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
```
Check out the results. SQL knew where to put the salary info because we joined the two tables on the unique identifier, the campus field.

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts4.png)

From there, we can add extra fields, conditions, etc. For instance, let's say we want to see the share of low-income students, average teacher salary and average years of teacher experience for all elementary schools. Then order by years of experience.

```
SELECT txschools.distname, txschools.campus, txschools.campname, txschools.type, txschools.low_inc, txstaff.tchr_sal, txstaff.avg_yrs_exp
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
WHERE txschools.type = 'E'
ORDER BY txstaff.avg_yrs_exp
```
[ts5]

Tip: To save on typing, remember we can type "ORDER BY 7" instead of "ORDER BY txstaff.avg_yrs_exp" because it's the 7th item in our SELECT query.

Maybe we want to see the average years of experience and salary for charter elementary schools versus traditional public ones.
```
SELECT txschools.charter, AVG(txstaff.tchr_sal), AVG(txstaff.avg_yrs_exp)
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
WHERE txschools.type = 'E'
GROUP BY txschools.charter
```
Here's what we get - charter schools have less-experienced, lower-paid teachers on average.

[ts6]




