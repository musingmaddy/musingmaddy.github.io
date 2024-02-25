<%*
let qcFileName = await tp.system.prompt("Note Title");
let titleName = qcFileName;
await tp.file.rename('index');
let baseFolder = "/content/post/";
let staticFolderPath = "/static/covers"; // Ensure this path is correct
let newFolder = `${baseFolder}/${titleName}/`;
await tp.file.move(newFolder + `index`);
let title = titleName;
let slug = titleName.split(" ").join("-").toLowerCase()
let date = tp.date.now();
let description = titleName + ' descriptions';
// Use the correct static folder path for the image
let image = 'cover.jpg';
let weight = '1';

// Gather tags and categories from the user
let tags = await tp.system.prompt("Enter tags (comma-separated)");
let categories = await tp.system.prompt("Enter categories (comma-separated)");

setTimeout(() => {
  app.fileManager.processFrontMatter(tp.config.target_file, frontmatter => {
    frontmatter["title"] = title;
    frontmatter["slug"] = slug;
    frontmatter["date"] = date;
    frontmatter["description"] = description;
    frontmatter["image"] = image;
    frontmatter["weight"] = 1;
    frontmatter["tags"] = tags.split(","); // Split tags into an array
    frontmatter["categories"] = categories.split(","); // Split categories into an array
  });
}, 200);
-%>