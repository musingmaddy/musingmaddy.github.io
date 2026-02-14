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
const cover = (await tp.system.prompt("Cover filename", "cover.webp"))?.trim() || "cover.webp";
const covercaption = (await tp.system.prompt("Cover caption (optional)", ""))?.trim() || "";
const math = ((await tp.system.prompt("Math enabled? (y/n)", "n")) || "n").toLowerCase() === "y";
const draft = ((await tp.system.prompt("Keep as draft? (y/n)", "y")) || "y").toLowerCase() === "y";

await tp.file.rename("index");
await tp.file.move(`${bundleFolder}/index`);

await app.fileManager.processFrontMatter(tp.config.target_file, (fm) => {
  fm.title = title;
  fm.slug = slug;
  fm.date = now;
  fm.lastmod = now;
  fm.description = description;
  fm.cover = cover;
  fm.images = [cover];
  if (covercaption) fm.covercaption = covercaption;
  fm.tags = tags;
  fm.categories = categories;
  fm.nolastmod = true;
  fm.math = math;
  fm.draft = draft;
});

tR += `# ${title}

${description}

<!--more-->

`;
-%>
