<%*
let qcFileName = await tp.system.prompt("Note Title");
let titleName = qcFileName;
await tp.file.rename('index');
let baseFolder = "/content/posts/";
let staticFolderPath = "/static/img"; // Ensure this path is correct
let newFolder = `${baseFolder}/${titleName}/`;
await tp.file.move(newFolder + `index`);
let title = titleName;
let slug = titleName.split(" ").join("-").toLowerCase()
let date = tp.date.now("YYYY-MM-DDTHH:mm:ssZ"); // Standard ISO 8601 format with timezone
let lastmod = date;
let description = titleName + ' descriptions';
let cover = '/img/cover.webp'; // Set cover image path


// Static fields updated to "Musing Maddy"
let author = "Musing Maddy";
let avatar = "/img/musingmaddy.webp"; // Updated avatar path
let authorlink = "https://musingmaddy.github.io"; // Assuming you have a similar GitHub page for Musing Maddy
let nolastmod = true;
let draft = false;
let showComments = true;

// Gather tags and categories from the user
let tags = await tp.system.prompt("Enter tags (comma-separated)");
let categories = await tp.system.prompt("Enter categories (comma-separated)");

setTimeout(() => {
  app.fileManager.processFrontMatter(tp.config.target_file, frontmatter => {
    frontmatter["title"] = title;
    frontmatter["slug"] = slug;
    frontmatter["date"] = date;
    frontmatter["lastmod"] = lastmod;
    frontmatter["description"] = description;
    frontmatter["author"] = author;
    frontmatter["avatar"] = avatar;
    frontmatter["authorlink"] = authorlink;
    frontmatter["cover"] = cover; // Cover is now disabled by default
    frontmatter["nolastmod"] = nolastmod;
    frontmatter["draft"] = draft;
    frontmatter["showComments"] = showComments;
    frontmatter["tags"] = tags.split(","); // Split tags into an array
    frontmatter["categories"] = categories.split(","); // Split categories into an array
  });
}, 200);
-%>