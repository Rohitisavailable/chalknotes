# chalknotes

A Hermes skill that does two things for any Udemy course:

1. **Extract course info** — the full curriculum (sections, lectures,
   player-accurate numbering), instructor, objectives, and free previews,
   straight from the public course page. No login required.
2. **Generate study notes** — paste a lecture transcript and get a
   hand-drawn-style PDF (Excalidraw scene → PDF) with diagrams, auto-filed
   into per-section folders that mirror the course.

## Why it's reliable

- Uses the **desktop in-app preview browser**, which loads Udemy's public
  pages without tripping the Cloudflare wall that blocks the headless
  automation browser.
- A tested parser (`scripts/parse_curriculum.py`) reproduces the exact
  lecture numbering shown in the Udemy course player (quizzes unnumbered,
  resource downloads counted, the tricky `Preview` marker handled).
- A stdlib-only layout library (`scripts/make_notes.py`) builds valid
  Excalidraw scenes with automatic text wrapping, overlap detection, and
  page-overflow checks, plus a matching HTML preview for PDF export.

## Layout

```
chalknotes/
  SKILL.md                        # the skill instructions
  scripts/
    parse_curriculum.py           # curriculum text -> curriculum.json
    make_notes.py                 # Excalidraw layout library (stdlib)
  examples/
    example_board.py              # copy-per-lecture driver template
  references/
    excalidraw-schema.md          # .excalidraw element/scene field table
```

## Quick start

```bash
# 1. Parse a curriculum block you extracted from the course page
python scripts/parse_curriculum.py raw.txt -o curriculum.json \
    --name "My Course" --url "https://www.udemy.com/course/my-course/"

# 2. Build a demo board to see the template
python examples/example_board.py ./out
#    -> out/00-example-lesson.excalidraw + out/00-example-lesson.html
#    Print the HTML to PDF with headless Chrome (see SKILL.md, Part B step 3).
```

No third-party Python packages are required for the scripts. PDF export
uses any headless Chrome; visual verification uses `pypdfium2` (or the
Hermes `pdf` skill) plus a vision model.

## Requirements

- Hermes with the desktop preview-pane tools (`desktop_preview`,
  `drive_preview`) for the smoothest Udemy extraction, or any real-browser
  tool as a fallback.
- A browser tool for HTML → PDF (`browser_exec` / headless Chrome).
- Python 3.10+ (standard library only).

## License

MIT-0 (MIT No Attribution) — free to use, modify, and redistribute, no attribution required. Same license as the ClawHub listing.
