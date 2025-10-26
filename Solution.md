```sql
WITH RECURSIVE split_items AS (SELECT 1 AS pos,
							          id,
							          items,
                                      SPLIT_PART(items,',',1) AS item_split
	                              FROM item
	                              
							   
							   UNION ALL
							   
							   SELECT pos + 1,
							          id,
							          items,
							          SPLIT_PART(items,',',pos+1) AS item_split
							       FROM split_items 
							       WHERE  SPLIT_PART(items,',',pos+1) <> ''),
								   
    lengths AS (SELECT pos,
	       id, 
	       item_split,
		   LENGTH(item_split) AS length
		   FROM split_items
		   ORDER BY id, pos)
		   
SELECT id, 
       STRING_AGG(length::TEXT,',') AS lengths
	 FROM lengths
	 GROUP BY id;
```

| id | lengths |
|----|---------|
| 1	 | 2,3,4 |
| 2	 | 0,1,1,4 |
