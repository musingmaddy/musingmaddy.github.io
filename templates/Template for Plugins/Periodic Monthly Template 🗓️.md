##### {{date:MMMM}} {{date:YYYY}}
##### 📅{{date:LL}}


---
🏷️ Tags : #Note-Monthly📅 #Not-Evergreen-Note🌳 #Not-Refactored♻️ 

---
##### 📆This Month's Weekly Notes :  
```dataview
TABLE file.cday AS "Date"
FROM "JOURNAL📔/Weekly" AND -"zREFERENCE 🧰"
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(31 day)
SORT file.cday DESC LIMIT 150
```

---
##### 📅This Month's Daily Notes :  
```dataview
TABLE file.cday AS "Date"
FROM "JOURNAL📔/Daily" AND -"zREFERENCE 🧰"
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(31 day)
SORT file.cday DESC LIMIT 150
```

---
##### 🗒️Week's Filed Notes :  
```dataview
TABLE file.cday AS "Date"
FROM -"zREFERENCE 🧰" AND -"JOURNAL📔"
WHERE file.name != this.file.name
AND file.cday >= this.file.cday - dur(31 day)
SORT file.cday DESC LIMIT 150
```

---







