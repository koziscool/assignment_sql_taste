# assignment_sql_taste
A delicious appetizer of SQL-ey goodness


## Queries

### Example

```
SELECT *
  FROM tutorial.us_housing_units
  WHERE month = 1
```

tutorial.us_housing_units


1. 10 results with information on all columns

SELECT * 
  FROM tutorial.us_housing_units
 LIMIT 10


2. Housing starts in the Midwest

SELECT midwest
  FROM tutorial.us_housing_units

3. All housing starts in December since 1985

SELECT south, west, midwest, northeast 
  FROM tutorial.us_housing_units
 WHERE month_name = 'December' and year >= 1985

 4. All housing starts in the second half of the year since 1990

SELECT south, west, midwest, northeast 
  FROM tutorial.us_housing_units
 WHERE month >= 7 and year >= 1990

5. All rows where housing starts were above 30,000 in the South region

SELECT * 
  FROM tutorial.us_housing_units
  WHERE where south >= 30
 

6. The sum of housing starts across all regions for each row

SELECT south + west + midwest + northeast as column_sum
  FROM tutorial.us_housing_units

7. All rows where the sum of all housing starts is above 70,000 Note: You can't use an alias in a WHERE clause.

SELECT *
  FROM tutorial.us_housing_units
  WHERE south + west + midwest + northeast >= 70

8. All rows where the sum of all housing starts is between 50-80k

SELECT *
  FROM tutorial.us_housing_units
  WHERE south + west + midwest + northeast BETWEEN 50 AND 80 

9. The average of all housing starts across all regions for each row

SELECT (south + west + midwest + northeast)/4 as region_avg
  FROM tutorial.us_housing_units

10. All rows where the housing starts in the South are above the sum of the other three regions

SELECT *
  FROM tutorial.us_housing_units
  WHERE south > (west + midwest + northeast) 

11. The percentage of housing starts that occur in each region since 1990 Note: Use an alias to title the new columns appropriately

SELECT  100 * south / (south + west + midwest + northeast) as south_pcg,
    100 * west / (south + west + midwest + northeast) as west_pcg,
    100 * midwest / (south + west + midwest + northeast) as midwest_pcg,
    100 * northeast / (south + west + midwest + northeast) as northeast_pcg
  FROM tutorial.us_housing_units
  WHERE year >= 1990 

########
tutorial.billboard_top_100_year_end
#########

1. All rows where Elvis Presley had a song on the top 100 charts

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist = 'Elvis Presley'

2. All rows where the artist's name contained "Tony" (not case sensitive)

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist ILIKE '%tony%'

3. All rows where the song title contained the word "love" in any way

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE song_name ILIKE '%love%'

4. All rows where the artist's name begins with the letter "A"

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist LIKE 'A%'

5. The top 3 songs from each year between 1960-1969

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE year_rank BETWEEN 1 AND 3
  AND year BETWEEN 1960 AND 1969

6. All rows where either Elvis Presley, The Rolling Stones, or Van Halen were the artist

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist IN ('Elvis Presley', 'Rolling Stones', 'Van Halen')

# this db table contains entries for 'Rolling Stones' but not 'The Rolling Stones' so I used the former

7. Which artist has had the most appearances on the top 100 list?
SELECT artist, COUNT (*)
  FROM tutorial.billboard_top_100_year_end
  GROUP BY artist
  ORDER BY COUNT DESC

8. Which artist has had the most #1 hits? How many?

SELECT artist, COUNT (*)
  FROM tutorial.billboard_top_100_year_end
  WHERE year_rank = 1
  GROUP BY artist
  ORDER BY COUNT DESC

9. All rows from 1970 where the songs were ranked 10-20th

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE year_rank BETWEEN 10 AND 20
  AND year = 1970

10. All rows from the 1990's where Madonna was not ranked 10-100th

SELECT artist, year, year_rank, song_name
  FROM tutorial.billboard_top_100_year_end
  WHERE year BETWEEN 1990 AND 1999
  AND (artist != 'Madonna'
  OR year_rank < 10)

# I think this is correct as the prompt was stated but 
# maybe too literal-minded.  Couldn't determine other intent if something else

11. All rows from 1985 which do not include Madonna or Phil Collins in the group.

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE "group" NOT ILIKE '%madonna%'
  AND "group" NOT ILIKE '%phil collins%'
  AND year = 1985

12. All number 1 songs in the data set.

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE year_rank = 1

13. All rows where the artist is not listed

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist IS NULL

14. All of Madonna's top 100 hits ordered by their ranking (1 to 100)

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist ILIKE '%madonna%'
  ORDER BY year_rank

15. All of Madonna's top 100 hits ordered by their ranking within each year

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE artist ILIKE '%madonna%'
  ORDER BY year, year_rank

16. Every number 1 song since 1990 followed by every number 2 song since 1990 and number 3 song since 1990. (Hint: Multiple ordering)

SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE year >= 1990 
  AND year_rank IN (1, 2, 3)
  ORDER BY year_rank, year

#########
Intermediate
tutorial.billboard_top_100_year_end
########



1. What is the highest position ever reached by Phil Collins?

SELECT MIN(year_rank)
  FROM tutorial.billboard_top_100_year_end
  WHERE artist = 'Phil Collins'

2. What is the average position reached by Michael Jackson?

SELECT AVG(year_rank)
  FROM tutorial.billboard_top_100_year_end
  WHERE artist = 'Michael Jackson'

3. Madonna's average position when she actually reached the top 10

SELECT AVG(year_rank)
  FROM tutorial.billboard_top_100_year_end
  WHERE artist = 'Madonna'
  AND year_rank <= 10

4. List the top 10 artists based on their number of appearances on this list (and what that number is) since 1985

SELECT artist, COUNT(1)
  FROM tutorial.billboard_top_100_year_end
  WHERE year >= 1985
  GROUP BY artist
  ORDER BY COUNT DESC
  LIMIT 10


5. The total count of top 10 hits written by either Elvis, Madonna, the Beatles, or Elton John

SELECT artist, COUNT(1)
  FROM tutorial.billboard_top_100_year_end
  WHERE year_rank <= 10
  AND artist IN ('Elvis Presley', 'Madonna', 'Beatles', 'Elton John')
  GROUP BY artist
  ORDER BY COUNT DESC

##### db has 'Beatles' but not 'The Beatles'




aapl_historical_stock_price

1. The count of days when Apple traded in a range that was larger than $5

SELECT COUNT(1)
  FROM tutorial.aapl_historical_stock_price 
  WHERE high - low > 5

2. The highest daily trading range that Apple stock achieved in 2012

SELECT MAX(high - low)
  FROM tutorial.aapl_historical_stock_price 
  WHERE year = 2012

3. The average price for all days when Apple's trading volume exceeded 10,000,000 shares.

SELECT AVG(close)
  FROM tutorial.aapl_historical_stock_price 
  WHERE volume > 10000000

4. The number of trading days in each month of the year 2012

SELECT year, month, COUNT(1)
  FROM tutorial.aapl_historical_stock_price
  WHERE year = 2012
  GROUP BY year, month

5. The maximum price Apple traded at during each year of the data set

SELECT year, MAX(close)
  FROM tutorial.aapl_historical_stock_price 
  GROUP BY year

6. The average price and trading volume on each calendar month across the full data set (this should return only 12 rows, one for each month!)

SELECT month, AVG(close), AVG (volume)
  FROM tutorial.aapl_historical_stock_price 
  GROUP BY month

7. The average price for each month and year of data since 2008, ordered by years descending and months ascending.

SELECT year, month, AVG(close)
  FROM tutorial.aapl_historical_stock_price
  WHERE year > 2008
  GROUP by year, month
  ORDER BY year DESC, month

8. The average price of days with a trading volume above 25,000,000 shares (just 1 row)

SELECT AVG(close)
  FROM tutorial.aapl_historical_stock_price
  WHERE volume > 25000000

9. The average price on all months with an average daily trading volume above 10,000,000 shares.

SELECT year, month, AVG(close)
  FROM tutorial.aapl_historical_stock_price
  GROUP BY year, month
  HAVING AVG(volume) > 10000000

10. The lowest and highest prices that Apple stock achieved between 2005 and 2010 (inclusive).

SELECT MAX(high), MIN(low)
  FROM tutorial.aapl_historical_stock_price
  WHERE year BETWEEN 2005 AND 2010

11. The average daily trading range in months where the stock moved more than $25 (open of month to close of month)

SELECT year, month, AVG(high - low)
    HAVING MAX(high) - MIN(low) > 25
    GROUP year, month

12. All months in the second half of the year where average daily trading volume was below 10,000,000.

SELECT year, month
    WHERE month >= 7
    HAVING AVG(volume) < 10000000
    GROUP year, month

13. A list of all calendar months by average daily trading volume (so only 12 rows), sorted from highest to lowest.

SELECT month, AVG(volume) as avg
  FROM tutorial.aapl_historical_stock_price
  GROUP BY month
  ORDER BY avg DESC
  
14. Count how many unique months there are in the data set (should equal 12)

SELECT COUNT(DISTINCT month)
  FROM tutorial.aapl_historical_stock_price

15. Count how many unique years there are in the data set

SELECT COUNT(DISTINCT year)
  FROM tutorial.aapl_historical_stock_price

16. Count how many unique prices there are in the data set

SELECT COUNT(DISTINCT close)
  FROM tutorial.aapl_historical_stock_price

17. Return the percentage of unique "open" prices compared to all open prices in the data set

SELECT 100 * ((COUNT(DISTINCT open) / (COUNT(open) AS float))
  FROM tutorial.aapl_historical_stock_price        

18. A listing of all months by their average daily trading volume and a classification that puts this volume into the following categories: "Low" = below 10MM, "Medium" = 10-25 MM, "High" = above 25MM

SELECT year, month, AVG(volume),
    CASE WHEN AVG(volume) < 10000000 THEN 'Low'
    CASE WHEN AVG(volume)  BETWEEN 10000000 AND 25000000 THEN 'Med'
    CASE WHEN AVG(volume)  > 25000000 THEN 'Hi'
        END AS trd_volume
  FROM tutorial.aapl_historical_stock_price        
  GROUP BY year, month
  ORDER BY trd_volume

19. A listing of average monthly price plus which quarter of the year they are in (e.g. "Q2" or "Q4").
This same listing filtered for only Q4 (use the new column not the months explicitly as part of this filtering).


SELECT year, month, AVG(volume),
    CASE WHEN month BETWEEN 1 AND 3 THEN 'Q1'
    CASE WHEN month BETWEEN 4 AND 6 THEN 'Q2'
    CASE WHEN month BETWEEN 7 AND 9 THEN 'Q3'
    ELSE 'Q4'
      END as year_quarter

    FROM tutorial.aapl_historical_stock_price        
    GROUP BY year, month
    ORDER BY year


SELECT * from ( 
    SELECT year, month, AVG(volume),
        CASE WHEN month BETWEEN 10 AND 12 THEN 'Q4'
          END as year_quarter

        FROM tutorial.aapl_historical_stock_price        
        GROUP BY year, month
        ORDER BY year
        )
    where year_quarter = 'Q4'





