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
![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts5.png)

Tip: To save on typing, remember we can type "ORDER BY 7" instead of "ORDER BY txstaff.avg_yrs_exp" because it's the 7th item in our SELECT query.

Maybe we want to see the average years of experience and salary for charter elementary schools versus traditional public ones.
```
SELECT txschools.charter, AVG(txstaff.tchr_sal), AVG(txstaff.avg_yrs_exp)
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
WHERE txschools.type = 'E'
GROUP BY txschools.charter
```
Here's what we get - charter elementary schools have less-experienced, lower-paid teachers on average.

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts6.png)

Does that pattern hold across all school levels? And how many schools in each group are we talking about? To see that, let's do
```
SELECT txschools.charter, txschools.type, AVG(txstaff.tchr_sal), AVG(txstaff.avg_yrs_exp), COUNT(*)
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
GROUP BY 1, 2
ORDER BY 2
```
The answer is yes, the pattern holds.

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts7.png)

## Prettify our results

Remember we can rename our fields something simpler.
```
SELECT txschools.charter, txschools.type, AVG(txstaff.tchr_sal) AS avg_salary, AVG(txstaff.avg_yrs_exp) AS avg_exper, COUNT(*) AS school_count
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
GROUP BY 1, 2
ORDER BY 2
```
And what about those crazy decimals? For that we can use the ROUND statement. The general syntax is ROUND(fieldname, X) where X is the number of decimal places. Try it with average years of experience:
```
SELECT txschools.charter, txschools.type, ROUND(AVG(txstaff.avg_yrs_exp),1)
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
GROUP BY 1, 2
ORDER BY 2
```
![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts9.png)

Next we can get really crazy and also rename that monster field we created.
```
SELECT txschools.charter, txschools.type, ROUND(AVG(txstaff.avg_yrs_exp),1) AS avg_tchr_exper
FROM txschools
JOIN txstaff ON txschools.campus = txstaff.campus
GROUP BY 1, 2
ORDER BY 2
```
Which produces a much-less menacing table:

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts10.png)

## Adding and updating columns

Sometimes we want to add a column to our table. Let's say we want to show Elementary, Middle etc. instead of E, M, etc.
You could just update the existing *type* field, but it's often better to keep your original column and add a new one.

```
ALTER TABLE txschools
ADD COLUMN schtype VARCHAR(12)
```
We tell SQL we're going to change, or alter, the existing table *txschools.* We want to ADD a COLUMN called *schtype*, and it will be a text (instead of number) column of **var**ying **char**acter length, up to 12 long.

Next, we want to UPDATE our new field *schtype* with values based on our original *type* field. Start with elementary schools.
```
UPDATE txschools
SET schtype = 'Elementary'
WHERE type = 'E'
```
To do all school types at once, we do multiple UPDATE queries. Remember to end each one with a semi-colon.
```
UPDATE txschools
SET schtype = 'Elementary'
WHERE type = 'E';
UPDATE txschools
SET schtype = 'Middle'
WHERE type = 'M';
UPDATE txschools
SET schtype = 'High'
WHERE type = 'S';
UPDATE txschools
SET schtype = 'Blended'
WHERE type = 'B'
```
Your updated new column is at the far right.

![alt_text](https://github.com/HackWriter/EWA-sql/blob/HackWriter-patch-1/ts11.png)

Quirky SQL stuff: Note that you ALTER TABLE *tablename* but you don't UPDATE TABLE *tablename,* you just UPDATE *tablename.*

