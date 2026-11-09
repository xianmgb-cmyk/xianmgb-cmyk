# AI intake review: a runnable work sample

**Xián Blanco Méndez · Blanco Méndez · 9 November 2026**

A small local AI prototype for turning synthetic invoice text into proposed structured records, then checking those proposals before a **simulated** business effect. The model proposes; separate rules decide whether the output matches a predeclared reference, needs review, or is a duplicate.

**This is a new public work sample, not a client deployment, CHAN evaluation, n8n R7 release, accounting product or security certification.** It contains no private AEGIS implementation, credentials, client records or financial data.

## Observed result

On 9 November 2026, **20/20 deterministic checks passed**. Separately, **three actual local Qwen3.5:9B responses matched the three synthetic references**, with one model attempt per input. Their observed durations were 40.343, 50.502 and 50.4968 seconds. These times include request/response overhead and are not a throughput benchmark. No time-saving claim follows: for these very short examples a deterministic parser could be much faster.

## What can be inspected

| Component | Evidence |
|---|---|
| Runnable program | [demo.py](demo.py), Python standard library only |
| Deterministic checks | 20 checks: malformed JSON, duplicate keys, unexpected fields, invalid amounts, mismatched reference/currency/amount, missing trusted reference and replay behavior |
| Actual model run | [evidence.json](evidence.json), including exact synthetic inputs, raw model outputs, decisions, timings and code/model identifiers |
| Code and data binding | Published source commit, SHA-256 of the executed source, frozen case-set hash and model digest |

The test cases and expected results are visible in the source. They were fixed before the recorded model run. These are **known, self-authored tests**, not a blind or independent evaluation. A source hash is a consistency check, not independent proof of execution. The full API response hash is recorded, but full response bytes are not distributed; the useful output and metadata are reproduced in the evidence file, without implying independent attestation.

## Run it

Python 3.10 or later. No packages are needed for the offline checks:

```sh
python demo.py
```

To run the three text-extraction examples against an **already installed, already running local Ollama model**:

```sh
python demo.py --live qwen3.5:9b
```

The program never downloads a model or installs anything. It sends only the three synthetic examples to `127.0.0.1:11434`; proxies and HTTP redirects are disabled. There are no tools, generated-code execution, production writes or customer uploads. Use a local model: a loopback URL alone does not prove that a separately configured backend will not route elsewhere. The recorded run used the installed local model identified in the evidence. The process prints JSON to standard output. Its simulated record store exists only in memory and disappears at exit.

The model receives a JSON schema using Ollama's [chat API](https://docs.ollama.com/api/chat) and [structured-output interface](https://docs.ollama.com/capabilities/structured-outputs). JSON formatting is **not** treated as evidence of factual correctness. The Python checker validates the returned values again.

## Three illustrative model inputs

English decimal amount, Spanish decimal comma, and a document containing an instruction to change the total. Each is attempted once. Failed or incomplete generations are recorded as `UNPROVEN`, without a silent retry or a fabricated fallback. These three short examples are not an extraction accuracy benchmark or a claim of prompt-injection resistance. The embedded instruction explicitly labels itself as malicious; it is an easy illustrative fixture, not an adversarial evaluation campaign.

`ACCEPT_SIMULATED` means agreement with the supplied synthetic reference. `DUPLICATE` means no second row is added to the same in-memory store. `REVIEW` means the automatic decision is unavailable or the repeat conflicts. `REJECT` means invalid output or a mismatch. Exit code 1 indicates a failed deterministic check; 2 indicates an unproven model call; 0 does **not** mean universal correctness or production readiness.

## Assumptions and limits that matter

The synthetic reference is trusted by construction. Real use would need an independently established reference or a human review step. This prototype does **not** prove that the reference is authoritative, fresh or correctly transcribed. Without it, the decision is `REVIEW`.

The store is single-process, non-durable and uses a deliberately simplified single-tenant invoice reference. There is no guarantee under concurrency, restart, multi-vendor collisions, currency exponent differences, revocation, network commit ambiguity or malicious local infrastructure. Only non-negative amounts are supported; credits are out of scope. The examples are plain text, not PDF/OCR. Three model calls cannot establish general quality, security or economic benefit.

For these tiny fixtures a deterministic parser could suffice. This sample demonstrates **how to review an AI-assisted workflow**, not that an LLM is necessary or cheaper. Customer time savings have not been measured. A commercial comparison must include setup, human checking, exceptions and rework on equivalent tasks.

## Authorship and contribution

This is work from Xián Blanco Méndez's owner-directed business and research workflow, developed with AI assistance. Xián set the commercial direction and authorized a public, reproducible demonstration. The assistant authored and executed this compact implementation and its self-tests. This is not a claim that Xián manually wrote every line, led a staffed client team, or delivered this to a paying customer.

The public sample is intentionally separate from private research. Earlier n8n and CHAN results are not included in its test count or attributed to this code.

## What this supports commercially

A bounded review of **one existing workflow**, with a sanitized input, an agreed expected result, reproducible failure cases, observations at the destination, and prioritized corrections. It supports a technical conversation and a small paid scope; it does not justify selling a comprehensive audit of an arbitrary agent.

Contact and existing services: [Blanco Méndez](https://www.blancomendez.com/).

---

## Resumen en español

Una muestra ejecutable, no una promesa: IA local para proponer datos; reglas separadas para comprobarlos; entradas, salidas y límites visibles. Pasaron los 20 controles deterministas y coincidieron las tres respuestas reales del modelo con sus referencias ficticias. Son mediciones distintas. No hay datos de clientes, pagos reales ni ahorro económico acreditado. El trabajo fue dirigido por Xián y desarrollado con asistencia de IA; la implementación concreta fue escrita y ejecutada por el asistente.
