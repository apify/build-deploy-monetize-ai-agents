# Build, deploy and monetize AI agents

Build a real AI agent, ship it to the Apify platform, and turn it into something people (and
other agents) can pay to use - all from this one forked repo.

## Who this is for and how it works

You are a developer who is comfortable with code but new to Apify. In about 40 minutes you will
learn the full Apify Actor lifecycle, then build an AI agent on top of it.

This is a **live, build-along workshop**. We pick one example use case and build it together,
end to end, while explaining each step:

- We scaffold, run, deploy, and monetize one Actor live, then turn it into an AI agent.
- You follow along in your own fork. This repo is your project root, so you scaffold and build
  your Actor right here, in place.
- Each lesson ends with a checkpoint and a "now do this for your own project" nudge, so by the
  end you can swap the example for your own idea and take it straight into your hackathon.

The example is chosen on the day. It is just a vehicle for the lifecycle, so anything you build
follows the same shape. See [Example agent use cases](#example-agent-use-cases) for the kind of
thing we build.

An **[Actor](https://docs.apify.com/platform/actors)** is a serverless program on
[the Apify platform](https://docs.apify.com/platform): it takes JSON input, does a task, and
writes results to a [dataset](https://docs.apify.com/platform/storage/dataset). That simple
shape is all you need to build, deploy, monetize, and eventually run a full AI agent.

> **New to Apify?** Skim [What is Apify](https://docs.apify.com/platform) and
> [Actors](https://docs.apify.com/platform/actors) before you start. A full link list lives in
> [Resources](#resources) at the bottom.

## Scope

What we cover, in order. Each lesson builds on the last.

1. [Lesson 1: Setup and your first Actor](#lesson-1-setup-and-your-first-actor)
2. [Lesson 2: Build and deploy](#lesson-2-build-and-deploy)
3. [Lesson 3: Monetize](#lesson-3-monetize)
4. [Lesson 4: Build an AI agent](#lesson-4-build-an-ai-agent)
5. [Lesson 5: Use Apify in your hackathon project](#lesson-5-use-apify-in-your-hackathon-project)

## Prerequisites

- An **[Apify account](https://console.apify.com/sign-up)** and an **API token** (you create
  the [token](https://docs.apify.com/platform/integrations/api) in Apify Console). You can sign
  up for free and most of the workshop runs on the free tier.
- **[Node.js 22+](https://nodejs.org/)** - the [Apify CLI](https://docs.apify.com/cli/) runs on
  Node.
- **[Python 3.10+](https://www.python.org/downloads/)** - for your Actor and the AI agent in
  lesson 4.
- An **AI coding tool with Agent Skills support** - [Claude Code](https://docs.claude.com/en/docs/claude-code),
  [Cursor](https://cursor.com/), or similar. You will install an Apify skill into it.
- An **LLM API key** for lesson 4 - Anthropic and/or OpenAI. Set `ANTHROPIC_API_KEY` and/or
  `OPENAI_API_KEY` in your environment.

> **Note:** The Apify free tier includes monthly usage credits, which is plenty for this
> workshop. If the event provides a promo code for extra credits, redeem it under **Billing**
> in Apify Console before lesson 3.

---

## Lesson 1: Setup and your first Actor

Install your tools and create, run, and inspect your first Actor locally - no Apify
account needed yet.

### 1. Install Node.js and Python

Make sure you have the runtimes the workshop needs:

```bash
node --version    # should be 22 or higher
python --version  # should be 3.10 or higher
```

If either is missing or too old, install it before continuing.

### 2. Install the Apify CLI

The [Apify CLI](https://docs.apify.com/cli/) is how you create, run, and deploy Actors from
your terminal. See the [installation guide](https://docs.apify.com/cli/docs/installation) for
other install methods.

```bash
npm install -g apify-cli
apify --version
```

You should see a version number printed.

### 3. Set your LLM API key

You will need this in lesson 4, so set it now. Add one or both to your shell environment:

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
export OPENAI_API_KEY="sk-..."
```

> **Tip:** Add these to your shell profile (for example `~/.zshrc`) so they persist across
> sessions. Never commit keys to the repo.

### 4. Install the Apify Actor Development skill

This skill teaches your AI coding tool how to scaffold, build, and deploy Actors. It powers the
vibe-coding in lesson 4.

```bash
npx skills add https://github.com/apify/agent-skills --skill apify-actor-development
```

Pick the `apify-actor-development` skill when prompted. You can browse all available skills in
the [apify/agent-skills](https://github.com/apify/agent-skills) repo, and read what this one
does in its [SKILL.md](https://github.com/apify/agent-skills/blob/main/skills/apify-actor-development/SKILL.md).

### 5. Create your first Actor

From the root of this repo, scaffold a basic Python Actor:

```bash
apify create
```

When prompted, choose **Python** and the **Getting started with Python** template. The CLI
creates your Actor's files in place. Browse every starter at
[Apify Actor templates](https://apify.com/templates), and read the
[Python SDK docs](https://docs.apify.com/sdk/python) for how Actor code is structured.

### 6. Tour the structure

Open the files the CLI created and find these (all defined in the
[Actor definition](https://docs.apify.com/platform/actors/development/actor-definition) docs):

- [`.actor/actor.json`](https://docs.apify.com/platform/actors/development/actor-definition/actor-json) -
  your Actor's metadata and configuration.
- [`.actor/input_schema.json`](https://docs.apify.com/platform/actors/development/actor-definition/input-schema) -
  the shape of the JSON input your Actor accepts.
- `src/` - your Actor's code (the `main` entry point lives here).
- [`Dockerfile`](https://docs.apify.com/platform/actors/development/actor-definition/dockerfile) -
  how your Actor is built and run on the Apify platform.

### 7. Run it locally

```bash
apify run
```

The Actor runs on your machine. When it finishes, look in the `storage/` folder: your output
is written to `storage/datasets/default/`, and any input it read is in
`storage/key_value_stores/default/`. See the
[Storage docs](https://docs.apify.com/platform/storage) for how datasets and key-value stores
work.

> **Note:** The Actor model is just this: JSON input goes in, the Actor runs a task, and
> results go out to a dataset. Everything else in this workshop builds on that shape.

**Resources**

- [Apify CLI installation](https://docs.apify.com/cli/docs/installation) and
  [command reference](https://docs.apify.com/cli/docs/reference)
- [Apify Actor templates](https://apify.com/templates)
- [Actor definition](https://docs.apify.com/platform/actors/development/actor-definition):
  [actor.json](https://docs.apify.com/platform/actors/development/actor-definition/actor-json),
  [input schema](https://docs.apify.com/platform/actors/development/actor-definition/input-schema),
  [Dockerfile](https://docs.apify.com/platform/actors/development/actor-definition/dockerfile)
- [Apify SDK for Python](https://docs.apify.com/sdk/python) and
  [Storage](https://docs.apify.com/platform/storage)
- [apify/agent-skills](https://github.com/apify/agent-skills)

Continue to [Lesson 2: Build and deploy](#lesson-2-build-and-deploy).

---

## Lesson 2: Build and deploy

Deploy your Actor to the Apify platform and run it in Apify Console.

### 1. Get your API token

In Apify Console, open **Settings > API & Integrations** and copy your personal API token. See
[API integration](https://docs.apify.com/platform/integrations/api) for details.

### 2. Log in from the CLI

```bash
apify login
```

Paste your API token when prompted. This links the CLI to your Apify account.

### 3. Push your Actor

```bash
apify push
```

This builds your Actor on the Apify platform and uploads your code. When it finishes, the CLI
prints a link to your Actor in Apify Console.

### 4. Run it in Apify Console

Open the link, fill in the input, and click **Start**. When the run finishes, open the
**Dataset** tab to see the same output you saw locally in `storage/`, now stored on the Apify
platform. The [Running Actors](https://docs.apify.com/platform/actors/running) docs walk through
this in Apify Console.

### 5. Note the API endpoint

Every Actor has a run endpoint, so you can trigger it over HTTP from anywhere:

```
https://api.apify.com/v2/acts/<your-username>~<actor-name>/runs?token=<API_TOKEN>
```

You will not call it now, but this is how your agent (or anyone) runs your Actor later. See
[Run Actor and retrieve data via API](https://docs.apify.com/academy/api/run-actor-and-retrieve-data-via-api)
and the [Apify API reference](https://docs.apify.com/api/v2).

### 6. Keep secrets out of your code with `apify secrets`

If your Actor needs an API key, store it as a secret instead of hardcoding it:

```bash
apify secrets add MY_API_KEY "the-secret-value"
```

You then reference the secret from your input schema or environment, so the value never lands
in the repo.

> **Note:** Secrets you add with `apify secrets` live on the Apify platform and are injected at
> runtime. This is the safe way to give a deployed Actor its credentials. See
> [secret environment variables](https://docs.apify.com/platform/actors/development/programming-interface/environment-variables)
> and [secret inputs](https://docs.apify.com/platform/actors/development/actor-definition/input-schema/secret-input).

**Resources**

- [API integration](https://docs.apify.com/platform/integrations/api) and the
  [Apify API reference](https://docs.apify.com/api/v2)
- [Running Actors](https://docs.apify.com/platform/actors/running) and
  [Run Actor and retrieve data via API](https://docs.apify.com/academy/api/run-actor-and-retrieve-data-via-api)
- [CLI command reference](https://docs.apify.com/cli/docs/reference) (`apify login`,
  `apify push`, `apify secrets`)
- [Secret environment variables](https://docs.apify.com/platform/actors/development/programming-interface/environment-variables)

Continue to [Lesson 3: Monetize](#lesson-3-monetize).

---

## Lesson 3: Monetize

Learn the monetization flow and configure pricing. We publish for real
in lesson 4, once the Actor actually does something.

### 1. Add your billing details

Before you can monetize, add your billing and payout details in Apify Console under
**Settings > Billing**. This is a prerequisite for receiving payouts.

### 2. Walk through publication

Open your Actor in Apify Console, go to the **Publication** tab, and review the publish
checklist - but hold off on submitting for review. Right now it's just the starter template, so
there's nothing worth shipping. You'll publish the finished agent in lesson 4. A public Actor is
what other people (and agents) can discover and run. See
[Publish your Actor](https://docs.apify.com/platform/actors/publishing/publish) for the full
checklist.

### 3. Choose a pricing model

The Apify platform supports three models:

- **Pay per usage** - users pay only for the platform resources their run consumes.
- **Rental** - users pay a flat recurring fee to use your Actor.
- **Pay per event (PPE)** - users pay for specific events you define, such as each result
  produced. This is the most flexible model for agents.

### 4. Set up monetization

In the **Publication** tab, open **Monetization** and follow the wizard (full walkthrough in
[Monetize your Actor](https://docs.apify.com/platform/actors/publishing/monetize)):

1. **Actor pricing** - choose your pricing model and configure the events or rates.
2. **Primary event** - pick the one event that best represents the value your Actor delivers
   (for example, one result returned).
3. **Review** - confirm everything, then submit.

> **Note:** Pay-per-event Actors with limited permissions automatically become eligible for
> autonomous agent payment over protocols like x402 and Skyfire. There is no separate opt-in,
> so an AI agent can discover and pay for your Actor on its own. Rental and pay-per-usage
> Actors are not eligible.

### 5. Track your earnings

Once your Actor has paying users, find your earnings in Apify Console under **Insights**. Payout
invoices are generated automatically each month.

**Resources**

- [Publishing overview](https://docs.apify.com/platform/actors/publishing) and
  [Publish your Actor](https://docs.apify.com/platform/actors/publishing/publish)
- [Monetize your Actor](https://docs.apify.com/platform/actors/publishing/monetize) (pricing
  models, primary event, x402 / Skyfire eligibility)
- [Actor pricing models explained](https://docs.apify.com/platform/actors/running/actors-in-store#pricing-models)
- [Apify Store](https://apify.com/store)

Continue to [Lesson 4: Build an AI agent](#lesson-4-build-an-ai-agent).

---

## Lesson 4: Build an AI agent

Turn your Actor into a real AI agent: a reasoning loop that calls tools and writes
its results to a dataset.

### 1. Understand the pattern

An AI agent on Apify is just an Actor with three parts:

- **The agent loop runs inside the Actor.** The Actor's code drives an LLM that reasons, decides
  what to do next, and repeats until the task is done.
- **Apify Actors are the tools.** When the agent needs real data or an action, it calls another
  Actor from [Apify Store](https://apify.com/store) (a scraper, a search Actor, your own Actor)
  over the [API](https://docs.apify.com/api/v2).
- **The dataset is the output.** Whatever the agent produces gets written to the dataset, just
  like in lesson 1.

You can hand-wire the loop yourself with
**[LangGraph](https://langchain-ai.github.io/langgraph/)** (`pip install langgraph`), or let a
framework run it for you with the
**[OpenAI Agents SDK](https://openai.github.io/openai-agents-python/)**
(`pip install openai-agents`, `from agents import Agent, Runner`). Use whichever you prefer.
Either way, point it at the latest [Claude](https://docs.claude.com/en/docs/about-claude/models)
or [OpenAI](https://platform.openai.com/docs/models) model and check the provider docs for the
current model id.

### 2. Pick your agent

Choose one of the [example use cases](#example-agent-use-cases) above, or bring your own. Keep
it small enough to finish in this lesson: one clear input, one tool Actor, one useful output.

### 3. Vibe-code it with your AI tool

You installed the `apify-actor-development` skill in lesson 1, so let your AI coding tool do the
heavy lifting. Describe the agent and let it scaffold and implement the code in this repo. For
example:

```
Build an AI agent inside this Actor. It takes a {your input} as input, uses the
{an Apify Store Actor} Actor as a tool to fetch data, runs an agent loop with
{LangGraph or the OpenAI Agents SDK} on the latest Claude model, and writes the
{your result} to the dataset. Add the LLM API key as a secret input in the input
schema so it works both locally and once deployed. Follow the Apify Actor
Development skill.
```

Let the tool update your input schema, add dependencies, and write the loop. Review what it
generates.

### 4. Run it locally

```bash
apify run
```

Pass your input, watch the agent loop in the logs, and check the result in
`storage/datasets/default/`.

> **Tip:** If the agent calls another Actor as a tool, it needs an API token at runtime. After
> `apify login` (lesson 2), `apify run` injects your token automatically as `APIFY_TOKEN`, and
> the [Apify client for Python](https://docs.apify.com/api/client/python/) picks it up with no
> extra setup. Only export `APIFY_TOKEN` manually if you run the script outside `apify run`. Once
> deployed, the platform injects it for you.

### 5. Add your LLM key to the platform

Your local runs read the key from your shell, but the deployed Actor needs its own copy. Store
it as a secret (the same `apify secrets` you saw in lesson 2), then reference it from your input
schema or environment:

```bash
apify secrets add ANTHROPIC_API_KEY "sk-ant-..."
```

> **Note:** Without this, the deployed run fails at the first model call - the key in your shell
> never leaves your machine.

### 6. Deploy it

When it works locally, ship it:

```bash
apify push
```

Run it in Apify Console to confirm your agent works on the Apify platform too.

### 7. Publish it for real

Now that your Actor does something useful, finish the publication you previewed in lesson 3. Open
the **Publication** tab, complete the checklist, and submit it to Apify Store. If you set up pay
per event, it's now agent-payable too.

**Resources**

- [LangGraph docs](https://langchain-ai.github.io/langgraph/) and
  [OpenAI Agents SDK docs](https://openai.github.io/openai-agents-python/)
- [Claude models](https://docs.claude.com/en/docs/about-claude/models) and
  [OpenAI models](https://platform.openai.com/docs/models)
- [Apify client for Python](https://docs.apify.com/api/client/python/) (call Actors as tools)
- [Apify Store](https://apify.com/store) (find Actors to use as tools) and the
  [Store API](https://docs.apify.com/api/v2)
- [apify-actor-development SKILL.md](https://github.com/apify/agent-skills/blob/main/skills/apify-actor-development/SKILL.md)

Continue to [Lesson 5: Use Apify in your hackathon project](#lesson-5-use-apify-in-your-hackathon-project).

---

## Lesson 5: Use Apify in your hackathon project

See how Apify speeds up your hackathon project - both as an AI agent you build from
this repo and as thousands of ready-made Actors you can plug straight in.

### Build your own AI agent from this repo

You now have the full lifecycle working. Fork this repo and use it as the starting point for your
own agent: the pattern stays fixed (loop, tools, dataset), and you change only three parts -
the input, the tool Actor it calls, and the output it writes. Let your AI coding tool plus the
`apify-actor-development` skill do the heavy lifting, and keep the LLM API key as a secret input
so it works locally and once deployed. When it works, deploy and publish it just like in lessons
2 to 4.

### Don't build from scratch - use ready-made Actors

You do not have to build everything yourself. [Apify Store](https://apify.com/store) has
thousands of ready-made Actors - scrapers, data extractors, integrations, and AI tools - that
you can use in your project in two ways:

- **As tools your agent calls** - point your agent at a Store Actor to fetch real data or take
  an action, exactly like the workshop agent did.
- **As building blocks in any project** - call a Store Actor over the
  [API](https://docs.apify.com/api/v2) from any stack to get data or automation without writing
  the scraper or integration yourself. Even if your hackathon project is not an AI agent, this is
  the fastest way to get real data into it.

Browse [Apify Store](https://apify.com/store), find an Actor for your domain, and wire it in.

### Your hackathon Apify credits

We are giving away Apify credits for this hackathon. Redeem the coupon code below under
**Settings > Billing** in Apify Console to top up your account, then use it across everything in
this workshop - running Actors, deploying your agent, and calling Store Actors.

```
Coupon code: <ADD-HACKATHON-CODE>
```

> **Note:** Redeem the coupon before you start so the credits are available for your runs. Apify
> credits cover Actor runs and platform usage; LLM API calls still bill your own Anthropic or
> OpenAI account.

**Resources**

- [Apify Store](https://apify.com/store) (thousands of ready-made Actors for your project)
- [Apify API reference](https://docs.apify.com/api/v2) (call any Actor from any stack)
- [Input schema](https://docs.apify.com/platform/actors/development/actor-definition/input-schema)
  and [dataset schema](https://docs.apify.com/platform/actors/development/actor-definition/dataset-schema)
- [Publish your Actor](https://docs.apify.com/platform/actors/publishing/publish) and
  [Monetize your Actor](https://docs.apify.com/platform/actors/publishing/monetize)
- Full link list in [Resources](#resources) below

That is the full lifecycle, plus everything Apify gives you for your hackathon. Now go build. Back
to the top: [Build, deploy and monetize AI agents](#build-deploy-and-monetize-ai-agents).

---

## Resources

Everything linked across the lessons, grouped for quick reference.

### Platform and Actors

- [The Apify platform](https://docs.apify.com/platform)
- [Actors overview](https://docs.apify.com/platform/actors)
- [Running Actors](https://docs.apify.com/platform/actors/running) and
  [Actors in Store](https://docs.apify.com/platform/actors/running/actors-in-store)
- [Apify Store](https://apify.com/store)

### CLI and SDKs

- [Apify CLI](https://docs.apify.com/cli/) -
  [installation](https://docs.apify.com/cli/docs/installation),
  [command reference](https://docs.apify.com/cli/docs/reference)
- [Apify SDK for Python](https://docs.apify.com/sdk/python)
- [Apify client for Python](https://docs.apify.com/api/client/python/)
- [Actor templates](https://apify.com/templates)

### Actor definition and storage

- [Actor definition](https://docs.apify.com/platform/actors/development/actor-definition)
- [actor.json](https://docs.apify.com/platform/actors/development/actor-definition/actor-json)
- [Input schema](https://docs.apify.com/platform/actors/development/actor-definition/input-schema)
  and [secret inputs](https://docs.apify.com/platform/actors/development/actor-definition/input-schema/secret-input)
- [Dataset schema](https://docs.apify.com/platform/actors/development/actor-definition/dataset-schema)
- [Dockerfile](https://docs.apify.com/platform/actors/development/actor-definition/dockerfile)
- [Storage](https://docs.apify.com/platform/storage) and
  [datasets](https://docs.apify.com/platform/storage/dataset)
- [Environment variables and secrets](https://docs.apify.com/platform/actors/development/programming-interface/environment-variables)

### Running over the API

- [API integration](https://docs.apify.com/platform/integrations/api)
- [Run Actor and retrieve data via API](https://docs.apify.com/academy/api/run-actor-and-retrieve-data-via-api)
- [Apify API reference](https://docs.apify.com/api/v2)

### Publishing and monetization

- [Publishing overview](https://docs.apify.com/platform/actors/publishing)
- [Publish your Actor](https://docs.apify.com/platform/actors/publishing/publish)
- [Monetize your Actor](https://docs.apify.com/platform/actors/publishing/monetize)
- [Pricing models](https://docs.apify.com/platform/actors/running/actors-in-store#pricing-models)

### AI agents and tooling

- [Apify agent skills](https://github.com/apify/agent-skills) and the
  [apify-actor-development skill](https://github.com/apify/agent-skills/blob/main/skills/apify-actor-development/SKILL.md)
- [LangGraph](https://langchain-ai.github.io/langgraph/)
- [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/)
- [Claude models](https://docs.claude.com/en/docs/about-claude/models) and
  [Claude Code](https://docs.claude.com/en/docs/claude-code)
- [OpenAI models](https://platform.openai.com/docs/models)

Back to the top: [Build, deploy and monetize AI agents](#build-deploy-and-monetize-ai-agents).
