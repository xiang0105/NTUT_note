# 確定每個 SQL 查詢的目的是優化與簡化語法可以提高可讀性和效能。以下是對每個查詢簡化的版本（若有可簡化之處）

---

## 1. 查詢 SC 表格中，學生沒有修 "01" 課程但有修 "02" 課程

優化語法：改用 NOT EXISTS 和 EXISTS，避免 LEFT JOIN。

```sql
SELECT Sname 
FROM Student 
WHERE EXISTS (
    SELECT 1 FROM SC WHERE SC.SId = Student.SId AND CId = '02'
)
AND NOT EXISTS (
    SELECT 1 FROM SC WHERE SC.SId = Student.SId AND CId = '01'
);
```

---

## 2. 查詢在 SC 表格存在成績的學生資訊

使用 IS NOT NULL 替代比較 NULL 的方式。

```sql
SELECT Sname, Score 
FROM Student 
JOIN SC ON Student.SId = SC.SId 
WHERE SC.Score IS NOT NULL;
```

---

## 3. 查詢有成績的學生資訊

與第 2 題結果相同。

```sql
SELECT Sname, Score 
FROM Student 
JOIN SC ON Student.SId = SC.SId 
WHERE SC.Score IS NOT NULL;
```

---

## 4. 查詢「李」姓老師的人數

直接使用 COUNT 和 LIKE，更直觀。

```sql
SELECT COUNT(*) 
FROM Teacher 
WHERE Tname LIKE '李%';
```

---

## 5. 查詢修過「張三」老師授課的同學資訊

改用 IN 替代子查詢結果，語法簡潔。

```sql
SELECT Sname, SId, CId 
FROM Student 
JOIN SC ON Student.SId = SC.SId 
WHERE SC.CId IN (
    SELECT CId FROM Course WHERE TId = (SELECT TId FROM Teacher WHERE Tname = '張三')
);
```

---

## 6. 查詢至少有一門課與學號為 "01" 的同學所修課程相同的同學資訊

直接用子查詢的結果比對課程，無需 LEFT JOIN。

```sql
SELECT Sname, SId 
FROM Student 
WHERE SId IN (
    SELECT SC.SId 
    FROM SC 
    WHERE CId IN (
        SELECT CId 
        FROM SC 
        WHERE SId = '01'
    )
);
```

---

## 7. 查詢兩門以上不及格課程的同學的學號、姓名及其平均成績

使用子查詢和 CASE WHEN，簡化 GROUP BY。

```sql
SELECT SId, Sname, AVG(Score) AS AvgScore 
FROM Student 
JOIN SC ON Student.SId = SC.SId 
GROUP BY SId, Sname 
HAVING SUM(CASE WHEN Score < 60 THEN 1 ELSE 0 END) >= 2;
```

---

## 8. 查詢 "01" 課程分數小於 60，按分數降冪排列的學生資訊

刪除多餘子查詢，直接條件篩選。

```sql
SELECT Sname, Score 
FROM Student 
JOIN SC ON Student.SId = SC.SId 
WHERE CId = '01' AND Score < 60 
ORDER BY Score DESC;
```

---

## 9. 查詢每門課程選修的學生人數

移除不必要的 LEFT JOIN。

```sql
SELECT CId, COUNT(*) AS StudentCount 
FROM SC 
GROUP BY CId 
ORDER BY CId;
```

---

## 10. 查詢只選修兩門課程的學生學號和姓名

刪除不必要的 LEFT JOIN。

```sql
SELECT SId, Sname 
FROM Student 
WHERE SId IN (
    SELECT SId 
    FROM SC 
    GROUP BY SId 
    HAVING COUNT(CId) = 2
);
```

---

這些調整基於以下原則：

1. **去除不必要的 JOIN**：減少對額外表的依賴。
2. **使用 EXISTS/IN 替代子查詢比對**：提高可讀性。
3. **簡化 WHERE 條件**：更直接地表達過濾邏輯。
4. **改用簡單函數 (如 IS NOT NULL、LIKE)**：取代冗長條件。

如果還有其他需求，歡迎再進一步探討！
