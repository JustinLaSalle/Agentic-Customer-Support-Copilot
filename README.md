# Support Copilot

A live, interactive AI customer support agent — built with the [Claude API](https://docs.claude.com), with a working chat demo, not just a script.

## Live demo

**[Try it here](https://claude.ai/artifact/SAcYXTdW1DBzW79r9zKYon)** — note: trying the interactive part requires a free Claude account to sign in with (this is a platform requirement for any page that calls Claude live, not something specific to this project).

## The problem

Support teams handle a high volume of requests that are genuinely simple to resolve — a refund within policy, a password reset, a straightforward cancellation — but still take a person's time to read, verify, and answer. The goal isn't to replace judgment on hard cases; it's to handle the clear-cut ones confidently and instantly, and know when to hand off the ones that actually need a person.

## What it does

Given a customer message, the agent:

1. **Checks a simulated account and a short policy knowledge base** — its only source of truth. It's explicitly instructed never to invent an account detail or policy that isn't in that data.
2. **Shows its reasoning as it works** — a visible step-by-step trace ("Checked account details," "Searched knowledge base for refund policy") appears before the reply, so the process isn't a black box.
3. **Decides: resolve directly, or escalate to a human** — policy-covered requests (refunds within the window, resets, cancellations) get resolved immediately; genuine anger, ambiguity, or anything the knowledge base doesn't clearly cover gets escalated instead. A badge on each reply shows which happened.
4. **Remembers the conversation** — every message includes the full conversation so far, not just the latest line, so follow-ups and confirmations ("yes, please go ahead") are understood in context instead of the agent losing track and escalating out of confusion.

## A bug worth mentioning

Early versions of this had no conversation memory — each message was sent to the model in total isolation, so a customer confirming something from two messages ago ("yes please") looked like a random, context-free statement to the agent, and it correctly escalated for lack of information. The fix was to track the full exchange client-side and include it in every request. This is a genuinely common real-world agent bug, not a contrived one, and worth being able to explain in an interview.

## How it works

- Single Claude API call per message, using structured JSON output (steps, resolution, reply)
- The simulated account and knowledge base sit directly in the prompt — in a real deployment, these would be swapped for a live CRM/helpdesk API call
- Client-side conversation history array, rebuilt into the prompt on every turn, giving the agent real multi-turn memory despite the underlying API being stateless

## Tech

- HTML / CSS / JavaScript (single file, no build step)
- Claude API, via Anthropic's Artifact runtime
- Structured JSON output, multi-turn context management
