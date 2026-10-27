---
name: explain
description: Create a narrated, animated explanation of a topic that plays in the browser from a secret link. Use when the user asks to explain, visualize or teach something visually, or asks for an explainer video, rather than a text answer or a static diagram.
---

Create a narrated visual explanation of $ARGUMENTS with the elucify MCP tools. When invoked automatically, explain the user's actual question, using any context they supplied.

1. Call `create_explanation` first and show the returned viewer link to the user immediately, before writing any scene; keep working in the same turn while they watch. Its result also carries the core authoring instructions and the scene schema for the common kinds (Text, Graph, Table, Diagram): follow them. If the narration will not be in English, pass the language's code as `language` (or put it in the first scene's `declaration.language`) and read the `languages` reference before writing the first scene.
2. Before a scene uses an element named in the index (a graph, table, geometry and so on), read its reference with `get_element_reference`; one call takes up to 6 names. Do not write from memory. Use `get_scene_schema` for exact fields (`kinds` narrows it to the object kinds you use). `get_authoring_instructions` returns the same core, if you want it separately.
3. Append one complete scene at a time with `append_scene`. Make the first scene cheap to write and long to play: one panel in the `full` slot, few objects and actions, 2-3 beats of narration, about 20-30 seconds of play. A scene's slots must all come from one group in the schema's Slot line (`full` is alone). If a scene is rejected, fix that same scene and resend it; if an append is interrupted, resend the identical scene. `append_scene` returns as soon as the scene is accepted, while its beats may still be voiced, so send the next scene straight away; its result says how many beats are ready.
4. Call `finalize_explanation` when every scene is accepted. It does not hold for the voice: if it says beats are still being voiced, the explanation ends by itself and no further call is needed. Give the viewer link and mention any captions-only warning.

The first link is secret, not private: anyone who has it can watch, and it can't be withdrawn, so tell the user to keep it to themselves. If they want others to watch, call `share_explanation` and give them that link instead; `revoke_share` withdraws it.

If the tools are missing or the connection is refused, tell the user to sign in: run `/mcp`, choose elucify and Authenticate (in the browser, Continue with Google, or without an account), then ask again. Signed in with Google, they can manage their explanations at https://elucify.dev/account. Do not substitute a text answer.
