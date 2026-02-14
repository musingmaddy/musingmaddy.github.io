<%*
const parseCSV = (s) =>
  (s ?? "").split(",").map(v => v.trim()).filter(Boolean);

const slugify = (s) =>
  s.normalize("NFKD")
    .replace(/[\u0300-\u036f]/g, "")
    .toLowerCase()
    .trim()
    .replace(/['"`]/g, "")
    .replace(/[^a-z0-9]+/g, "-")
    .replace(/^-+|-+$/g, "");

const title = (await tp.system.prompt("Post title"))?.trim();
if (!title) { new Notice("Post title is required."); return; }

const slug = slugify(title);
const baseFolder = "content/posts";
const bundleFolder = `${baseFolder}/${slug}`;

if (app.vault.getAbstractFileByPath(bundleFolder)) {
  new Notice(`Post folder already exists: ${bundleFolder}`);
  return;
}

const now = tp.date.now("YYYY-MM-DDTHH:mm:ssZ");
const description = ((await tp.system.prompt("Short description", title)) || title).trim();
const tags = parseCSV(await tp.system.prompt("Tags (comma-separated)"));
const categories = parseCSV(await tp.system.prompt("Categories (comma-separated)"));

// Defaults
const author = "Maddy Meraki";
const avatar = "/img/musingmaddy.webp";
const draft = false;
const nolastmod = true;

// Cover dropdown from static/img/covers/
const coverDir = "static/img/covers/";
const coverOptions = app.vault.getFiles()
  .filter(f => f.path.startsWith(coverDir))
  .map(f => "/" + f.path.replace(/^static\//, "")) // static/img/... -> /img/...
  .sort((a, b) => a.localeCompare(b));

let cover = "/img/covers/cover.webp";
if (coverOptions.length > 0) {
  cover = await tp.system.suggester(
    coverOptions.map(p => p.replace("/img/covers/", "")), // display names
    coverOptions,                                          // actual values
    false,
    "Select cover image"
  );
  if (!cover) cover = coverOptions[0];
} else {
  cover = ((await tp.system.prompt("Cover path", "/img/covers/cover.webp")) || "/img/covers/cover.webp").trim();
}

await tp.file.rename("index");
await tp.file.move(`${bundleFolder}/index`);

await app.fileManager.processFrontMatter(tp.config.target_file, (fm) => {
  fm.title = title;
  fm.slug = slug;
  fm.date = now;
  fm.lastmod = now;
  fm.description = description;
  fm.author = author;
  fm.avatar = avatar;
  fm.cover = cover;
  fm.images = [cover];
  fm.tags = tags;
  fm.categories = categories;
  fm.nolastmod = nolastmod;
  fm.draft = draft;
});

tR += `# ${title}

${description}

<!--more-->

`;
-%>