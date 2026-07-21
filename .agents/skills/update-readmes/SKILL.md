# update-readmes

When invoked, scan all asset folders for new files not yet referenced in their respective READMEs, append them in the correct format, and commit+push.

## What it does

### 1. `mine/` → root `README.md`

New non-STL/PDF files are appended before the links section as `<img src="mine/<filename>" width="640">`.
New STL/PDF files are appended to the links section as `[<filename>](mine/<filename>)` on a new line.

### 2. `svg/` → both root `README.md` and `svg/README.md`

New SVGs are appended to the root `README.md` before the links section as `<img src="svg/<filename>" width="640">`.
New SVGs are appended to `svg/README.md` as `<img src="<filename>" width="1920">` on a new line before EOF.

### 3. `md/` → `md/README.md`

New `.txt` files are appended as `` ```txt `` code blocks with the file content inside.

## Conventions

- All images use `<img>` HTML tags, not markdown `![]()` syntax
- Root README width = `640`, SVG README width = `1920`
- STL/PDF files are plain markdown links, not `<img>` tags
- Commit message format: `auto: add <count> new file(s) to READMEs`

## Implementation

Run the following steps:

1. For each folder (`mine/`, `svg/`, `md/`), list files on disk and grep for their references in the corresponding README.
2. For each unreferenced file, append the correct line to the README.
3. `git add` all changed READMEs, `git commit -m "auto: add X new file(s) to READMEs"`, `git push`.

## Pruning (optional)

Files that exist only as references but not on disk (dangling refs) should be removed from READMEs.
