##### On {{date: dddd, MMMM D, YYYY }} ⛅

---
🏷️Tags : #Note-Weekly📅 #Not-Literature-Note✍️ #Not-Refactored♻️ 

---
##### 🗓️ From Last Wednesday Daily Notes :  
```dataview
TABLE file.ctime AS "Date"
FROM -"zREFERENCE 🧰" AND -"JOURNAL📔"
WHERE file.path != this.file.path 
WHERE file.cday >= this.file.cday - dur(7 days) 
FLATTEN number(dateformat(this.file.cday, "c")) AS Cday 
WHERE (Cday > 3 AND file.cday >= this.file.cday - dur(Cday - 3 + " days")) 
OR (Cday <= 3 AND file.cday >= this.file.cday - dur(4 + Cday + " days")) 
SORT file.ctime DESC LIMIT 150
```

---
##### 🗓️ This Week's Daily Notes :  
```dataview
TABLE file.cday AS "Date"
FROM "JOURNAL📔" AND -"zREFERENCE 🧰"
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(7 day)
SORT file.cday DESC LIMIT 150
```

---
##### 🗒️ This Week's Filed Notes :  
```dataview
TABLE file.cday AS "Date"
FROM -"zREFERENCE 🧰" AND -"JOURNAL📔"
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(7 day)
SORT file.cday DESC LIMIT 150
```

---