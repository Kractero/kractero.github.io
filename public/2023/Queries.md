<details>
  <summary>Details: Total Trades</summary>

```sql
SELECT COUNT(*) FROM TRADES
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00');

SELECT COUNT(*) FROM TRADES
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
AND PRICE > 0;

SELECT SUM(price) FROM TRADES
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')

SELECT COUNT(DISTINCT buyer)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00');

SELECT COUNT(DISTINCT seller)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00');

SELECT COUNT(DISTINCT trader) AS unique_traders
FROM (
  SELECT DISTINCT buyer AS trader, timestamp FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')

  UNION

  SELECT DISTINCT seller AS trader, timestamp FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
);

SELECT COUNT(*) FROM trades
WHERE price >= 250 AND price <= 499
AND timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')

SELECT COUNT(*) FROM trades
WHERE price >= 500
AND timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
```

</details>

<details>
  <summary>Details: 2023 Rarities</summary>

```sql
SELECT category, COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY category;

SELECT category, AVG(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
AND PRICE > 0
GROUP BY category;

SELECT category, SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
AND PRICE > 0
GROUP BY category;
```

</details>

<details>
  <summary>Details: 2023 Seasons </summary>

```sql
SELECT season, COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY season;

SELECT season, AVG(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
AND PRICE > 0
GROUP BY season;

SELECT season, SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
AND PRICE > 0
GROUP BY season;
```

</details>

<details>
  <summary>Details: 2023 Rarity and Seasons</summary>

```sql
  SELECT category, season, SUM(price)
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
    AND PRICE > 0
  GROUP BY category, season;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Money Moved</summary>

```sql
SELECT
    Trader,
    SUM(Money_Moved) AS Total_Money_Moved
FROM (
    SELECT
        buyer AS Trader,
        SUM(price) AS Money_Moved
    FROM trades
    WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
      AND timestamp < strftime('%s', '2024-01-01 00:00:00')
    GROUP BY buyer

    UNION ALL

    SELECT
        seller AS Trader,
        SUM(price) AS Money_Moved
    FROM trades
    WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
      AND timestamp < strftime('%s', '2024-01-01 00:00:00')
    GROUP BY seller
) AS CombinedResults
GROUP BY Trader
ORDER BY Total_Money_Moved DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Capital Moved</summary>

```sql
SELECT
    buyer,
    SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY buyer
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Capital Moved As Seller</summary>

```sql
SELECT
    seller,
    SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY seller
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Trade Count</summary>

```sql
SELECT nation, COUNT(*) AS trade_count
FROM (
  SELECT buyer AS nation
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  UNION ALL
  SELECT seller AS nation
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
)
GROUP BY nation
ORDER BY trade_count DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Trade Count (>0)</summary>

```sql
SELECT trader, COUNT(*) AS trade_count
FROM (
  SELECT buyer AS trader
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND PRICE > 0
  UNION ALL
  SELECT seller AS trader
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND PRICE > 0
)
GROUP BY trader
ORDER BY trade_count DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Cards Bought</summary>

```sql
SELECT buyer, COUNT(*) as trades_bought
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY buyer
ORDER BY trades_bought DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Cards Bought (>0)</summary>

```sql
SELECT buyer, COUNT(*) as trades_bought
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND PRICE > 0
GROUP BY buyer
ORDER BY trades_bought DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Cards Sold</summary>

```sql
SELECT seller, COUNT(*) as trades_sold
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY seller
ORDER BY trades_sold DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Cards Sold (>0)</summary>

```sql
SELECT seller, COUNT(*) as trades_sold
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND PRICE > 0
GROUP BY seller
ORDER BY trades_sold DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Cards Gifted</summary>

```sql
SELECT seller, COUNT(*) as trades_sold
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
	AND PRICE = 0
GROUP BY seller
ORDER BY trades_sold DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases over 250</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND PRICE > 250
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Sales over 250</summary>

```sql
SELECT seller, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND PRICE > 250
GROUP BY seller
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Season 1 Cards</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND season = 1
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Season 1 Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND season = 1
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Season 2 Cards</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND season = 2
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Season 2 Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND season = 2
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Season 3 Cards</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND season = 3
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Season 3 Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND season = 3
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Common Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'c'
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Uncommon Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'u'
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Rare Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'r'
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Ultra-Rare Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'ur'
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Epic Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'e'
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Legendary Cards</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'l'
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Purchases of Legendary Cards (>0)</summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'l'
        AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Highest number of legendaries gifted (price=0) </summary>

```sql
SELECT seller, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'l'
        AND PRICE = 0
GROUP BY seller
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Highest number of legendaries received (price=0) </summary>

```sql
SELECT buyer, COUNT(*)
FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND category = 'l'
        AND PRICE = 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
  <summary>2023 Top 10 Trades</summary>

```sql
SELECT
    buyer,
    seller,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
AND price > 0
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive Common Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'c'
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive Uncommon Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'u'
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive Rare Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'r'
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive Ultra-Rare Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'ur'
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive Epic Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'e'
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive Legendary Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'l'
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Expensive S1 Legendary Trades</summary>

```sql
SELECT
    seller,
    buyer,
    price,
    strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND price > 0
  AND CATEGORY = 'l'
  AND SEASON = 1
ORDER BY price DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Traded Legendary (>0)</summary>

```sql
SELECT
    season,
    card_name,
        COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND CATEGORY = 'l'
  AND PRICE > 0
GROUP BY season, card_name
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Bank Moved by Legendary</summary>

```sql
SELECT
    season,
    card_name,
        SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND CATEGORY = 'l'
  AND PRICE > 0
GROUP BY season, card_name
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Bank Moved (Jan to July)</summary>

```sql
SELECT
    Trader,
    SUM(Money_Moved) AS Total_Money_Moved
FROM (
    SELECT
        buyer AS Trader,
        SUM(price) AS Money_Moved
    FROM trades
    WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
      AND timestamp < strftime('%s', '2023-07-01 00:00:00')
    GROUP BY buyer

    UNION ALL

    SELECT
        seller AS Trader,
        SUM(price) AS Money_Moved
    FROM trades
    WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
      AND timestamp < strftime('%s', '2023-07-01 00:00:00')
    GROUP BY seller
) AS CombinedResults
GROUP BY Trader
ORDER BY Total_Money_Moved DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Bank Moved As Buyer (Jan to July)</summary>

```sql
SELECT
    buyer,
    SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2023-07-01 00:00:00')
GROUP BY buyer
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Bank Moved As Seller (Jan to July)</summary>

```sql
SELECT
    seller,
    SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2023-07-01 00:00:00')
GROUP BY seller
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Purchases (Jan to July)</summary>

```sql
SELECT
    buyer,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2023-07-01 00:00:00')
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Purchases (Jan to July, >0)</summary>

```sql
SELECT
    buyer,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2023-07-01 00:00:00')
  AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Sales (Jan to July)</summary>

```sql
SELECT
    seller,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2023-07-01 00:00:00')
GROUP BY seller
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Sales (Jan to July, >0)</summary>

```sql
SELECT
    seller,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2023-07-01 00:00:00')
  AND PRICE > 0
GROUP BY seller
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Capital Moved (July to Jan)</summary>

```sql
SELECT
    Trader,
    SUM(Money_Moved) AS Total_Money_Moved
FROM (
    SELECT
        buyer AS Trader,
        SUM(price) AS Money_Moved
    FROM trades
    WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
      AND timestamp < strftime('%s', '2024-01-01 00:00:00')
    GROUP BY buyer

    UNION ALL

    SELECT
        seller AS Trader,
        SUM(price) AS Money_Moved
    FROM trades
    WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
      AND timestamp < strftime('%s', '2024-01-01 00:00:00')
    GROUP BY seller
) AS CombinedResults
GROUP BY Trader
ORDER BY Total_Money_Moved DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Capital Moved in Purchases (July to Jan)</summary>

```sql
SELECT
    buyer,
    SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY buyer
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Capital Moved in Sales (July to Jan)</summary>

```sql
SELECT
    seller,
    SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY seller
ORDER BY SUM(price) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Purchase Count (July to Jan)</summary>

```sql
SELECT
    buyer,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Purchase Count (July to Jan, >0)</summary>

```sql
SELECT
    buyer,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND PRICE > 0
GROUP BY buyer
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Sales Count (July to Jan)</summary>

```sql
SELECT
    seller,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY seller
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Active Traders in Terms of Sales Count (July to Jan, >0)</summary>

```sql
SELECT
    seller,
    COUNT(*)
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND PRICE > 0
GROUP BY seller
ORDER BY COUNT(*) DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Day in which the most capital was moved</summary>

```sql
SELECT
  strftime('%j %H', timestamp, 'unixepoch') AS hour_of_year,
  SUM(price) AS total_money_moved
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY hour_of_year
ORDER BY total_money_moved DESC
LIMIT 1;
```

</details>

<details>
    <summary>2023 Top 5 Trades on Said Day</summary>

```sql
SELECT seller, buyer, price,
        strftime('%Y-%m-%d', timestamp, 'unixepoch') as readable_date,
    season || ' ' || category || ' ' || card_name AS consolidated_info
FROM trades
WHERE strftime('%j %H', timestamp, 'unixepoch') = '343 07'
ORDER BY price DESC
LIMIT 5;
```

</details>

<details>
    <summary>2023 Day in which most trades took place</summary>

```sql
SELECT
  strftime('%j %H', timestamp, 'unixepoch') AS hour_of_year,
  COUNT(*) AS total_trades
FROM trades
WHERE timestamp >= strftime('%s', '2023-07-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY hour_of_year
ORDER BY total_trades DESC
LIMIT 1;

SELECT buyer, COUNT(buyer) AS buyer_count
FROM trades
WHERE strftime('%j %H', timestamp, 'unixepoch') = '026 22'
GROUP BY buyer
ORDER BY buyer_count DESC
LIMIT 1;

SELECT seller, COUNT(seller) AS seller_count
FROM trades
WHERE strftime('%j %H', timestamp, 'unixepoch') = '026 22'
GROUP BY seller
ORDER BY seller_count DESC
LIMIT 1;
```

</details>

<details>
    <summary>2023 Trades on each hour of the day</summary>

```sql
SELECT
    strftime('%H', timestamp, 'unixepoch') AS hour_of_day,
    COUNT(*) AS total_trades,
        SUM(PRICE)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY hour_of_day
ORDER BY hour_of_day;
```

</details>

<details>
    <summary>2023 Trades on each day of the week</summary>

```sql
SELECT
    strftime('%w', timestamp, 'unixepoch') AS day_of_week,
    COUNT(*) AS total_trades,
        SUM(PRICE)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY day_of_week
ORDER BY day_of_week;
```

</details>

<details>
    <summary>2023 Trades on each month of the year</summary>

```sql
SELECT
    strftime('%Y-%m', timestamp, 'unixepoch') AS month,
    COUNT(*) AS total_trades,
        SUM(price)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY month
ORDER BY month;
```

</details>

<details>
    <summary>2023 Monthly Most Active Traders in terms of trade count</summary>

```sql
WITH MonthlyStats AS (
  SELECT
    strftime('%Y-%m', timestamp, 'unixepoch') AS month,
    buyer AS trader,
    COUNT(*) AS total_trades
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND PRICE > 0
  GROUP BY month, trader

  UNION ALL

  SELECT
    strftime('%Y-%m', timestamp, 'unixepoch') AS month,
    seller AS trader,
    COUNT(*) AS total_trades
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND PRICE > 0
  GROUP BY month, trader
)

SELECT
  month,
  trader,
  total_trades
FROM (
  SELECT
    month,
    trader,
    total_trades,
    ROW_NUMBER() OVER (PARTITION BY month ORDER BY total_trades DESC) AS trades_rank
  FROM MonthlyStats
) RankedStats
WHERE trades_rank = 1
ORDER BY month;
```

</details>

<details>
    <summary>2023 Monthly Most Active Traders in terms of capital moved</summary>

```sql
WITH MonthlyStats AS (
  SELECT
    strftime('%Y-%m', timestamp, 'unixepoch') AS month,
    buyer AS trader,
    SUM(price) AS capital_moved
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND PRICE > 0
  GROUP BY month, trader

  UNION ALL

  SELECT
    strftime('%Y-%m', timestamp, 'unixepoch') AS month,
    seller AS trader,
    SUM(price) AS capital_moved
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
  AND PRICE > 0
  GROUP BY month, trader
)

SELECT
  month,
  trader,
  capital_moved
FROM (
  SELECT
    month,
    trader,
    capital_moved,
    ROW_NUMBER() OVER (PARTITION BY month ORDER BY capital_moved DESC) AS money_rank
  FROM MonthlyStats
) RankedStats
WHERE money_rank = 1
ORDER BY month;
```

</details>

<details>
    <summary>2023 Monthly Unique Cards Traded</summary>

```sql
SELECT strftime('%Y-%m', timestamp, 'unixepoch') AS month_year,
       COUNT(DISTINCT card_id) AS unique_cards_traded
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY month_year
ORDER BY month_year;
```

</details>

<details>
    <summary>2023 Top 3 Legendaries Each Month</summary>

```sql
WITH MonthlyTopCards AS (
  SELECT
    strftime('%Y-%m', timestamp, 'unixepoch') AS month_year,
    card_name,
    season,
    SUM(price) AS total_money_moved,
    ROW_NUMBER() OVER (PARTITION BY strftime('%Y-%m', timestamp, 'unixepoch') ORDER BY SUM(price) DESC) AS rn
  FROM trades
  WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
    AND timestamp < strftime('%s', '2024-01-01 00:00:00')
        AND CATEGORY = 'l'
  GROUP BY month_year, card_name, season
)

SELECT month_year, card_name, season, total_money_moved
FROM MonthlyTopCards
WHERE rn <= 3
ORDER BY month_year, total_money_moved DESC;
```

</details>

<details>
    <summary>2023 Most Moved Cards</summary>

```sql
SELECT
    season,
    category,
    card_name,
    COUNT(*) AS card_count
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY season, category, card_id, card_name
ORDER BY card_count DESC
LIMIT 10;
```

</details>

<details>
    <summary>2023 Most Money Moved Per Card</summary>

```sql
SELECT
    season,
    category,
    card_name,
    SUM(price) AS card_count,
        COUNT(*) AS transfers
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
  AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY season, category, card_id, card_name
ORDER BY card_count DESC
LIMIT 10;
```

</details>

<details>
  <summary>Hour of the Day Heatmap</summary>

```sql
SELECT
  strftime('%w', timestamp, 'unixepoch') AS day_of_week,
	strftime('%H', timestamp, 'unixepoch') AS hour_of_day,
  COUNT(*) AS total_trades,
	SUM(PRICE)
FROM trades
WHERE timestamp >= strftime('%s', '2023-01-01 00:00:00')
AND timestamp < strftime('%s', '2024-01-01 00:00:00')
GROUP BY day_of_week, hour_of_day
ORDER BY day_of_week, hour_of_day;
```

</details>
