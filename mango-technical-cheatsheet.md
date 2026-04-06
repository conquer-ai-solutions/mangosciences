# Mango Sciences — Technical Cheat Sheet v2

*How to handle every technical question on today's call — with ready-to-go responses drawn from real Conquer engagements.*

*Synthesized from 12 client engagements: Playtech (enterprise agentic platform), Lumanity (biopharma enablement), News UK (architecture), Einstein (healthcare data sovereignty), Suvoda (clinical trial accuracy), YBS (data platform/RAG), Azets (unstructured data at scale), AB (asset management), Inchcape (intro/discovery), Pacific Life (process discovery), EWB (POC scoping), Mango Sciences first call*

---

## 1. WHEN THEY ASK ABOUT TECH STACK / "WHAT DO YOU BUILD IN?"

Tom's principle: **tech agnostic, client-first.** He never leads with a stack. He asks about theirs first, then adapts.

**How Tom handles it (from AB):**
> "We're very tech agnostic. Your tech team might tell us, OK, we build everything on Azure or AWS, we build everything in Python, we use ChatGPT models. They might say these are all the things it needs to do. And if you come to us and say it needs to be this stack, we can adhere to that."

**For Mango today — when Hanif talks about their stack:**

Ask first: *"Before we get into what we'd recommend, can you walk us through what you're currently running on? Cloud provider, primary languages, what your data layer looks like, what models you've experimented with?"*

Then adapt. The key phrase is: *"The spirit of how we work is — how do we set your team up to own and operate this in the longer term. Whatever we build will run in your cloud environment, it's your source code, your IP."*

If they ask what Conquer prefers: *"We tend to reach for Python, we're model-agnostic — we'll use Claude, GPT, open-source models, whatever fits the use case. For data-heavy work like yours, we'd typically look at what gives you the best extraction accuracy for your specific data types, and we'd benchmark that early."*

---

## 2. WHEN THEY TALK ABOUT DATA HARMONISATION ARCHITECTURE

This is Mango's core obsession — 2.7M messy EHR records, ontology-based NLP, moving toward multimodal.

**What we know from Azets (structuring unstructured data at scale):**
Azets dealt with 100K+ SME clients worth of fragmented data. Their staged approach:
- Stage 1: Auto-validation (deterministic rules catch obvious issues)
- Stage 2: Pre-flight checklist (AI flags anomalies before human review)
- Stage 3: Human-in-the-loop for edge cases
- Stage 4: Self-service automation once confidence is high enough

**What we know from Suvoda (accuracy cascades in healthcare):**
Suvoda's V2 accuracy scores tell a critical story:
- Static/known cases: **99.9%** accuracy
- Semi-novel cases: **71.6%** accuracy
- Truly novel cases: **41.4%** accuracy

The lesson: **accuracy degrades dramatically as novelty increases.** Mango's agentic pipelines failed because they threw agents at the hardest cases first.

**What we know from Lumanity (biopharma data processing):**
Lumanity's Knowledge FRaiME was built to ingest ~50 complex biopharma documents, query them with citations, and export structured outputs. The insight: even in pharma, you need a **staged confidence pipeline** — deterministic rules first, then AI-assisted extraction, then human review for edge cases. The key deliverable their team valued most was **citation-backed answers** — every extracted fact traceable to a source document.

**For Mango today — when Hanif describes their data challenge:**

*"That accuracy cascade is something we've seen across multiple healthcare engagements. When you throw agents at truly novel data, accuracy drops off a cliff — we've seen it go from 99% on known patterns to 40% on novel ones. So what we've found works is a tiered approach: start with the data you can validate deterministically, use agents to accelerate the middle tier where you have partial structure, and keep humans on the genuinely novel cases. Over time, as the agent sees more confirmed examples, that middle tier expands and the human-only tier shrinks."*

*"The ontology-first approach you're taking is actually the right foundation — you need the semantic framework before agents can reason about the data. The question is: where in your current pipeline does the ontology break down? Is it at entity extraction, at relationship mapping, or at harmonisation across different source formats?"*

---

## 3. WHEN THEY ASK ABOUT AGENTIC PLATFORM ARCHITECTURE

This is where Playtech gives us the most credible framework. Mango wants a flywheel — the capability to rapidly create applications. That's an **enterprise agentic platform**, not a single agent.

**The Playtech three-layer model (what we built with them):**

> "The enterprise agentic platform exists across three layers. There's the **orchestration layer** — how agents are deployed, coordinated, lifecycle managed. There's the **services layer** — shared capabilities like memory, data access, model routing. And there's the **governance layer** — permissions, logging, cost controls, release management."

**The News UK deterministic/probabilistic split:**
Within each layer, Tom splits components into deterministic (must be bulletproof) and probabilistic (accept non-determinism, but contain it with validation gates).

**The Playtech lifecycle problem (directly applies to Mango):**
> "Somebody has to have an agent version control story, somebody has to have an agent promotion story, somebody has to have an agent logging, monitoring story. When you've got 50 business units building or running 10 agents each, all with different life cycles, all of which need to be version controlled — within 3 months you're gonna have 5000 agents."

Mango won't have 5000 agents, but they will have multiple agents across harmonisation, annotation, similarity, clinical trials, and eventually multimodal processing. The lifecycle question matters early.

**For Mango today — when they ask what an agentic data platform looks like:**

*"The way we think about it — and we've built this for other enterprise clients — is three layers. First, the orchestration layer: how do your agents get deployed, versioned, coordinated? For you that's the harmonisation pipeline, the annotation workflows, the clinical trial processing. Second, the services layer: shared capabilities that every agent needs — access to your patient data, your ontology, your model routing, your cost tracking. Third, the governance layer: who can deploy what, what gets logged, how do you audit what the AI did and why."*

*"Within each layer, there's a split between deterministic and probabilistic components. Your data pipeline, your ontology rules, your validation checks — those need to be bulletproof. The LLM calls that do extraction, classification, harmonisation — those sit inside the deterministic framework with validation gates around them. So an agent can attempt to harmonise a record, but a validation step checks it against your ontology before it's committed. If confidence is below threshold, it routes to a human annotator."*

*"This is exactly the flywheel Mohit described — you're not building one application, you're building the platform that makes every subsequent application faster to create, cheaper to run, and safer to operate."*

---

## 4. WHEN THEY RAISE DATA PRIVACY / REGULATORY COMPLIANCE

Cancer patient data from India, US-headquartered company, moving into clinical trials. This is a multi-jurisdictional compliance challenge.

**How Tom handles it (from Einstein — healthcare in Brazil with cross-border restrictions):**
> "All the data stays in your estate, none of your data ever leaves your estate. We build the solution ourselves in-house and then work with your development team to get it integrated into your cloud environment. Ultimately what we build is your intellectual property, your source code."

**What we know from Lumanity (enterprise LLM governance):**
Lumanity needed Anthropic stack deployed for 30-70 users with:
- **RBAC** — role-based access so not everyone sees everything
- **Environment segmentation** — sandbox/dev/prod isolation
- **Privacy controls** — preventing accidental upload of sensitive data to LLM APIs
- **Audit trails** — compliance logging of every AI interaction

**What we know from Playtech (AI security at enterprise scale):**
Playtech explicitly tested our knowledge on:
- **Prompt injection** — malicious instructions embedded in data
- **Data poisoning** — compromised training/reference data
- **Hallucination** — plausible but fabricated outputs
- **Content safety** — outputs that could cause legal/reputational harm

For oncology, all four of these matter intensely. A hallucinated drug interaction or fabricated clinical outcome is not just wrong — it's dangerous.

**For Mango today — when privacy comes up:**

*"With patient data — especially oncology data at this scale — we'd never propose an architecture where data leaves your environment. Everything we build runs in your cloud, your VPC. The models we use for extraction and harmonisation would either be deployed within your infrastructure, or we'd use API calls where only the minimum necessary context is sent — and we'd work with your compliance team to define exactly what that boundary looks like."*

*"We've worked with healthcare providers where data sovereignty meant certain data couldn't cross borders at all. Our approach: the AI runs where the data lives, not the other way around. If your Indian patient data needs to stay in India, we build the pipeline to run there."*

*"On the governance side — and this is something we'd design into the platform from day one, not bolt on later — you need RBAC so not every user sees every patient record, environment segmentation so dev work never touches production data, audit trails so you can show a regulator exactly what the AI did with any given record, and safety rails around hallucination and content accuracy. We've built all of those for other enterprise clients."*

If they push on specifics: *"The compliance architecture is something we'd work through with your team in the technical deep-dive after the alignment doc. We need to understand your specific constraints — HIPAA applicability given you're US-headquartered, Indian data protection requirements for the source data, and clinical trial regulations for the CRO work. We've navigated all of those individually; the question is how they intersect for your specific setup."*

---

## 5. WHEN THEY ASK ABOUT LLM SELECTION / "WHICH MODELS?"

**How Tom handles it (from Einstein — evaluating multiple platforms):**
Einstein was running identical use cases across multiple LLM providers to evaluate. Tom didn't push a specific model — he positioned Conquer as the team that helps evaluate and choose.

**What we know from Suvoda (fine-tuning in healthcare):**
Fine-tuned small models beat general-purpose large models for extraction tasks. But accuracy still drops on truly novel cases. The approach: use the right model for each task type, not one model for everything.

**What we know from Lumanity (Anthropic stack for biopharma):**
Lumanity is deploying Anthropic's Claude as their primary LLM for biopharma consulting work — insights factbook generation, document querying with citations, export to structured formats. The key: Claude's longer context window and citation capabilities were the differentiator for their use case.

**For Mango today:**

*"Model selection is something we'd benchmark early in delivery, not assume upfront. For data harmonisation specifically, we'd likely test a few approaches: a frontier model like Claude for complex reasoning tasks — understanding clinical context, resolving ambiguous records — potentially a fine-tuned smaller model for high-volume extraction where you need consistency and lower cost, and your existing NLP pipeline for the deterministic parts that are already working."*

*"One thing we've learned in healthcare specifically: for extraction tasks, a fine-tuned small model often outperforms a general-purpose large model. We had a case where we fine-tuned a model specifically for document extraction and it ended up being more accurate than the humans doing it manually. The key was: we could trace every extracted value back to a citation in the source document, so the reviewer could validate in seconds rather than re-reading the whole thing."*

*"For the ontology work — mapping messy data to structured cancer ontologies — that's where frontier models earn their keep. They can handle the ambiguity, the abbreviations, the inconsistent terminology across different hospitals and countries. But you'd want a smaller, cheaper model for the high-volume stuff like deduplication and format normalisation."*

---

## 6. WHEN THEY TALK ABOUT PATIENT SIMILARITY / GAVIN'S TRADITIONAL MODELS

Gavin has done significant work on patient similarity using traditional ML. He'll be protective of this work.

**For Mango today:**

*"The patient similarity work is really interesting because it's a perfect example of where traditional models and agentic AI complement each other. Your traditional similarity models give you the mathematical rigour for cohort building. What agents can add is the ability to pull in unstructured context — clinical notes, imaging reports, genomic data — that your current models can't easily consume. So the agent enriches the input to your similarity engine rather than replacing it."*

*"The question for us would be: what data is your similarity model currently blind to that a clinician would consider relevant? That gap is usually where agents add the most value."*

---

## 7. WHEN THEY ASK ABOUT CLINICAL TRIALS (CRO PARTNERSHIP)

**From Suvoda — what we know about clinical trial AI:**
- Test script generation with 99.9% accuracy on known patterns
- Accuracy drops to 41.4% on novel protocols
- **Hallucination is the critical risk** — plausible but ungrounded outputs in regulated domains
- Human-in-the-loop validation is non-negotiable
- Fine-tuning helps but doesn't solve truly novel cases

**For Mango today:**

*"We've got several clients in the clinical trial space — the NDAs mean we can't say their name, let alone what we built. But the domain is one we understand deeply. The core challenge is always the same: you need AI that's accurate enough to trust with patient-affecting decisions, but honest enough to flag when it doesn't know. In clinical, a confident wrong answer is worse than no answer at all."*

*"For your first two trials — the question I'd ask is: which parts of the trial workflow are administrative burden versus clinical judgement? Site selection, patient matching, regulatory document processing — those are high-volume, pattern-heavy tasks where agents can accelerate. Protocol design and safety monitoring — those need human judgement with AI support, not AI autonomy."*

---

## 8. WHEN THEY ASK ABOUT MULTIMODAL (GENOMICS, RADIOLOGY/PATHOLOGY)

**For Mango today:**

*"Multimodal is where things get really interesting and really hard at the same time. Each modality has its own noise characteristics, its own standards — or lack of standards — and its own regulatory requirements."*

*"Our view: don't try to build a unified multimodal pipeline from day one. Get the text and structured data harmonisation rock-solid first — that's your foundation. Then add genomic data as a second modality, because it's structured even if complex. Imaging comes last because it requires the most specialised models and the validation is hardest."*

*"The flywheel Mohit described — this is exactly how you get there. Build the platform for text harmonisation, prove the pattern works, then the same platform architecture extends to new modalities. Each one is a new application on the same three-layer foundation."*

---

## 9. WHEN THEY ASK "HOW LONG / HOW MUCH?"

Don't price anything. This is a discovery call.

**If they push:**

*"Honestly, it's too early to put numbers on it — we need to understand the roadmap in detail first. What I can tell you is our typical first engagement is 8-12 weeks for a focused proof of value. But the right answer comes out of the alignment document, where we scope it together based on what actually matters most to you."*

*"We do all of discovery at our risk, for free. And when we move to delivery, we don't invoice until the thing is built and it's doing what you expect it to do. All the risk is on our side — we need to earn our stripes."*

---

## 10. THE "BUILD VS BUY" QUESTION

**What we know from Playtech (explicit buy-vs-build framework):**
Playtech's position was clear: *"If there's going to be custom code, we're going to own that custom code. However, from the buy versus build perspective, I would rather buy if there was something that's going to do the job."*

The principle: **buy infrastructure, build intelligence.** Standard orchestration, standard compute, standard storage — but the AI components that are specific to your domain get built bespoke.

**What we know from News UK:**
> "Buy the orchestration layer if something good exists, build the AI components that are specific to your domain. Don't reinvent infrastructure, but don't buy generic AI tools when your problem is domain-specific."

**For Mango today:**

*"For annotation tooling — there are off-the-shelf platforms, but the question is whether they understand oncology data. If your annotation team is correcting AI-harmonised records against cancer-specific ontologies, you probably need something purpose-built. Not from scratch — a custom interface on top of standard components that speaks your domain language."*

*"The general principle: buy your infrastructure — compute, storage, orchestration. Build the intelligence — the harmonisation logic, the ontology mapping, the clinical reasoning that's specific to your data and your domain. That's where the value lives and that's what should be your IP."*

---

## 11. WHEN THEY ASK ABOUT ENABLEMENT vs. BUILD

This is a Lumanity pattern that may come up. Mango might not just want us to build something — they might want us to help their team build better.

**What we know from Lumanity:**
Lumanity's original engagement was a product build (Knowledge FRaiME). It failed an internal gate. The pivot: instead of building one product, Conquer offered **Anthropic stack enablement** — install the LLM infrastructure, set up RBAC and governance, train their team to build on it safely. This became a more valuable engagement because it gave them capability, not just an application.

**What we know from Playtech (non-technical user enablement):**
Playtech rolled out Cursor to non-technical staff and saw massive organic adoption: *"We made a significant effort to say, hey, all of you non-technical people, come along, here's a demo of how to make it work. It's been wildly successful — people are continuing to use it, and now we're seeing non-technical areas of the company having really great ideas for products and productivity tools."*

The challenge: those non-technical users then hit DevOps/git/security walls that didn't exist centrally. The enablement needed a governance layer underneath it.

**For Mango today — if they hint at wanting capability, not just a deliverable:**

*"One thing we've seen with companies at your stage — sometimes the most valuable thing isn't building one application, it's building the capability to build many. We've done this with other clients where instead of building a single use case, we installed the AI infrastructure — models, governance, access controls, development patterns — and then enabled their team to build on top of it. The flywheel you're describing suggests that might be the right model here."*

*"The question is: does your team want to be building agents themselves in 6 months, or do you want us to keep building them for you? The answer changes the architecture and the engagement model quite a bit."*

---

## 12. WHEN THEY ASK ABOUT AI SECURITY / HALLUCINATION / TRUST

Playtech explicitly stress-tested us here. In oncology, this is even more critical.

**What we know from Playtech (the security gauntlet):**
Playtech's Adam Mitchell deliberately probed: *"How do I protect against prompt injection? How do I know data hasn't been poisoned? How do I know my system doesn't hallucinate? How do I know it doesn't give a stupid answer that's going to get me sued?"*

The four threats: prompt injection, data poisoning, hallucination, harmful/inaccurate content.

**What we know from Suvoda (hallucination in clinical):**
The specific failure mode: AI generates outputs that look clinically plausible but are **fabricated** — not pulled from source data, but synthesized from training patterns. In Suvoda's case, novel test cases showed 41.4% accuracy. The remaining 58.6% wasn't all wrong — some of it was plausible-sounding but ungrounded.

**For Mango today:**

*"In oncology, AI security isn't a nice-to-have — it's foundational. We think about four threat vectors: prompt injection — can someone embed malicious instructions in the data the agent is processing? Data poisoning — if the training data or reference ontology has errors, the agent inherits them. Hallucination — the agent generates something that looks clinically plausible but isn't grounded in your actual data. And content safety — making sure the output meets clinical and regulatory standards."*

*"For each of those, the mitigation is different. Prompt injection is an architecture problem — you sanitise inputs before agents see them. Data poisoning is a data quality problem — your ontology and validation pipeline are the defence. Hallucination is solved through citations — every output the agent produces should be traceable back to a source record. And content safety is a governance problem — you define what the agent is and isn't allowed to output, and you build guardrails around it."*

*"We've built all four of these defence layers for other enterprise clients. The oncology-specific piece is making sure your medical ontology is the source of truth — if the agent can't cite a specific record or ontology entry for its output, it should flag uncertainty rather than fabricate an answer."*

---

## 13. HANDLING THE "FRAGMENTED ESTATE" PROBLEM

Mango has data from multiple hospitals, multiple formats, across multiple countries. This mirrors Playtech's acquisition-driven fragmentation.

**What we know from Playtech:**
> "I describe Playtech as 50 companies in a trench coat because we've grown through acquisition. We don't have almost any common IT stack across all of these units. There's 80 development teams — the largest group of commonality is probably about 9. Everyone else is a completely different git solution, build server, programming language, target audience, technology stack."

Their solution: **platform above the fragmentation** — a central enablement layer with decentralised execution. Don't force convergence, abstract above the chaos.

**For Mango today — when they describe data coming from dozens of hospitals in different formats:**

*"We've dealt with this exact pattern — a client with 80 different development teams, completely different tech stacks across all of them, grown through acquisition. The answer isn't forcing everyone onto one standard. It's building a platform layer that sits above the fragmentation. You define the target data model — your harmonised ontology — and then build adapters for each source format. The platform handles the routing, the transformation, and the quality checks. Each new data source is a new adapter, not a new architecture."*

*"For you, that means: each hospital, each country, each EHR system gets an ingestion adapter. The adapters do the initial normalisation. Then the harmonisation agents work on partially-normalised data against your ontology. The annotation team handles what the agents can't resolve. Same platform, different inputs."*

---

## QUICK REFERENCE: TECHNICAL PHRASES TO USE

| Situation | What to say |
|-----------|-------------|
| They describe messy data | "Is the messiness at the entity level, the relationship level, or the cross-source harmonisation level?" |
| They mention ontology | "What's the ontology coverage like? Are you mapping to SNOMED, ICD, or a custom cancer-specific ontology?" |
| They ask about models | "We'd benchmark early — different tasks need different models. Extraction wants a fine-tuned small model, reasoning wants a frontier model." |
| They mention failed agentic attempts | "What specifically failed — was it accuracy, hallucination, or the agent doing plausible-looking but wrong things?" |
| They ask about platform architecture | "Three layers: orchestration, services, governance. Deterministic foundations with probabilistic AI contained inside validation gates." |
| They ask about cost | "Too early to scope — but our first engagements are typically 8-12 weeks, and we do discovery at our risk." |
| They worry about data privacy | "Everything runs in your environment. The AI goes to the data, not the other way around." |
| They mention multimodal | "Text and structured first, genomics second, imaging last. Each modality is a new app on the same platform." |
| They ask about annotation tooling | "Standard infrastructure, domain-specific interface. Annotators need confidence scores and source citations." |
| They mention clinical trials | "Deep experience under NDA. Key split: admin burden vs clinical judgement — agents accelerate the first, support the second." |
| They ask about accuracy | "Accuracy degrades with novelty. Start with known patterns, prove it there, then expand. We've seen 99% on known drop to 40% on novel." |
| They ask about hallucination | "Every output should be citation-backed. If the agent can't point to a source record, it should flag uncertainty, not fabricate." |
| They worry about fragmented data sources | "Don't force convergence. Platform above the fragmentation — target ontology + source adapters + quality gates." |
| They ask if we can enable their team | "We can build for you, or we can build the platform and enable your team to build on it. The flywheel answer is usually the second." |
| They ask about security | "Four threats: prompt injection, data poisoning, hallucination, content safety. Each has a different architectural mitigation." |
| They ask about agent lifecycle | "Version control, promotion, logging, monitoring — all designed in from day one, not bolted on when you have 50 agents." |
