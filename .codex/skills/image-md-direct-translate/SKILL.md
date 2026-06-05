---
name: image-md-direct-translate
description: Transcribe product-detail images, screenshots, or image-based documents containing Chinese, Japanese, Korean, English, or mixed-language text into a source-language Markdown file, then create a second Markdown file that keeps English, brand names, model names, dates, numbers, and UI labels unchanged while literally translating non-Korean text into Korean. Use when Codex is asked to extract text from images into .md, preserve original wording, make an additional Korean literal-translation Markdown, or repeat the image-to-source-md-to-Korean-md workflow for non-developer users; also use for Korean requests like "이미지 중국어/일본어를 그대로 md로 만들고 한국어로 직역해줘".
---

# Image Markdown Direct Translate

## Overview

Convert an image-based source into two Markdown files: one faithful source transcription and one Korean literal translation. Keep the workflow conservative: preserve visible wording first, then translate without polishing, summarizing, or localizing beyond what the user requested.

## Workflow

1. Identify the input image or image-derived file path from the user request.
2. Create the source Markdown file first unless it already exists and the user asks only for translation.
3. Create the Korean literal-translation Markdown file second.
4. Verify both files exist and skim key sections for omissions, wrong language handling, and broken Markdown tables.
5. Report the created file paths and note any uncertainty from low-resolution, cropped, or ambiguous text.

## Source Markdown Rules

- Preserve source text exactly as visible whenever possible. Do not translate, paraphrase, normalize, or "fix" source wording in the source Markdown.
- Keep English, numbers, dates, currencies, punctuation, brand names, product names, app labels, and route strings as shown.
- Ignore purely decorative UI elements such as check icons unless they contain text.
- Preserve document order from top to bottom. Use Markdown headings, bullets, and tables to mirror the visual hierarchy.
- For tables, convert visual tables to Markdown tables. Preserve English cell values such as `Check-in`, `Concert`, and `Check-out`.
- If text is unreadable, mark it as `[확인 필요: ...]` and describe the location briefly instead of inventing content.

## Korean Literal Translation Rules

- Translate Chinese, Japanese, or other non-Korean source-language text into Korean as directly as possible.
- Keep English unchanged, including `SOUND CHECK`, `GENERAL`, `TICKETS`, `VIEW VOUCHER`, `CONTACT US`, brand names, product names, venue names in Latin characters, dates, times, numbers, currency amounts, URLs, and app route strings.
- Keep existing Korean unchanged unless it is clearly part of a larger non-Korean phrase that must be rendered coherently.
- Do not make copywriting smoother than the source. Prefer literal wording over natural marketing Korean.
- Preserve Markdown structure, heading levels, bullet order, and table shape from the source Markdown.
- Keep uncertainty markers from the source Markdown and translate only the explanatory Korean if needed.

## File Naming

- For an image `name.png`, create `name.md` for the source transcription and `name_한국어직역.md` for the Korean literal translation unless the user specifies another name.
- For an existing source Markdown `name.md`, create `name_한국어직역.md`.
- Place output beside the input file unless the user specifies an output directory.
- Never overwrite an existing file silently. If the expected output exists, inspect it and either update it intentionally for the current request or choose a clear suffix such as `_2`.

## Image Handling

- Use available OCR when reliable, but always sanity-check visually for long product-detail pages, small fonts, CJK text, tables, and section headings.
- For very tall images, crop or view in segments from top to bottom. Track segment order to avoid duplicated or missing text.
- If local OCR is unavailable or poor for CJK, use visual inspection and write the Markdown directly. Do not present low-quality OCR as final without checking.
- When the task references an image path, use local image inspection tools before editing output files.

## Editing Discipline

- Use `apply_patch` for manual output-file edits.
- Do not modify the original image.
- Keep temporary OCR scripts or image chunks out of the final workspace unless the user asks to keep them.
- Before final response, verify output files with `ls` and skim representative beginning/end sections.
- In the final response, provide clickable paths to the created Markdown files and keep the summary short.
