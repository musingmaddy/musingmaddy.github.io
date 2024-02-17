<%*
let title = await tp.system.prompt("Enter Blog Post Title");

// Handle blank titles 
if (!title || title.trim() === "") {
    tp.system.alert("Please enter a valid title.");
    return;
}

// Construct folder path 
const blogFolder = "/content/post/all_posts";

// Check if folder exists 
if (!await tp.file.exists(blogFolder)) {
    tp.system.alert("The specified folder path doesn't exist.");
    return;
}

// Create directory 
const dirPath = `${blogFolder}/${tp.file.normalize_title(title)}`;
if (!await tp.file.exists(dirPath)) {
    await tp.file.create_folder(dirPath);
}

// Create index.md file and populate basic content
const filePath = `${dirPath}/index.md`;
if (!await tp.file.exists(filePath)) {
    await tp.file.create_file(filePath, `---
title: "${title.replace(/"/g, '\\"')}"
date: ${tp.date.now("YYYY-MM-DD")}
tags: []
---

Write your blog post content here.
`);
    tp.file.open(filePath);
} else {
    tp.system.alert("A post with this title already exists.");
}
*%>