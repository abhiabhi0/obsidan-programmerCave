**ROW_NUMBER():** This function assigns a unique sequential number to each row within a window. It's like numbering the rows in order. 

**RANK():** The RANK() function handles tied values by assigning the same rank to them. However, it may skip subsequent ranks, leaving gaps in the sequence. 

**DENSE_RANK():** Similar to RANK(), DENSE_RANK() also handles tied values by assigning the same rank. However, it does not skip ranks, resulting in no gaps in the sequence.

![[Pasted image 20240524104120.png]]

```sql
SELECT artist_name, concert_revenue, 
ROW_NUMBER() OVER (ORDER BY concert_revenue) AS row_num, 
RANK() OVER (ORDER BY concert_revenue) AS rank_num, 
DENSE_RANK() OVER (ORDER BY concert_revenue) AS dense_rank_num 
FROM concerts;
```

![[Pasted image 20240524104207.png]]

**Interpreting the results:**

- **ROW_NUMBER()**: Assigns sequential ranks to each artist based on their concert revenue. BTS gets rank 1, Beyonce rank 2, and so on.
- **RANK()**: Assigns ranks, handling tied values with the same rank. Bruno Mars and Taylor Swift have the same revenue, so they both get rank 4, resulting in a +2 gap before Justin Bieber (rank 6).
- **DENSE_RANK()**: Also handles ties with the same rank but does not skip ranks. After Bruno Mars and Taylor Swift, Justin Bieber follows at rank 5, maintaining a continuous ranking sequence.