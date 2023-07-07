<%* 
let year = await tp.system.prompt('Year',tp.date.now('YYYY')); 
let mon = await tp.system.prompt('Month',tp.date.now('M')); 
tR += '# '+ tp.date.now('YYYY MMMM',0,`${year}/${mon}/1`,'YYYY/M/D') + ' Trade Diary 📗\n\n'; 
let curMon = mon; let day = 0; 
while (mon == curMon) 
{ 
let curDay = tp.date.now('dddd, Do MMMM YYYY',day,`${year}/${mon}/1`,'YYYY/M/D'); 
let curDate = tp.date.now('YY-MM-DD',day,`${year}/${mon}/1`,'YYYY/M/D'); 
tR += `# ${curDate}`+ ' Title\n\n';
tR += `## 📆 ${curDay}\n\n`; 
tR += '### Title Here\n\n'; 
tR += '#### 🕛 Pre Market\n'; 
tR += ' - \n\n'; 
tR += '#### 🕘 Market Hours\n'; 
tR += ' - \n\n'; 
tR += '#### 🕞 Post Market\n'; 
tR += ' - \n\n'; 
tR += '🏷️Tags : #Investments💷/Trade-Diary📗 \n\n'; 
tR += '--- \n'; 
day++; curMon = tp.date.now('M',day,`${year}/${mon}/1`,'YYYY/M/D'); 
} 
%>

