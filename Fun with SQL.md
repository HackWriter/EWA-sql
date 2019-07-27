## Getting started - SELECT, ORDER and WHERE

We'll use the College Scorecard table, *scorecard*.

Our queries will build off this basic syntax:

SELECT column1, column2, etc.
FROM table


Select everything in a table:
```
SELECT *
FROM scorecard
```


Select certain fields:
```
SELECT instnm, city, stabbr, ugds
FROM scorecard
```


Say we want the same information as above, but ordered by number of undergrads:

```
SELECT instnm, city, stabbr, ugds
FROM scorecard
ORDER BY ugds
```

The default is ascending order. To make it descending, add DESC:
```
SELECT instnm, city, stabbr, ugds
FROM scorecard
ORDER BY ugds DESC
```

If we only the top 10 biggest schools, add LIMIT
```
SELECT instnm, city, stabbr, ugds
FROM scorecard
ORDER BY ugds DESC
LIMIT 10
```


Add a condition - the WHERE clause. Let's get colleges from Wisconsin:

```
SELECT instnm, city, stabbr, ugds
FROM scorecard
WHERE stabbr = 'WI'
```


How about two conditions? Here are Wisconsin colleges with at least 5,000 undergrads, ranked in descending order:

```
SELECT instnm, city, stabbr, ugds
FROM scorecard
WHERE stabbr = 'WI' and ugds >= 5000
ORDER BY ugds DESC
```
Note that we put single quotes around WI because it's text, but we don't use quotes with undergrads because it's a number.

Now, what if we want these larger colleges from Wisconsin or Illinois?
```
SELECT instnm, city, stabbr, ugds
FROM scorecard
WHERE (stabbr = 'WI' OR stabbr = 'IL') and ugds >= '5000'
ORDER BY ugds DESC
```
Parentheses make a difference! See what happens when you don't use them.

What if you want only colleges with between 5,000 and 10,000 students? You can get the answer this way
```
SELECT instnm, city, stabbr, ugds
FROM scorecard
WHERE ugds >=5000 AND ugds <=10000
```
or change the last line to 
```
WHERE ugds BETWEEN 5000 AND 10000
```

## Grouping, averaging, counting and more

Which colleges have the highest levels of student debt? Let's look at Florida.

```
SELECT instnm, city, control, grad_debt
FROM scorecard
WHERE stabbr = 'FL'
ORDER BY grad_debt DESC
```
We know from the data dictionary that for control, 1 = public, 2 = private and 3 = for-profit. What is the average debt by school type? For that, we need the AVG (average) and GROUP BY statements.

```
SELECT control, AVG(grad_debt)
FROM scorecard
WHERE stabbr = 'FL'
GROUP BY control
```
Let's also count how many schools are in each group.
```
SELECT control, AVG(grad_debt), COUNT(*)
FROM scorecard
WHERE stabbr = 'FL'
GROUP BY control
```
The 17 for-profit schools have the highest average debt, at $27,558. 
But let's double-check the raw data. Seven of the 79 Florida colleges have no debt data. Let's exclude them from the count.

```
SELECT control, AVG(grad_debt), COUNT(*)
FROM scorecard
WHERE stabbr = 'FL' AND grad_debt 
GROUP BY control
```
There 16 for-profit schools with debt data. Note that the average debt ($27,558) is still the same - SQL ignores NULL values when calculating the average.

We can also see the greatest and smallest debt amounts by college type.
```
SELECT control, MIN(grad_debt), MAX(grad_debt), count(*)
FROM scorecard
WHERE stabbr = 'FL'  AND grad_debt IS NOT NULL
GROUP BY control
```
Let's clean up our field names. You can display new ones with AS.
```
SELECT control, MIN(grad_debt) AS minimum, MAX(grad_debt) AS maximum, count(*)
FROM scorecard
WHERE stabbr = 'FL'  AND grad_debt IS NOT NULL
GROUP BY control
```

