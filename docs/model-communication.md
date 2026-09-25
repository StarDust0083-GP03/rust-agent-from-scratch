# Model communication

Status: initial interface notes; no implementation yet.

## Goal

Call Anthropic and OpenAI for chat, and TypeSafe AI's Jev for typed decisions. Keep the project dependency-free for now; the wire-format and HTTP implementation are future work. Jev is **not** a chat model, so it should not be forced into the same chat request/response interface.

## Basic interfaces to design

- **Chat request/response (Anthropic, OpenAI):** model, system instructions, ordered messages → assistant output, stop reason, usage.
- **Decision request/response (Jev):** model, `state` (string/object/array), keyed `questions` → keyed typed `answers`, model used, usage. Question kinds: `noul` (yes probability), `choice` (chosen option, distribution, confidence), `score` (weighted score, level distribution, confidence). Each question has `instructions`; Choice and Score require `criteria`.
- **Common transport concerns:** API key, HTTP errors (status and provider message), and timeouts. These can be shared without making chat and decision results identical.

Sources: [TypeSafe API reference](https://docs.typesafe.ai/api), [Jev model details](https://docs.typesafe.ai/models).

## Provider adapters

- `Anthropic`: map the shared conversation to Anthropic's Messages API and map its response back.
- `OpenAI`: map to OpenAI's chat/responses API and map its response back.
- `Jev`: TypeSafe AI's System One decision model. `POST https://api.typesafe.ai/v1/systemone` with `Authorization: Bearer <API_KEY>` and JSON `{ "model": "jev-latest", "state": ..., "questions": { ... } }`. Returns `{ "model": ..., "answers": { ... }, "usage": ... }`. Get a key from the [TypeSafe dashboard](https://console.typesafe.ai/keys). [Official quick start](https://docs.typesafe.ai/introduction/quickstart), [HTTP API](https://docs.typesafe.ai/api).

## Initial scope

Start with one non-streaming chat request to one chat provider. Build Jev separately as one `noul` decision on a text state; it does not generate assistant text. Defer streaming, tools, images, retries, and provider-specific options until needed.

## Documentation checked

- Anthropic API overview: <https://docs.anthropic.com/en/api/overview>
- OpenAI API reference: <https://platform.openai.com/docs/api-reference>
- Jev: [TypeSafe AI introduction](https://typesafe.ai/blog/introducing-system-one-models-and-jev), [official docs](https://docs.typesafe.ai/), [quick start](https://docs.typesafe.ai/introduction/quickstart), [API reference](https://docs.typesafe.ai/api), [models](https://docs.typesafe.ai/models).

## Open questions

1. Should the first implementation support only plain text, non-streaming calls?
2. Which chat provider should be the first end-to-end implementation?
