+++
title = 'The Middleman Pipeline'
date = '2026-06-08'
tags = ["ai","security",]
+++

<b>TL;DR:</b> We can protect our private data when using cloud AI by hiding it among highly realistic "fake" data. To keep this fast and cheap, a central "middleman" server groups similar user requests together into one single batch before sending them to the AI.

I spend a lot of time thinking about the gap between artificial intelligence and security. We all want to use powerful cloud AI models to fix our code, summarize medical records, or analyze financial data. The problem is that sending this private data to an external server is a huge privacy risk.

For a while, the security community has tried to fix this by altering or scrambling the user's prompt before it leaves their computer. But this always creates new problems. If you change the prompt too much, the AI gives a bad answer. If you ask a local AI to generate fake prompts to hide your real one, the cost of sending all those extra prompts to the cloud gets way too high. Plus, waiting for all those answers makes the app feel incredibly slow.

I want to share a different thought process and a solution I have been exploring. It relies on separating the heavy instructions from the private data, and using a smart "middleman" relay.


## The Core Idea: Instruction Separation

Generating a complete, fake paragraph that sounds exactly like a real user is nearly impossible to do perfectly every time. So, we do not do it.

Instead, our local app strips the user's prompt down to a generic template. Imagine a user types: <b>"</b>Summarize the medical history for patient John Doe, diagnosed with Lupus.<b>"</b>

The local engine removes the sensitive parts. It creates a blank template: <b>"</b>Summarize the medical history for patient [NAME], diagnosed with [DISEASES].<b>"</b>

The sensitive variables (John Doe, Lupus) are locked away safely on the user's computer. We do not even need to build this extraction process from scratch. We can use reliable, open-source tools like Microsoft Presidio to automatically detect and pull out this kind of sensitive data right on the local machine.

## Creating the Perfect Noise

Now we need to hide that real data. The local engine generates a few fake variables that belong to the exact same medical category. For example, it might generate "Jane Smith with Celiac Disease" and "Robert Roe with Rheumatoid Arthritis."

Because these fake variables are from the exact same context as the real one, any anomaly detection system on the enterprise AI side will just see three perfectly normal, plausible questions. The real data is hidden in plain sight.

## The Middleman Relay

Sending three separate prompts to a paid AI API triples your cost. This is where the central relay, or the "middleman", saves the project.

Instead of sending full prompts, the middleman server waits a fraction of a second to group requests from different users who have similar needs. Let's say three different users are all asking the AI to find bugs in some code. The middleman takes their blank template and batches all of their code snippets (both real and fake) into a single payload.

It sends the enterprise AI one single command: "Find the bugs in the code for the following boxes:" followed by a list of the variables.

## Why This is a Game Changer?

The external AI only reads the heavy instructions once. This saves massive amounts of compute and money. It processes the variables in parallel and sends a list of answers back to the middleman.

The middleman looks at its temporary memory, routes the correct answers back to the correct users, and then immediately wipes its memory clean.

When your local app receives the bundle of answers, it simply throws away the answers for the fake data. You are left with the exact answer you needed, generated in less than a second, and the enterprise AI never knew which data was actually real.

By separating the instruction from the variables, we guarantee the fake data is high quality, and we allow the central relay to compress the data. It solves the privacy problem, the speed problem, and the cost problem all at once.

<I>Reference: This thought process builds upon concepts explored in "SharedRequest: Privacy-Preserving Model-Agnostic Inference for Large Language Models" by Peihua Mai, et al.</I>