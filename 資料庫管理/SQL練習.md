# SQL 練習 1

---

有一資料庫 School，假設它的 relation 分別代表：學生 (Student)、課程 (Course)、老師 (Teacher)、學生修課資訊 (SC)，此四個 relation 的 schema 如下所示：

- Student(SId, Sname, Sage, Ssex)  
- Course(CId, Cname, TId)  
- Teacher(TId, Tname)  
- SC(SId, CId, Score)

---

請用 SQL 語言寫出下列之查詢。

---

## 1. 查詢 SC 表格中，學生沒有修 "01" 課程但有修 "02" 課程的情況

```sql
SELECT student.Sname, sc.CId 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
WHERE sc.CId = '02';
```

---

## 2. 查詢在 SC 表格存在成績的學生資訊

```sql
SELECT student.Sname, sc.score 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
WHERE sc.score != 'NULL';
```

---

## 3. 查詢有成績的學生資訊

```sql
SELECT student.Sname, sc.score 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
WHERE sc.score != 'NULL';
```

---

## 4. 查詢「李」姓老師的人數

```sql
SELECT COUNT(teacher.Tname) 
FROM teacher 
WHERE LEFT(teacher.Tname, 1) = '李';
```

---

## 5. 查詢修過「張三」老師授課的同學的資訊

```sql
SELECT student.Sname, student.SId, sc.CId 
FROM student 
JOIN sc ON sc.SId = student.SId 
WHERE sc.CId = (
    SELECT course.CId 
    FROM course 
    LEFT JOIN teacher ON course.TId = teacher.TId 
    WHERE teacher.Tname = '張三'
);
```

---

## 6. 查詢至少有一門課與學號為 "01" 同學所修課程相同的同學資訊

```sql
SELECT student.Sname, student.SId, sc.CId 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
WHERE sc.CId IN (
    SELECT sc.CId 
    FROM sc 
    LEFT JOIN student ON sc.SId = student.SId 
    WHERE student.SId = '01'
);
```

---

## 7. 查詢兩門以上不及格課程的同學的學號，姓名及其平均成績

```sql
SELECT student.SId, student.Sname, AVG(sc.score) 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
GROUP BY student.SId 
HAVING COUNT(CASE WHEN sc.score < '60' THEN 1 END) >= 2;
```

---

## 8. 查詢 "01" 課程分數小於 60，按分數降冪排列的學生資訊

```sql
SELECT student.Sname, sc.score 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
WHERE sc.CId = '01' 
  AND student.SId IN (
    SELECT sc.SId 
    FROM sc 
    LEFT JOIN course ON course.CId = sc.CId 
    WHERE sc.score < '60'
) 
ORDER BY sc.score DESC;
```

---

## 9. 查詢每門課程選修的學生人數

```sql
SELECT sc.CId, COUNT(student.Sname) 
FROM sc 
LEFT JOIN student ON sc.SId = student.SId 
GROUP BY sc.CId 
ORDER BY sc.CId;
```

---

## 10. 查詢只選修兩門課程的學生學號和姓名

```sql
SELECT student.SId, student.Sname 
FROM student 
LEFT JOIN sc ON student.SId = sc.SId 
GROUP BY student.SId, student.Sname 
HAVING COUNT(sc.CId) = 2;
```
