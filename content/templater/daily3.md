<%
let title = await tp.system.prompt("Enter Blog Post Title:");

// Handle blank titles 
if (title === "") {
  tp.system.showMessage("Please enter a valid title.", { type: "error" });
  return;
}

// Construct folder path 
const blogFolder = "/content/post/all_posts"; 

// Check if folder exists 
if (!await tp.file.exists(blogFolder)) {
  tp.system.showMessage("The specified folder path doesn't exist.", { type: "error" });
  return;
}

// Create directory 
const dirPath = `${blogFolder}/${title}`;
if (!await tp.file.exists(dirPath)) {
  await tp.file.createFolder(dirPath);
}

// Create index.md file and populate basic content
const filePath = `${dirPath}/index.md`;
await tp.file.create(filePath, `---
title: ${title}
date: ${tp.date.now("YYYY-MM-DD")}
tags: []
---

Write your blog post content here.
`);

tp.file.open(filePath);
%>
