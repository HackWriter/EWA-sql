For most of our queries, we'll be working off this basic syntax:

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

