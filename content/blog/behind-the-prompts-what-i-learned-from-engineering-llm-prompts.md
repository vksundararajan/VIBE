+++
title = "Behind the Prompts: What I Learned from Engineering LLM Prompts"
date = "2025-05-16T08:55:40-04:00"
tags = ["AI","KT"]
+++

Over the last 2 days, I’ve dived deep into <mark>Prompt Engineering</mark> — not the surface-level prompt crafting, but the core architecture of how LLMs interpret, generate, and optimise responses based on prompt structure and configuration.

What started as a casual exploration through **Google’s Prompt Engineering Essentials** turned into an unexpectedly technical journey that bridged NLP design, token sampling, inference configuration, and agent-based reasoning.

Let’s break it down.

## LLMs Are Not Magical — They're Token Predictors
Large Language Models, whether GPT, Gemini, Claude, or Llama, are not creative writers — they are statistical machines that **predict the next token given the previous context**. That’s all.

When we input a prompt, the LLM
- Tokenises the input text (word pieces, subwords, or characters),
- Uses the prompt as conditioning context,
- Predicts the next token by sampling from a probability distribution over its vocabulary.

Each output token becomes part of the next input — forming an autoregressive chain. Now, this chain is where prompt engineering plays a major role.

## Prompt Engineering = Controlling the Prediction Pipeline

When we design prompts, we're essentially **biasing the LLM** towards the desired output path. But that’s only half the job — the other half is configuring the **LLM's sampling settings**, which control how randomness and diversity affect token generation.

From Google’s whitepaper, I realised there are three core parameters we must understand:

### Temperature → Controls entropy in token sampling
- `temperature = 0`: greedy decoding (always picks the highest probability token)
- `temperature = 1`: softmax distribution as-is
- `temperature > 1`: more randomness (creativity ↗️, consistency ↘️)

### Top-K → Limits the candidate pool to the top-K probable tokens.
- `K = 1`: greedy
- `K = 40`: more creative
- `K = vocab size`: no filtering

### Top-P (Nucleus Sampling) → Chooses the smallest possible set of tokens whose cumulative probability ≥ P.
- `P = 0.9`: includes only highly probable tokens
- `P = 1`: considers the full vocabulary (no filtering)

These three form the **sampling triad**. 

Google recommends baseline configs:
- <mark>Factual Tasks</mark> temperature: 0.2, top-p: 0.9, top-k: 20
- <mark>Creative Tasks</mark> temperature: 0.9, top-p: 0.99, top-k: 40
- <mark>Deterministic (math/code)</mark> temperature: 0, top-p: 1, top-k: 1

## Google's 5-Step Prompting Framework
- <mark>Task || Thoughtfully</mark> You clearly tell the model what to do, who it should act as, and how the output should look. For example, you can define a role like “You are a cyber expert” and ask for answers in bullet points or JSON. This sets the initial prompt conditioning. The model shifts its attention toward specific vocabulary and structure. It prepares the token prediction path based on the persona and output format you mentioned.

- <mark>Context || Create</mark> You provide supporting details, like constraints or goals — such as “do it under ₹500” or “explain to a beginner”. It helps the model understand the scope better. The context tokens affect the model’s attention weights. The LLM uses this context to bias the prediction by giving more importance to those tokens during generation.

- <mark>References || Really</mark> You show examples or previous responses that were good. It gives the model a pattern to follow. These can be 2–3 samples that set the tone or format. This activates in-context learning. The model recognises structure, language style, and logic flow from your examples and aligns its output by matching the embedded patterns.

- <mark>Evaluate || Excellent</mark> After getting the result, you verify if it met the need — was the tone right? Was the format followed? Did it make sense for the audience? Though not internal to the model, this step is feedback for your prompt quality. If output didn’t match expectations, it means token prediction wasn’t correctly guided — prompting needs adjustment.

- <mark>Iterate || Inputs</mark> If the output isn’t perfect, modify your prompt — add clarity, change wording, or split it into parts. Try again with better input. Even a small change in input text shifts how the model navigates through its layers. It changes token embedding paths and how predictions evolve — similar to re-routing logic flow.

## The Prompt is Code — Especially in Agent Design
I’ve been building a tool named [AdaptiveFuzz](https://github.com/vksundararajan/AdaptiveFuzz), a cybersecurity automation agent that performs **basic enumeration techniques** using an LLM backbone. Unlike traditional programming, where functions are deterministic and require strict syntax, **LLMs operate via intention expressed in text**.

With the right prompt + system design:
- The agent can access the browser, shell, or tools via ReAct,
- The orchestration is entirely driven by natural language,
- The chain of reasoning (CoT) helps the LLM **think before acting**.

For instance:
> You are a red team analyst. Use the terminal to list all users in this Linux system. Then analyse `/etc/passwd` for any suspicious accounts.

This acts like a function call. If the agent is connected to terminal tools, the LLM breaks down tasks step-by-step, issues commands, processes results, and iterates. The better the prompt, the more accurate the enumeration.

## Multi-Level Prompting: Roles, Context, System Control
Multi-Level Prompting outlines system prompting, role prompting, and contextual prompting as primary techniques:

- <mark>System Prompt</mark> Sets the model’s global behaviour.
> You are a JSON API. Only respond in JSON.

- <mark>Role Prompt</mark> Assigns a persona or domain role.
> You are an expert penetration tester working in a red team.

- <mark>Context Prompt</mark> Provides immediate task-level detail.
> Given the following user access logs...

These enable **modular instruction pipelines**, just like functions in traditional code — each part has intent, scope, and isolation.

## Chain of Thought, ReAct, Self-Consistency — Not Just Fancy Names
### Chain of Thought (CoT)
Prompts like:
> Let’s solve this step by step…

Force the model to **externalise its reasoning**, leading to better accuracy in multi-step problems like math or data processing.

### ReAct (Reason and Act)
Combines reasoning with real-world tool usage:

1. Think
2. Act (search, shell, API call)
3. Observe
4. Repeat
This is what powers real LLM agents.

### Self-Consistency
Run multiple CoT prompts → sample various reasoning paths → pick the most common answer.

It’s like ensemble learning, but in prompt space.

## Realisation: Prompt Engineering Is Software Engineering
The biggest shift in my thinking?

Prompt engineering isn’t just an art — it’s **programming with natural language**.
And as LLMs become more capable, prompt structures will become as critical as code architecture.

You:
- Define control flow with reasoning chains
- Manage memory with context
- Handle outputs with system constraints
- Tune response behaviour with sampling configs
For those of us building agents or intelligent assistants — this is now the foundational layer.

Prompt engineering isn't just about better answers — it's about unlocking LLM automation, agent autonomy, and scalable intelligence.

If you're building tools, testing ideas, or debugging workflows, get under the hood. Study the temperature. Tune the top-k. Structure your prompts.

Because behind every good LLM output... is a great engineer, writing better prompts.
