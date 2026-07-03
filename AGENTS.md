# Guidance for AI coding tools

This repo is a forkable starter for a hands-on Apify Actor workshop. The full workshop lives in
[`README.md`](./README.md): intro, prerequisites, example use cases, five steps, and a
resources list. `README.md` is both the workshop structure and your reference for building
agents here.

This repo is the project root. Build the user's Actor directly here. Do not create a separate
demo folder.

## Defer to the Apify Actor Development skill

For scaffolding, building, debugging, or deploying Actors, defer to the **Apify Actor
Development Agent Skill** (`apify-actor-development`). Do not duplicate its instructions here.

```bash
npx skills add https://github.com/apify/agent-skills --skill apify-actor-development
```

## Actor lifecycle (the workflow this repo teaches)

1. **Scaffold** - `apify create` a Python starter (the "Getting started with Python" template).
   [Templates](https://apify.com/templates), [Python SDK](https://docs.apify.com/sdk/python).
2. **Run locally** - `apify run`. Output lands in `storage/datasets/default/`.
   [Storage](https://docs.apify.com/platform/storage).
3. **Deploy** - `apify login` then `apify push`, then run it in Apify Console.
   [CLI reference](https://docs.apify.com/cli/docs/reference),
   [Running Actors](https://docs.apify.com/platform/actors/running).
4. **Monetize** - publish from the **Publication** tab, set up pricing (pay per usage, rental,
   or pay per event). Pay-per-event Actors with limited permissions are auto-eligible for agent
   payment (x402 / Skyfire).
   [Monetize](https://docs.apify.com/platform/actors/publishing/monetize).

## Actor anatomy

- [`.actor/actor.json`](https://docs.apify.com/platform/actors/development/actor-definition/actor-json) -
  metadata and config.
- [`.actor/input_schema.json`](https://docs.apify.com/platform/actors/development/actor-definition/input-schema) -
  the JSON input shape.
- `src/` - Actor code (`main` entry point).
- [`Dockerfile`](https://docs.apify.com/platform/actors/development/actor-definition/dockerfile) -
  build and run definition.
- Output goes to a [dataset](https://docs.apify.com/platform/storage/dataset).
- Keep secrets in env vars or [`apify secrets`](https://docs.apify.com/platform/actors/development/programming-interface/environment-variables),
  never in the repo.

## Building an AI agent (step 2, the payoff)

The pattern: **the agent loop runs inside the Actor, Apify Actors are the tools it calls, and
the dataset is the output.**

- Drive the loop with [LangGraph](https://langchain-ai.github.io/langgraph/) (hand-wired) or the
  [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/) (framework).
- Call Actors as tools with the
  [Apify client for Python](https://docs.apify.com/api/client/python/); find tools in
  [Apify Store](https://apify.com/store).
- Use the latest [Claude](https://docs.claude.com/en/docs/about-claude/models) or
  [OpenAI](https://platform.openai.com/docs/models) model; check provider docs for the current
  model id. The agent needs `APIFY_TOKEN` plus an LLM API key at runtime.

## Writing style (for any docs edits)

- "Actor" is always capitalized when it means an Apify Actor. "task" and "schedule" stay
  lowercase.
- Use "Apify Store", "Apify Console", "Apify Proxy" (no "the"), but "the Apify platform".
- Headings in sentence case, never title case.
- No em dashes. Use a spaced hyphen ` - ` instead.
- US English, Oxford comma.
