##### On {{date: dddd, MMMM D, YYYY }} ⛅

---
🏷️ Tags : #Note-Weekly📅 #Not-Literature-Note✍️ #Not-Refactored♻️ 

---
##### 🗓️ This Week's Daily Notes :  
```dataview
TABLE dateformat(file.cday, "DDDD") AS Date
FROM "PERIODIC NOTES📔" OR #Note-Daily📅 
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(7 day)
SORT file.cday DESC LIMIT 150
```

---
##### 🗒️ This Week's Filed Notes :  
```dataview
TABLE file.cday AS "Date"
FROM #Child👼 
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(7 day)
SORT file.cday DESC LIMIT 150
```

---
##### 🗒️ Last 2 Week's Weekly Note :  
```dataview
TABLE file.cday AS "Date"
FROM #Note-Weekly📅 AND -"zREFERENCE 🧰"
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(30 day)
SORT file.cday DESC LIMIT 2
```
