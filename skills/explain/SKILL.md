---
name: explain
description: Create a narrated, animated explanation of a topic that plays in the browser from a secret link. Use when the user asks to explain, visualize or teach something visually, or asks for an explainer video, rather than a text answer or a static diagram.
---

Create a narrated visual explanation of $ARGUMENTS with the elucify MCP tools. When invoked automatically, explain the user's actual question, using any context they supplied.

1. Call `get_authoring_instructions` and follow it. It returns the core rules and an index of references. If the narration will not be in English, read the `languages` reference now, before step 2.
2. Call `create_explanation` and show the returned viewer link to the user immediately, before writing any scene. Keep working in the same turn while they watch.
3. Before a scene uses an element named in the index (a graph, table, geometry and so on), read its reference with `get_element_reference`; one call takes up to 6 names. Do not write from memory. Use `get_scene_schema` for exact fields (`kinds` narrows it to the object kinds you use).
4. Append one complete scene at a time with `append_scene`, starting with a substantial first scene (2-3 beats, about 30 seconds of narration) so it is still playing when the next scene arrives. If a scene is rejected, fix that same scene and resend it; if an append is interrupted, resend the identical scene.
5. Call `finalize_explanation` when every scene is accepted. Give the viewer link and mention any captions-only warning.

The first link is secret, not private: anyone who has it can watch, and it can't be withdrawn, so tell the user to keep it to themselves. If they want others to watch, call `share_explanation` and give them that link instead; `revoke_share` withdraws it.

If the tools are missing or the connection is refused, tell the user to sign in: run `/mcp`, choose elucify and Authenticate (in the browser, Continue with Google, or without an account), then ask again. Signed in with Google, they can manage their explanations at https://elucify.dev/account. Do not substitute a text answer.
