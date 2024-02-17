<%*
let title = await tp.system.prompt("Enter Blog Post Title:");

// Handle blank titles
if (title === "") {
  tp.obsidian.Notice("Please enter a valid title.");
  return;
}

// Construct folder path
const blogFolder = "/content/post/all_posts";

// Check if folder exists
if (!await tp.file.exists(blogFolder)) {
  tp.obsidian.Notice("The specified folder path doesn't exist.");
  return;
}

// Create directory with replaced spaces
const dirPath = `${blogFolder}/${title.replace(/ /g, "-")}`;

// Create index.md file and populate basic content
const filePath = `${dirPath}/index.md`;
await tp.file.create_new(filePath, `---
title: ${title}
date: ${tp.date.now("YYYY-MM-DD")}
tags: []
---

Write your blog post content here.
`);

// Open the new file
tp.file.open(filePath);
%>
