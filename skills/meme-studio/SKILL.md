---
name: meme-studio
description: Make a meme or an animated GIF with Meme Studio - pick the template that fits the joke, write a caption for each of its slots and give the person the finished picture or the link to edit it.
---

# Make a meme with Meme Studio

Every tool named below comes from Meme Studio's MCP server, `https://memes.mrhyde.dev/mcp`. If
`suggest_templates` is not available, add it first: `claude mcp add --transport http meme-studio https://memes.mrhyde.dev/mcp`
(Claude Code), `codex mcp add meme-studio --url https://memes.mrhyde.dev/mcp` (Codex), or point any other MCP
client at the URL. No account or key is needed.

## Steps

1. `suggest_templates` with the joke or the situation, in the person's words and language ("my code works locally but
   not in production"). It returns the five templates that fit best, each with what it is used for (`used_for`),
   whether it is animated, and its slots: what goes in each one (`hint`) and about how many characters fit
   (`fits_chars`). For what is popular or new, use `popular_templates` (`trending`, `most_used` or `new`); for a
   template by name, `search_templates`.
2. Pick the template whose `used_for` and slot hints match the joke. A meme works when each caption does what its
   slot's hint says: in Drake, the top slot is what gets rejected and the bottom one what gets chosen.
3. `create_meme` with the template, one caption per slot (`slot` with its number or name), each under the slot's
   `fits_chars`, in the person's language, and `language` set. Ask for `picture: "final"` to get the finished meme:
   a 1200 px picture, or an animated GIF for an animated template. To use the person's own picture, give `image`
   (base64) or `image_url` (an https address) instead of `template`.
4. Follow `deliver` in the answer:
   - `picture`: the attached picture is the finished meme. Give it to the person, or save it to a file if they cannot
     see tool results.
   - `edit_url`: give the person that link, which opens the meme in Meme Studio to adjust and download it.
   - `fix_first`: fix what the warnings say and call `create_meme` again. After two tries, give what you have.

## Rules

- Paste `edit_url` exactly as returned, on its own line, never retyped: it carries the whole meme.
- Never paste a picture's base64 by hand. With a terminal, pipe the `tools/call` JSON from the shell into
  `curl --data-binary @-` to `https://memes.mrhyde.dev/mcp` and decode the image item of the result into a file.
- Do not put captions over faces, and keep each caption to one idea in a few words.
- Name only file paths you saved to or were told, never a guessed one.

## Done when

- The person has the finished picture (or the GIF), or the `edit_url` exactly as returned.
- Each caption sits in its slot and does what the slot's hint says.
