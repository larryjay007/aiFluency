# Agent Concepts and MCP Basics

## Workflow vs. Agent

Anthropic's "Building Effective Agents" draws a specific architectural line: **workflows**
are systems where LLMs and tools run through predefined code paths — the steps, and their
order, are decided in advance, by a human. **Agents** are systems where the LLM dynamically
directs its own process and tool usage, deciding what happens next based on what it's
currently finding — control over the path stays with the model, not a fixed script written
ahead of time.

The essay also names the specific sub-patterns that sit under "workflow": prompt chaining
(a task broken into a fixed sequence of steps, each processing the previous one's output),
routing (classifying an input and sending it down one of several fixed paths), and
orchestrator-workers (a central LLM that dynamically breaks a task into subtasks it
decides on the fly — closer to agent territory, since the subtasks aren't pre-defined).
True agents go a step further than any of these: they plan, act, observe the real result
of their action (a tool call, a piece of code executing), and decide their next move based
on that observation, often across many turns, sometimes pausing for human input at
checkpoints rather than needing a human to sequence every step in advance.

My FL-04 pipeline (source-grounded study notes: gather → synthesize → draft → review →
format) is a **workflow**, specifically the essay's own named "prompt chaining" pattern —
a task decomposed into a fixed sequence of steps I designed ahead of time, where each
step's output feeds directly into the next. Nothing in it lets the AI decide to skip a
step, loop back to gather more if a briefing looks thin, or reach for a different tool
mid-run based on what it actually finds. I am the one deciding the path stays fixed at
five steps every single time, regardless of the topic. That's not a weakness by the
essay's own framing — it's explicit that workflows are the right choice for well-defined,
predictable tasks like mine, and that added complexity (like true agentic autonomy) should
only be introduced when it demonstrably improves outcomes, not by default.

## What MCP Is

MCP (Model Context Protocol) is an open standard for connecting AI applications to
external systems — described in Anthropic's own docs as "a USB-C port for AI
applications": one standard way to connect, instead of a custom integration built
separately for every single tool or data source an AI might need to reach.

It defines three primitives, each with a different party in control:
- **Tools** — model-controlled: executable functions the AI itself decides to call to take
  an action (e.g. searching files, making an API call, running a query). The model chooses
  when and how to use these.
- **Resources** — application-controlled: structured data made available as context (e.g.
  file contents, database records) that the client manages and attaches, more like a GET
  request than an action with side effects.
- **Prompts** — user-controlled: reusable instruction templates a person invokes on
  purpose, closer to a saved slash-command than something the model decides to use on its
  own.

The distinction matters because it maps directly onto the workflow-vs-agent line above: a
model calling MCP tools on its own, mid-task, based on what it's finding, is exactly the
kind of dynamic tool use that turns a fixed workflow into something closer to an agent.

## What My FL-04 Workflow Would Need to Become an Agent

Right now, I run each of FL-04's five steps myself, in order, manually pasting output from
one step into the next. To become an agent rather than a workflow, the LLM itself would
need real decision-making power at each handoff, not just execution power within a fixed
step: deciding when a NotebookLM briefing is too thin and needs a follow-up gather query,
rather than me deciding that by reading it; checking its own drafted notes against the
source and automatically re-drafting anything that doesn't trace back cleanly, instead of
me doing that review by hand; and deciding, per topic, whether more research is even
needed before drafting starts, rather than always running the same five fixed steps
regardless of the topic's complexity.

That requires two real changes: giving the model actual callable tools (a search or
NotebookLM-equivalent tool it can invoke itself, not text I've pre-fetched and pasted in),
and letting it operate in a loop — observing its own output, deciding whether it's good
enough, and choosing to act again if not, rather than stopping after one fixed pass through
each step.

One concrete upgrade, grounded in what I actually just tested: connect a live MCP tool
(like the Google Drive connector below) directly into the pipeline, and let the model
decide on its own whether a topic needs an additional real file pulled in — rather than me
deciding that ahead of time and manually attaching it.

## Live MCP Connector Test — Google Drive

Connected the Google Drive MCP connector to Claude and ran three real tasks:

**Task 1 — List recent files.** Called `list_recent_files`, which returned real, live data:
recent Drive items including a FlyRank internship kickoff slide deck, personal photos, and
a video file — information genuinely impossible for chat alone to produce, since it has no
access to a personal Drive account without a live connection.

**Task 2 — Search for specific files.** Ran two structured searches (`search_files`) for
Colab notebook backups by name and by file type. Both returned empty. This is a real,
informative negative result, not a failure: it confirmed that my actual workflow has been
GitHub-first the whole time (Colab → "Save a copy in GitHub"), not Drive-first, so there was
genuinely nothing to find. A plain chat session could not have verified this either way.

**Task 3 — Read full file content.** Called `read_file_content` on the FlyRank kickoff deck
found in Task 1, and got back the full 57-slide text content — including real, previously
unknown details like my track's specific lead mentor (Mirza Ašćerić for Machine Learning)
and the AI Fluency track's mentors (Léo Yigit Ekiz, Eldin Pintol). This is genuinely new
information neither I nor the AI had before this call — proof of real tool use, not
recalled chat context.

All three outputs show actual tool-use blocks, not plain conversational text — the
connector was genuinely queried live for each one.
