---
name: stewreads
description: Save an ebook or a one-page brief to the user's StewReads library through the StewReads connector, or find and reopen something they saved there. Use when the user says "stew this", "send this to Stew" or "save this to StewReads", asks for an ebook or brief to keep and read later, or wants to continue from a saved StewReads item.
---

# StewReads

StewReads is a personal library for documents made with AI. With the StewReads connector you can write an ebook or a brief and save it to the user's library, and find a saved item so the user can keep working from it.

## Before you start

This skill needs the StewReads connector (MCP server URL `https://mcp.stewreads.com/mcp`). If its tools are not available in this conversation, tell the user and point them to the setup guide at https://stewreads.com/help/mcp. Nothing can be saved without it.

## Tools

The connector has four tools. Your client may show them with a prefix.

| Tool | What it does |
| --- | --- |
| `get_content_guidelines(content_type)` | Returns the writing guidelines for `ebook` or `brief`. |
| `save(markdown, title, content_type)` | Adds a new item to the library. Optional: `filename`, `model_id`, `model_name`. |
| `query(query, content_type)` | Lists saved items, newest first. Both arguments are optional. `query` text matches item titles. |
| `fetch(artifact_id)` | Returns one saved item with its full markdown in `content`. |

`save`, `query` and `fetch` return `{success, data, error}`.

## Choose the content type

- `ebook`: long-form reading on a topic. Can be read in StewReads, emailed, or listened to as an audiobook.
- `brief`: one short page that captures a single idea.

Choose from what the user asked for, never from which AI client or model you are. If it is unclear which they want, ask once before saving.

## Save an ebook

1. Call `get_content_guidelines(content_type="ebook")`.
2. Write the ebook by those guidelines. Work out the topic the user wants to keep; if it is not clear, ask one short question. Write it as an original document in your own words, drawing on what the conversation worked out. Do not quote the user's messages or write "you asked" or "I said".
3. Call `save` with the ebook as `markdown`, a clear `title`, `content_type="ebook"`, and the title as `filename`.
4. Reply briefly with the saved title and the `reader_url` link from the result.

If the user asked to save, there is no need to show the full text first. If they want to review it, show it and save after they confirm.

## Save a brief

1. Call `get_content_guidelines(content_type="brief")`.
2. Write one brief in the shape the guidelines give. If the conversation holds several ideas worth keeping, suggest an ebook instead.
3. Call `save` with the brief as `markdown`, its `title` and `content_type="brief"`.
4. Reply briefly with the saved title and the `reader_url` link.

Always call `get_content_guidelines` for the matching type before you write anything you will save. The guidelines can change, so do not write from memory.

## Find a saved item and continue from it

1. Call `query`. Pass `query` text to search titles, `content_type` to list only ebooks or only briefs, or neither to list everything. Results cover items saved from every AI client the user has connected; pass `include_all_clients=false` only if the user asks for items saved from this client alone.
2. Call `fetch` with the `artifact_id` of the item the user means. If several items could match, ask which one.
3. Work from the returned `content`.

`query` returns 20 items unless you pass `limit` (up to 100). Raise `offset` to see more.

An item whose `source_format` is `epub` is an imported EPUB. `fetch` cannot return it.

`save` always creates a new item. It cannot update or replace one saved earlier. If the user asks to change a saved item, tell them that saving again adds a separate copy and the earlier one stays.

## What to tell the user about delivery

No tool takes an email address, so do not ask the user for one. StewReads emails a saved ebook only when the user has turned on auto-send in StewReads, and only to the address they chose there, which can be a Kindle address. Briefs are never emailed.

After saving an ebook, read `delivery_status` in the result:

| `delivery_status` | What to say |
| --- | --- |
| `skipped` | Saved. Not emailed, because auto-send is off. |
| `not_configured` | Saved. Not emailed, because auto-send is on but its destination has no address. |
| `queued` | Saved, and the email is waiting to go out. Do not say it was sent. |
| `sent` | Saved, and the email was accepted for delivery to the address in `delivery_target_masked`. |
| `failed` | Saved, but the email failed. |

Auto-send and the delivery address are set in StewReads, not through the connector. The Kindle guide is at https://stewreads.com/help/kindle.

## Limits

- One save holds up to 200,000 characters of markdown and a title of up to 500 characters. A longer document has to be shortened or split into several saves.
- Through these tools you cannot delete or change saved items, change settings, email an address typed in the chat, or start an audiobook. If asked, say so and point the user to StewReads.
- The connector saves ebooks and briefs only. It does not save or retrieve coding-session notes, handoffs or other working files. If the user asks to save one, say so, and do not save it as an ebook or brief unless they ask for that. If they ask about such an item saved earlier, say the connector cannot list or fetch it; do not say it was deleted.

## Errors

When `success` is false, read `error.code` and `error.message`:

- `UNAUTHORIZED`: ask the user to reconnect StewReads.
- `VALIDATION_ERROR`: nothing was saved. Tell the user what the message says.
- `NOT_FOUND`: the item does not exist or cannot be fetched with these tools.
- `RATE_LIMITED` or `UPSTREAM_ERROR`: try again shortly if `error.retryable` is true. Before repeating a `save`, call `query` to check the item was not already added.
