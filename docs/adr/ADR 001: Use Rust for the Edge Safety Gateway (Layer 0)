#### Status: `Accepted`

## 1. Context
Every single user prompt enters our application through a front-door gateway before reaching our ML classifiers or the Claude API. Because 100% of traffic hits this layer, any delay or crash here slows down or breaks the entire app. In v1, this gateway has three focused jobs:

Rate limiting: Stopping users or bots from flooding the API.

Known-bad pattern matching: Instantly blocking obvious jailbreaks, banned strings, or injection templates.

Request hashing: Creating unique digital fingerprints of incoming prompts and user IDs.

## 2. Decision
We will build the Layer 0 gateway in Rust (keeping ML models in Python for Layer 1).

## 3. Why Rust Makes Sense Here
Near-zero latency overhead: Rust compiles directly to machine code with no garbage collector pauses. In modern LLM proxy benchmarks, Rust gateways add less than 1 millisecond of p99 latency compared to hundreds of milliseconds in Python proxies. Safe users feel zero delay.

Low memory footprint under spikes: A Rust gateway handles thousands of concurrent connections using roughly 20–30 MB of RAM. If traffic suddenly spikes, it won't crash with Out-Of-Memory (OOM) errors.

Predictable safety and reliability: Rust's compiler catches memory bugs and concurrency race conditions before the code ever runs, which is critical for a service sitting in front of every API call.

## 4. Why These Three Features Matter Most at the Edge
Keeping Layer 0 strictly limited to these three tasks gives us the highest protection for the lowest cost:

Rate Limiting (Protects Wallet & Uptime): LLM API calls are expensive. Rate limiting stops credential stuffing, scraping, and runaway scripts at the front door before they ever trigger costly Python compute or Claude API tokens.

Known-Bad Patterns (Blocks Obvious Attacks for Free): Running fast regex or string-set matching (like Aho-Corasick) in Rust takes microseconds. Catching known jailbreaks and banned phrases immediately saves us from wasting GPU/CPU cycles in Layer 1 or paying for an LLM judge in Layer 2.

Request Hashing (Enables Privacy, Caching & Forensics):

Privacy: Hashing user IDs lets us track abuse across requests without storing raw personal data.

Deduplication & Caching: Hashing the prompt lets us immediately spot repeated spam attacks or cache repeated safety verdicts so we don't pay to evaluate the exact same prompt twice.

## 5. Trade-offs We Accept
Slower initial development: Rust has a steeper learning curve and stricter compiler rules than Python or TypeScript. Writing and changing code takes more effort upfront.

Two-language stack complexity: Because our Layer 1 classifier is in Python, the team has to maintain two languages, two build pipelines, and network communication (HTTP/gRPC) between them.

Limited AI ecosystem: Rust has fewer mature ML libraries than Python, which is why we strictly avoid running models in the gateway and limit Rust to networking, hashing, and pattern matching.
