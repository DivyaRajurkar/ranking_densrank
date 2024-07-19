# ranking_densrank
Certainly! Here's an example of creating a schema for a table, inserting data into it, and using `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` to handle rows with duplicate values. 

We'll create a table named `Scores` to store students' scores, then insert data into it, and finally use the mentioned ranking functions.

### Step 1: Create the Schema

```sql
CREATE TABLE Scores (
    ID INT PRIMARY KEY,
    StudentName VARCHAR(50),
    Subject VARCHAR(50),
    Score INT
);
```

### Step 2: Insert Data

```sql
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (1, 'Alice', 'Math', 85);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (2, 'Bob', 'Math', 90);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (3, 'Charlie', 'Math', 85);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (4, 'David', 'Math', 95);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (5, 'Eva', 'Math', 85);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (6, 'Frank', 'Math', 90);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (7, 'Grace', 'Math', 85);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (8, 'Helen', 'Math', 95);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (9, 'Ivy', 'Math', 85);
INSERT INTO Scores (ID, StudentName, Subject, Score) VALUES (10, 'Jack', 'Math', 80);
```

### Step 3: Query with Ranking Functions

Here, we'll use `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` to rank students based on their scores.

```sql
SELECT 
    StudentName, 
    Subject, 
    Score,
    ROW_NUMBER() OVER (PARTITION BY Subject ORDER BY Score DESC) AS RowNumber,
    RANK() OVER (PARTITION BY Subject ORDER BY Score DESC) AS Rank,
    DENSE_RANK() OVER (PARTITION BY Subject ORDER BY Score DESC) AS DenseRank
FROM Scores;
```

### Explanation of the Ranking Functions

- **ROW_NUMBER()**: Assigns a unique number to each row, starting from 1 for each partition of data. If there are duplicates in the score, it still assigns a unique number.
- **RANK()**: Assigns the same rank to rows with the same score but leaves gaps in the ranking sequence for subsequent rows.
- **DENSE_RANK()**: Similar to `RANK()`, but does not leave gaps in the ranking sequence.

### Example Output

Here's how the output might look:

| StudentName | Subject | Score | RowNumber | Rank | DenseRank |
|-------------|---------|-------|-----------|------|-----------|
| David       | Math    | 95    | 1         | 1    | 1         |
| Helen       | Math    | 95    | 2         | 1    | 1         |
| Bob         | Math    | 90    | 3         | 3    | 2         |
| Frank       | Math    | 90    | 4         | 3    | 2         |
| Alice       | Math    | 85    | 5         | 5    | 3         |
| Charlie     | Math    | 85    | 6         | 5    | 3         |
| Eva         | Math    | 85    | 7         | 5    | 3         |
| Grace       | Math    | 85    | 8         | 5    | 3         |
| Ivy         | Math    | 85    | 9         | 5    | 3         |
| Jack        | Math    | 80    | 10        | 10   | 4         |

- **RowNumber**: Sequentially numbered rows regardless of duplicates.
- **Rank**: Ties have the same rank, but the next rank is skipped.
- **DenseRank**: Ties have the same rank, but the next rank is not skipped.

This example should give you a clear understanding of how to use and interpret `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` in SQL.
