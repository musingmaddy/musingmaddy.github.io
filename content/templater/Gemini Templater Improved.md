<%
// Prompt for title and handle blank input
let title = await tp.system.prompt("Enter Blog Post Title:");
if (title === "") {
  tp.system.showMessage("Please enter a valid title.", { type: "error" });
  return;
}

// Construct folder path with configurable base directory
const blogFolder = await tp.system.prompt(
  "Enter the base directory for your blog posts (default: /content/post/all_posts):",
  { default: "/content/post/all_posts" }
);

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

// Create index.md file with customizable template content
const filePath = `${dirPath}/index.md`;
const content = `---
title: ${title}
date: ${tp.date.now("YYYY-MM-DD")}

tags: []

---

${await tp.templater.include("../_blog_post_template.md")}

Write your blog post content here.
`;

await tp.file.create(filePath, content);

// Open the file for editing
tp.file.open(filePath);
%>
