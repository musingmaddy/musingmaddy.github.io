<% let title = await dv.ui.prompt("Enter Blog Post Title:")

// Handle blank titles 
if (title === "") {
  dv.ui.showMessage("Please enter a valid title.", { type: "error" });
  return;
}

// Construct folder path 
const blogFolder = "/content/post/all_posts"; 

// Check if folder exists 
if (!await dv.file.exists(blogFolder)) {
  dv.ui.showMessage("The specified folder path doesn't exist.", { type: "error" });
  return;
}

// Create directory 
const dirPath = `${blogFolder}/${title}`;
if (!await dv.file.exists(dirPath)) {
  await dv.file.createFolder(dirPath);
}

// Create index.md file and populate basic content
const filePath = `${dirPath}/index.md`;
await dv.file.create(filePath, `---
title: ${title}
date: ${dv.date.now("YYYY-MM-DD")}
tags: []
---

Write your blog post content here.
`);

dv.file.open(filePath);
%>
