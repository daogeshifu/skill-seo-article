# Quality Gate

Run this checklist before returning the outline or article.

## JSON Checks

- Apply these only in `outline` mode:
- Output is valid JSON.
- The JSON object contains only the keys required by the active mode.
- `title` contains exactly 3 non-empty strings.
- No code fences, lead-in text, or trailing explanation.

If JSON shape fails, revise before returning output.

## Outline Checks

- `result` contains only `H2:` and `H3:` lines.
- `H2: Conclusion` is present.
- `H2: FAQs` is present.
- `FAQs` contains 3 to 5 `H3:` question lines.
- No `H4` or deeper structure.
- Headings are concise and keyword-aware without stuffing.

If any outline check fails, revise before returning output.

## Hard Checks

- H1 contains the exact primary keyword or a very close natural variant.
- The keyword appears in the first 100 words.
- The article includes at least 3 H2 headings.
- The body is at least 1500 words unless a shorter target was explicitly requested.
- Keyword density is not below 0.8% and not above 3%.
- In `article` mode, a `.docx` file was actually written.
- In `article` mode, the returned message includes the absolute `.docx` path.
- The Markdown source includes one `#` title and a clear conclusion and FAQ section before conversion.
- No progress update text appears inside the final output payload.

If any hard check fails, revise before returning output.

## Soft Checks

- Keyword density stays near 1% to 2%.
- Intro clearly answers why the topic matters.
- Sections are ordered logically for search intent.
- Conclusion gives a takeaway, CTA, or next step.
- Sentences stay readable and paragraphs are not bloated.
- Brand mentions are natural and not repetitive.
- Links are relevant and not forced.
- The output filename is readable and slug-like when not explicitly provided.

## Anti-Patterns

- Keyword stuffing
- Generic filler paragraphs
- Unsupported statistics
- Claims of rankings or performance guarantees
- Abrupt FAQ appended with no relation to the article
- Broken links or placeholder URLs such as `example.com`
- Returning the full article inline instead of writing the `.docx`
- Mixing progress lines into the final JSON or final file-path response
