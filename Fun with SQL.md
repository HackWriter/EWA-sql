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
