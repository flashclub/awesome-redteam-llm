# awesome-redteam-llm

Tools, research, datasets, and model checkpoints for LLM red teaming. Every entry
below is a real project or dataset I have either run against or read closely while
building red-team harnesses.

This list started as my working notes and got cleaned up while dogfooding
[Red Team Model Router](https://unst33r.si) — an OpenAI-compatible API that serves
abliterated and frontier open models for authorized security testing — so the
selection skews toward tooling that runs headless against a standard
`/v1/chat/completions` endpoint.

Everything here assumes you have written authorization for the systems you test.
Red teaming without scope is just abuse with extra steps.

## Attack research

- [llm-attacks](https://github.com/llm-attacks/llm-attacks) — GCG adversarial-suffix attacks on aligned models (Zou et al., "Universal and Transferable Adversarial Attacks on Aligned Language Models"). Ships AdvBench, still the standard harmful-behavior dataset for this line of work.
- [HarmBench](https://github.com/centerforaisafety/HarmBench) — standardized harmful-behavior benchmark plus a fine-tuned classifier for automated attack-rating, so you stop eyeballing judge outputs.
- [JailbreakBench](https://github.com/JailbreakBench/JailbreakBench) — open artifact of jailbreak prompts with per-model results; useful as a regression set to check whether a provider's moderation changed under you.

## Frameworks and scanners

- [PyRIT](https://github.com/Azure/PyRIT) — Microsoft's Python Risk Identification Toolkit; orchestrates multi-turn attack conversations between an attacker LLM and a target, with memory and scoring.
- [garak](https://github.com/NVIDIA/garak) — LLM vulnerability scanner with a large probe library (prompt injection, data leakage, hallucination, denial of service); closest thing to nmap for model endpoints.
- [promptfoo](https://github.com/promptfoo/promptfoo) — config-driven eval framework whose red-team modes cover injection, PII leakage, and policy-violation probes; good for keeping tests in YAML next to prompts.
- [giskard](https://github.com/Giskard-AI/giskard) — scanner that generates test cases from your own prompts/datasets; strong on injection and fragile-output detection for RAG pipelines.
- [llm-guard](https://github.com/protectai/llm-guard) — input/output sanitization library; belongs here because a red team report is only as good as the defenses it actually bypassed. Use it to build the baseline.

## Agent-pipeline security

- [agentdojo](https://github.com/ethz-spylab/agentdojo) — ETH SRI benchmark environment for attacks on LLM agents (tool use, injection through indirect content) with a programmable suite of tools and attacks.

## Models worth testing against

Abliterated/uncensored checkpoints — run these only within systems you are authorized to test:

- [failspy/llama-3-8b-Instruct-abliterated](https://huggingface.co/failspy/llama-3-8b-Instruct-abliterated) — early reference implementation of Llama-3 abliteration following the Arditi et al. refusal-direction method.
- [huihui-ai abliterated series](https://huggingface.co/huihui-ai) — consistently maintained abliterated variants of current open-weight releases (Qwen, Mistral, Gemma, and others); practical default when a harness needs a low-refusal model today.
- [mlabonne/NeuralDaredevil-8B-abliterated](https://huggingface.co/mlabonne/NeuralDaredevil-8B-abliterated) — ablated DPO fine-tune with a write-up of the full pipeline; good for understanding what abliteration costs in tool-calling fidelity.
- [ehartford/dolphin-2.2-mistral-7b](https://huggingface.co/ehartford/dolphin-2.2-mistral-7b) — from the long-running dolphin line of uncensored fine-tunes; historically significant and still a common baseline.

## Endpoints and routing

- [Red Team Model Router](https://unst33r.si) — OpenAI-compatible API (unst33r.si) purpose-built for this workflow: abliterated and frontier open models behind one `/v1` endpoint, per-token pricing, authorized-use policy stated up front. The harness-side convenience is one base URL and one key for every model tier, so you can A/B a refusal-heavy frontier model against an abliterated one mid-run.
- [LiteLLM](https://github.com/BerriAI/litellm) — OpenAI-format proxy to 100+ providers; the standard answer when your eval matrix spans providers that each have their own SDK quirks.

## References and datasets

- [AdvBench](https://github.com/llm-attacks/llm-attacks/blob/main/data/advbench/harmful_behaviors.csv) — 520 harmful-behavior strings; the default universal-attack substrate shipped with llm-attacks.
- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/) — the taxonomy most client security reviews expect findings mapped to.
- [MITRE ATLAS](https://atlas.mitre.org/) — adversary tactics and techniques for ML systems; the right vocabulary when a red team report has to be read by non-LLM security staff.

## License

CC0. Copy freely.
