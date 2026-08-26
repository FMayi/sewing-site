# How to add a new project to my sewing site

Written for future me, who has forgotten all of this.

## The basics

| Thing | Where |
|---|---|
| Project folder | `C:\Users\FMayi\Desktop\Sewing-site` |
| Editor | Cursor (open the folder, not a single file) |
| Code online | github.com/FMayi/sewing-site |
| Live site | https://sewing-site-sepia.vercel.app |

**How it works:** I edit files on my computer → commit → push to GitHub → Vercel notices and rebuilds the live site automatically, usually within a minute. I never upload anything by hand.

---

## Before you start: photos

**Take process photos while you sew.** You can write the words any time. You cannot go back and photograph cutting the fabric on a bag that's already finished.

Prop the phone up and snap: pieces cut out, zipper pinned, half assembled, finished bag. Four is plenty.

### Prep every photo before it goes in the folder

1. Open in Windows Photos
2. `...` → **Resize** → about **1200 pixels** on the long side
3. Save as a new file

A phone photo is 3-5MB. Resized it's about 200KB and looks identical on screen. Big photos make the page slow on phone data.

### Filename rules — these matter

- **All lowercase**
- **No spaces** — use hyphens: `tote-bag-step-1.jpg`

Windows thinks `Photo.JPG` and `photo.jpg` are the same file. Vercel's servers run Linux and do not. Get this wrong and the photo works on my computer and is broken on the live site. Lowercase everything and it never comes up.

Put finished files in the `images` folder.

---

## Step 1: make the project page

Easiest route: open an existing project page, **Save As** with a new name, then change what's inside.

Name the file lowercase with hyphens, ending in `.html` — for example `denim-tote.html`. It goes in the main `Sewing-site` folder, **beside** `index.html`, not inside `images`.

Then change **all** of these. Missing one is the usual mistake:

- The `<title>` — shows in the browser tab
- The `<h1>` — the heading on the page
- All five `og:` tags — title, description, image, url, type
- The photos and the words

Full template at the bottom of this file.

## Step 2: add the thumbnail to the home page

Open `index.html`. Inside `<div class="gallery">`, copy an existing block and change the three parts:

```html
<a href="denim-tote.html" title="Denim Tote">
    <img src="images/denim-tote.jpg" alt="Denim Tote" loading="lazy">
    <span>Denim Tote</span>
</a>
```

The `href` must match the filename **exactly**, including capitals. This is the number one cause of a "not found" page.

## Step 3: check it on my own computer

Double-click `index.html` in File Explorer. It opens in Chrome.

- New thumbnail shows up, right size, not stretched
- Clicking it opens the project page
- "Back to the gallery" returns home
- Every photo loads (no broken-image icons)

Fix anything wrong now, before pushing.

## Step 4: commit and push

In Cursor: **Terminal** menu → **New Terminal**. Then:

```
git add .
git commit -m "Add denim tote project"
git push
```

What these mean:

- `git add .` — mark all my changes to be saved
- `git commit -m "..."` — save a snapshot with a note about what changed
- `git push` — send it to GitHub, which triggers Vercel

Silence means success. Errors are loud.

Want to see what changed before saving it? `git status` lists the files, `git diff` shows the actual lines (press `q` to exit).

## Step 5: check the live site

Wait about a minute, then open https://sewing-site-sepia.vercel.app on my **phone**, not just the computer. Click through every page.

Done.

---

## When something breaks

### "404" or "Not found" when clicking a thumbnail

The `href` in `index.html` doesn't match the real filename. Almost always a capital letter. Check the exact name in Cursor's sidebar and compare character by character.

Works on my computer but not live? That's this bug, guaranteed — Windows ignores capitals, the live server doesn't.

### A photo shows a broken icon

Same cause: the `src` doesn't match the file. Check spelling, capitals, and that it's really `.jpg` and not `.jpeg` or `.png`.

### The page looks unstyled — no grid, no spacing

Something in `style.css` is broken. CSS fails **silently**: one bad line and the browser skips it without any error message. Look for a missing `;`, a missing `}`, or a typo like `0..95rem` or `gird` instead of `grid`.

To find it: right-click the broken part in Chrome → **Inspect** → **Styles** panel. Rules that didn't apply show crossed out.

### git push fails with an SSL certificate error

Already fixed once with:

```
git config --global http.sslBackend schannel
```

If it comes back, run it again. **Never** run `http.sslVerify false` — that doesn't fix the problem, it turns off the check that noticed it.

### git push says "permission denied" or "authentication failed"

Git is signed in as the wrong GitHub account. Windows key → **Credential Manager** → **Windows Credentials** → remove entries starting with `git:https://github.com` → push again and sign in as **FMayi**.

### The WhatsApp link preview shows no image

- `og:image` must be a **complete** address starting with `https://sewing-site-sepia.vercel.app/` — a relative path like `images/bag.jpg` will not work
- WhatsApp caches previews hard. Test with `?v=2` on the end of the link (typed into the chat, not into any file), then `?v=3`, and so on
- Check tags at opengraph.xyz first — it doesn't cache
- WhatsApp often refuses tall portrait photos. A landscape image around 1200×630 works best

### The live site doesn't show my changes

1. Did I actually `git push`? Run `git status` — if it lists files, they aren't saved
2. Vercel takes about a minute
3. Hard refresh the browser: **Ctrl+F5**

---

## Full page template

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>PROJECT NAME - Franchesca's Sewing Projects</title>

        <meta property="og:title" content="PROJECT NAME">
        <meta property="og:description" content="One sentence about the project.">
        <meta property="og:image" content="https://sewing-site-sepia.vercel.app/images/FILENAME.jpg">
        <meta property="og:url" content="https://sewing-site-sepia.vercel.app/PAGENAME.html">
        <meta property="og:type" content="article">

        <link rel="stylesheet" href="style.css">
    </head>
    <body>
        <p><a href="index.html">Back to the gallery</a></p>
        <h1>PROJECT NAME</h1>
        <div class="post">
            <p>A few sentences about the project.</p>

            <h2>Materials</h2>
            <ul>
                <li>Pattern: </li>
                <li>Fabric: </li>
                <li>Hardware: </li>
            </ul>

            <h2>Steps</h2>
            <ol>
                <li>Cutting the fabric
                    <p>What I did at this stage.</p>
                </li>
                <li>Sewing the panels
                    <p>What I did at this stage.</p>
                </li>
            </ol>

            <img src="images/FILENAME.jpg" alt="Describe the photo" loading="lazy">
        </div>
    </body>
</html>
```

### Things that are easy to get wrong in that template

- `og:` tags use `property=`. The `charset` and `viewport` tags use `name=`. Not interchangeable.
- Every `content="..."` needs its **closing quote**. Miss one and it silently eats the tag below it.
- Paragraphs inside a numbered list go **inside** the `<li>`, before the `</li>` — not between list items.
- The photo and text must sit **inside** `<div class="post">`, before the `</div>`. Outside it, none of the styling applies.
- `alt` text should describe the photo for someone who can't see it. Not optional, not decoration.
