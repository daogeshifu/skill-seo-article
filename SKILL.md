---
name: seo-article
description: Generate SEO outlines or full long-form articles for independent websites from a keyword, brand, and optional references. Use when the user asks for H2/H3 planning, blog writing, HTML article output, or JSON payloads for content pipelines.
user-invocable: true
---

# SEO Article

Produce one SEO deliverable per request. Infer the mode unless the user explicitly sets it. Ask a follow-up only when the primary keyword is missing.

## Load References

- Read [references/output-contract.md](references/output-contract.md) before finalizing the response.
- Read [references/quality-gate.md](references/quality-gate.md) before returning the response.
- Read [references/outline-mode.md](references/outline-mode.md) only for `outline` mode.
- Read [references/article-mode.md](references/article-mode.md) only for `article` mode.

## Inputs

Accept either free text or structured key-value input.

Supported fields:

- `mode` or `模式` (optional: `outline` or `article`)
- `keyword` or `关键词` (required)
- `brand` or `品牌` (optional)
- `topic` or `主题` (optional)
- `language` or `语言` (optional)
- `target_country` or `目标地区` (optional)
- `audience` or `受众` (optional)
- `reference_urls` or `参考链接` (optional list)
- `site_domain` or `站点域名` (optional)
- `internal_pages` or `内链页面` (optional list)
- `recommend_links` or `推荐链接` (optional list)
- `link_form` or `链接形式` (optional)
- `exclude_brands` or `排除品牌` (optional list)
- `search_intent` or `搜索意图` (optional)

Defaults:

- `mode`: infer from the request; use `outline` when the user asks for a structure or outline, otherwise use `article`
- `brand`: `none`
- `topic`: a practical article angle centered on the keyword
- `language`: match the user's language
- `target_country`: `global`
- `audience`: inferred from keyword intent
- `search_intent`: inferred from the keyword if not provided
- `link_form`: infer from user context; prefer natural inline anchors
- `exclude_brands`: empty

If `keyword` is missing, ask for it and stop.

## Workflow

Work in one pass:

1. Infer intent and audience.
2. Build an outline and long-tail keyword set.
3. Branch by `mode`.
4. Self-check against the quality gate.
5. Revise up to two times if needed.
6. Package the final output in the required JSON shape.

Keep the brief and checklist internal unless the user explicitly asks to see them.

## Planning Rules

Before writing, derive:

- primary search intent
- target audience and pain points
- one H1 direction
- 3-6 H2 sections
- H3 opportunities where useful
- at least 5 long-tail keywords
- a brand angle if a brand is provided

If the topic is broader than the keyword, anchor the article around the keyword and use the topic as the promise or framing.

## Mode Selection

Use `outline` mode when the user asks for:

- an outline
- a structure
- headings only
- H2/H3 planning
- content brief scaffolding

Use `article` mode when the user asks for:

- a blog post
- a full article
- HTML output
- publish-ready content
- a complete SEO article

## Shared Rules

- Return JSON only. Do not wrap it in code fences.
- Use exactly two top-level keys: `title` and `result`.
- `title` must contain exactly 3 SEO-friendly title options.
- Keep planning notes, checklists, and revision notes internal.
- Do not invent citations, rankings, statistics, links, or image URLs.
- Exclude or avoid promoting brands listed in `exclude_brands`.

## Article Rules

In `article` mode:

- Keep total length above 1500 words unless the user explicitly asks for a shorter article.
- Use exactly one H1 and include the primary keyword in it.
- Mention the primary keyword in the first paragraph and closing section.
- Include the primary keyword in at least one H2.
- Use at least 3 H2 headings.
- Use H3 only when they clarify structure.
- Include an introduction, body, conclusion, and FAQ.
- Integrate the brand naturally. If `brand` is `none`, keep the article neutral.
- If `site_domain` is provided, mention it naturally near the beginning and the ending.
- Prefer practical explanation, comparison, step-by-step guidance, or buyer education based on intent.
- When adding images, use real web URLs only. If you cannot verify accessible image URLs, omit images instead of inventing them.
- When adding links, use `recommend_links` first if provided and respect `link_form`.

## Outline Rules

In `outline` mode:

- Output JSON only, with no code fences and no prose outside the JSON object.
- Use exactly two top-level keys: `title` and `result`.
- `title` must contain 3 SEO-friendly title options.
- `result` must be a single string containing only `H2:` and `H3:` lines separated by `\n`.
- Do not use markdown headings like `##` or `###`.
- Do not output paragraphs, notes, numbering, or explanations outside heading lines.
- Always include `H2: Conclusion`.
- Always include `H2: FAQs` followed by 3 to 5 `H3:` question lines.
- Never generate H4 or deeper headings.

Apply adaptive structure:

- For `vs`, `compare`, or `difference` keywords: include what each side is, key differences, and how to choose.
- For `types`, `methods`, `ways`, or `categories`: include a type or method section with 3 to 5 H3 items.
- For `cost`, `price`, `budget`, or `how much`: include cost drivers, alternatives, and hidden costs.
- For `install`, `setup`, `how to`, or `steps`: include preparation, steps, mistakes, and safety.
- For `inspection`, `frequency`, `standard`, `OSHA`, or `ASME`: include types, frequency, standards, who can perform the work, and checklist or process.

## Self-Check

Review the draft or outline before returning it.

- Hard fail in `outline` mode if the output contains anything other than `H2:` and `H3:` lines in `result`.
- Hard fail in `outline` mode if `Conclusion` or `FAQs` is missing.
- Hard fail in `article` mode if the output is not valid JSON with `title` and `result`.
- Hard fail in `article` mode if `result` is not HTML.
- Hard fail in `article` mode if H1 does not contain the keyword.
- Hard fail in `article` mode if the keyword is absent from the introduction.
- Hard fail in `article` mode if there are fewer than 3 H2 headings.
- Hard fail in `article` mode if word count is below 1500 unless a shorter article was explicitly requested.
- Hard fail in `article` mode if keyword density is below 0.8% or above 3%.
- Soft warn if keyword density is outside the 1%-2% target.
- Soft warn if the conclusion is weak or missing.
- Soft warn if paragraphs are too dense or the article reads as keyword-stuffed.

Revise up to two times. Return only the final version, not the review notes.

## Trigger Examples

Use this skill for requests like:

- "给我一篇关键词+品牌的谷歌 SEO 文章"
- "给我一个只包含 H2/H3 的 SEO 大纲"
- "Write an SEO article for my brand from this keyword"
- "帮我生成独立站博客，返回 HTML 和 JSON"
- "根据关键词和参考链接生成 SEO outline"
