## Let's join

One of the most powerful tools in SQL is the ability to join two or more tables. For this exercise, we'll be using the *txschools* and *txstaff* files.

Look at *txschools*, which has academic ratings and student data for all 762 public schools in Dallas County, Texas.

Each school has a nine-digit code, campus. This is what we call a "unique identifier," and we need it to join tables accurately.

(You'll also notice that each school district has a six-digit code, but because there are multiple schools per district, it's not unique.)

[IMAGE TS1]

Now look at the *txstaff* table, which has data on teacher experience and average teacher and administrator salaries. The school names aren't included, but we do have the campus field, with those 9-digit schools codes.

[image ts2]

First, let's do a simple query with our first table, *txschools*. 
```
SELECT distname, campus, campname, type
FROM txschools
```
[image ts3]

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

[image ts4]




