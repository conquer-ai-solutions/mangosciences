# Mango Sciences — Technical Cheat Sheet

*How Tom handles stack, architecture, data, and privacy questions — with Mango-specific responses ready to go.*

*Synthesized from: News UK (architecture), Einstein (healthcare data sovereignty), Suvoda (clinical trial accuracy), YBS (data platform/RAG), Azets (unstructured data at scale), EWB (POC scoping)*

---

## 1. WHEN THEY ASK ABOUT TECH STACK / "WHAT DO YOU BUILD IN?"

Tom's principle: **tech agnostic, client-first.** He never leads with a stack. He asks about theirs first, then adapts.

**How Tom handles it (from AB call):**
> "We're very tech agnostic. Your tech team might tell us, OK, we build everything on Azure or AWS, we build everything in Python, we use ChatGPT models, we use blah blah blah. They might say these are all the things it needs to do. And we might say, for this we recommend this model — you might say, oh, we already have an agreement with OpenAI, so we'd rather you use that. So if you come to us and say it needs to be this stack, we can adhere to that."

**How Tom frames it (from Inchcape call):**
> "Once we've got an agreed alignment document, we might come back to you and say, look, this is our recommended technology stack based on our experience. Does this align with what you've got? So we know you're an Azure shop straight off the bat, OK, so we'll definitely build all of this in Azure."

**For Mango today — when Hanif talks about their stack:**

Ask first: *"Before we get into what we'd recommend, can you walk us through what you're currently running on? Cloud provider, primary languages, what your data layer looks like, what models you've experimented with?"*

Then adapt: If they're on AWS, say AWS. If they're using specific models, acknowledge them. The key phrase is: *"The spirit of how we work is — how do we set your team up to own and operate this in the longer term. So whatever we build will run in your cloud environment, it's your source code, your IP."*

If they ask what Conquer prefers: *"We tend to reach for Python, we're model-agnostic — we'll use Claude, GPT, open-source models, whatever fits the use case. For data-heavy work like yours, we'd typically look at what gives you the best extraction accuracy for your specific data types, and we'd benchmark that early."*

---

## 2. WHEN THEY TALK ABOUT DATA HARMONISATION ARCHITECTURE

This is Mango's core obsession — 2.7M messy EHR records, ontology-based NLP, moving toward multimodal. You need to sound credible here.

**What Tom knows from Azets (structuring unstructured data at scale):**
The Azets engagement dealt with 100K+ SME clients worth of fragmented data across document types. Their approach:
- Stage 1: Auto-validation (deterministic rules catch obvious issues)
- Stage 2: Pre-flight checklist (AI flags anomalies before human review)
- Stage 3: Human-in-the-loop for edge cases
- Stage 4: Self-service automation once confidence is high enough

**What Tom knows from Suvoda (accuracy cascades in healthcare):**
Suvoda's V2 accuracy scores tell a critical story:
- Static/known cases: 99.9% accuracy
- Semi-novel cases: 71.6% accuracy
- Truly novel cases: 41.4% accuracy

The lesson: **accuracy degrades dramatically as novelty increases.** For Mango, this means their agentic pipelines failed because they threw agents at the hardest cases (messy, novel data) first. The approach should be: start with the most structured/predictable data, prove accuracy there, then progressively tackle messier categories.

**For Mango today — when Hanif describes their data challenge:**

*"That accuracy cascade is something we've seen across multiple healthcare engagements. When you throw agents at truly novel data, accuracy drops off a cliff — we've seen it go from 99% on known patterns to 40% on novel ones. So what we've found works is a tiered approach: start with the data you can validate deterministically, use agents to accelerate the middle tier where you have partial structure, and keep humans on the genuinely novel cases. Over time, as the agent sees more confirmed examples, that middle tier expands and the human-only tier shrinks."*

*"The ontology-first approach you're taking is actually the right foundation — you need the semantic framework before agents can reason about the data. The question is: where in your current pipeline does the ontology break down? Is it at entity extraction, at relationship mapping, or at harmonisation across different source formats?"*

---

## 3. WHEN THEY ASK ABOUT AGENTIC READINESS / "WHAT DOES THE ARCHITECTURE LOOK LIKE?"

**How Tom thinks about architecture (from News UK):**
Tom splits every system into **deterministic** and **probabilistic** components:
- Deterministic layer: orchestration, routing, validation rules — must be bulletproof
- Probabilistic layer: LLM calls, extraction, classification — where you accept non-determinism but contain it

> "There's a natural boundary between deterministic and probabilistic. The orchestration — the workflow, the routing, the scheduling — that needs to be rock-solid. The AI components sit inside that as probabilistic steps with validation gates around them."

**For Mango today — when they ask what an agentic data platform looks like:**

*"The way we think about it is there are two layers. There's the deterministic layer — your data pipeline, your ontology rules, your validation checks — that needs to be bulletproof. Then there's the probabilistic layer — the LLM calls that do extraction, classification, harmonisation — and those sit inside the deterministic framework with validation gates around them. So an agent can attempt to harmonise a record, but a validation step checks it against your ontology before it's committed. If confidence is below threshold, it routes to a human annotator."*

*"For your annotation team specifically — the interface they work with should be showing them the agent's work with confidence scores and citations back to the source data. Not asking them to do the work from scratch, but to validate and correct what the agent has already done. That's where the real time savings come from."*

---

## 4. WHEN THEY RAISE DATA PRIVACY / REGULATORY COMPLIANCE

Einstein had identical concerns — healthcare data in Brazil with cross-border restrictions. Mango will have HIPAA, Indian data protection laws, and potentially GDPR considerations.

**How Tom handles it (from Einstein call):**
> "All the data stays in your estate, none of your data ever leaves your estate. We build the solution ourselves in-house and then we work with your development team to get it integrated into your cloud environment. Ultimately what we build is your intellectual property, your source code."

> "We're very tech agnostic. So your tech team might tell us, for certain data, some of it has to be processed in Brazil and can't leave — OK, we work within that constraint."

**For Mango today — when privacy comes up (and it will with cancer patient data):**

*"With patient data — especially oncology data at this scale — we'd never propose an architecture where data leaves your environment. Everything we build runs in your cloud, your VPC. The models we use for extraction and harmonisation would either be deployed within your infrastructure, or we'd use API calls where only the minimum necessary context is sent — and we'd work with your compliance team to define exactly what that boundary looks like."*

*"We've worked with healthcare providers where the data sovereignty requirements meant certain data couldn't cross borders at all. Our approach was: the AI runs where the data lives, not the other way around. If your Indian patient data needs to stay in India, we build the pipeline to run there."*

If they push on specifics: *"Honestly, the compliance architecture is something we'd work through with your team in the technical deep-dive after the alignment doc. We need to understand your specific regulatory constraints — HIPAA applicability given you're US-headquartered, Indian data protection requirements for the source data, and any clinical trial regulations for the CRO work. We've navigated all of those individually; the question is how they intersect for your specific setup."*

---

## 5. WHEN THEY ASK ABOUT LLM SELECTION / "WHICH MODELS?"

**How Tom handles it (from Einstein call — they were evaluating multiple platforms):**
Einstein was running the same use case across multiple platforms to evaluate. Tom didn't push a specific model — he positioned Conquer as the team that helps you evaluate and choose.

**For Mango today:**

*"Model selection is something we'd benchmark early in delivery, not assume upfront. For data harmonisation specifically, we'd likely test a few approaches: a general-purpose model like Claude or GPT-4 for the complex reasoning tasks, potentially a fine-tuned smaller model for high-volume extraction where you need consistency and lower cost, and your existing NLP pipeline for the deterministic parts that are already working."*

*"One thing we've learned — especially in healthcare — is that for extraction tasks, a fine-tuned small model often outperforms a general-purpose large model. We had a case where we fine-tuned a model specifically for document extraction and it ended up being more accurate than the humans doing it manually. The key was: we could trace every extracted value back to a citation in the source document, so the reviewer could validate in seconds rather than re-reading the whole thing."*

This references the private credit case study Tom showed in the first Mango call — Hanif and Gavin will remember it.

---

## 6. WHEN THEY TALK ABOUT PATIENT SIMILARITY / GAVIN'S TRADITIONAL MODELS

Gavin has done significant work on patient similarity using traditional ML. He'll be protective of this work and curious about how agentic AI enhances it.

**For Mango today:**

*"The patient similarity work is really interesting to us because it's a perfect example of where traditional models and agentic AI complement each other rather than replace each other. Your traditional similarity models give you the mathematical rigour for cohort building. What agents can add is the ability to pull in unstructured context — clinical notes, imaging reports, genomic data — that your current models can't easily consume. So the agent enriches the input to your similarity engine rather than replacing it."*

*"The question for us would be: what data is your similarity model currently blind to that a clinician would consider relevant? That gap is usually where agents can add the most value."*

---

## 7. WHEN THEY ASK ABOUT CLINICAL TRIALS (CRO PARTNERSHIP)

You can't name Conquer's existing clinical trial client, but you can signal deep domain knowledge.

**From Suvoda — what you know about clinical trial AI:**
- Test script generation with 99.9% accuracy on known patterns
- Accuracy drops dramatically on novel protocols (41.4%)
- Human-in-the-loop validation is non-negotiable for clinical
- Hallucination is the critical risk — plausible but ungrounded outputs
- Fine-tuning helps but doesn't solve truly novel cases

**For Mango today:**

*"We've got several clients in the clinical trial space — the NDAs mean we can't say their name, let alone what we built. But I can tell you the domain is one we understand deeply. The core challenge is always the same: you need AI that's accurate enough to trust with patient-affecting decisions, but honest enough to flag when it doesn't know. In clinical, a confident wrong answer is worse than no answer at all."*

*"For your first two trials coming up — the question I'd ask is: which parts of the trial workflow are administrative burden versus clinical judgement? Site selection, patient matching, regulatory document processing — those are high-volume, pattern-heavy tasks where agents can accelerate. Protocol design and safety monitoring — those need human judgement with AI support, not AI autonomy."*

---

## 8. WHEN THEY ASK ABOUT MULTIMODAL (GENOMICS, RADIOLOGY/PATHOLOGY)

Mango said they're moving toward multimodal data. This is bleeding-edge territory.

**For Mango today:**

*"Multimodal is where things get really interesting and really hard at the same time. The harmonisation challenge you have now with structured and text data multiplies when you add imaging and genomics. Each modality has its own noise characteristics, its own standards — or lack of standards — and its own regulatory requirements."*

*"Our view would be: don't try to build a unified multimodal pipeline from day one. Get the text and structured data harmonisation rock-solid first — that's your foundation. Then add genomic data as a second modality, because it's structured even if complex. Imaging comes last because it requires the most specialised models and the validation is hardest."*

*"The flywheel Mohit described — building the capability to rapidly create applications — this is exactly how you get there. You build the platform for text harmonisation, prove the pattern works, then the same platform architecture extends to new modalities. Each one is a new application on the same foundation."*

---

## 9. WHEN THEY ASK "HOW LONG / HOW MUCH?"

**How Tom handles this (from AB and Inchcape):**

He gives a reference point from a past engagement, not a quote. Keeps it loose but concrete.

> "Just to give you an idea — this took us 10 weeks to develop, 3 weeks to integrate into their existing systems. Cost was in the low six figures, pounds sterling."

**For Mango today:**

Don't price anything. This is a discovery call. If they push:

*"Honestly, it's too early to put numbers on it — we need to understand the roadmap in detail first. What I can tell you is our typical first engagement is in the range of 8-12 weeks for a focused proof of value. But the right answer comes out of the alignment document, where we scope it together based on what actually matters most to you."*

*"We do all of discovery at our risk, for free. And when we move to delivery, we don't invoice until the thing is built and it's doing what you expect it to do. So all the risk is on our side — we need to earn our stripes."*

---

## 10. THE "BUILD VS BUY" QUESTION

Mango has a non-technical annotation team that needs tooling. They might ask whether to buy annotation tools or build custom ones.

**How Tom thinks about it (from News UK architecture):**
> "There's a natural split: buy the orchestration layer if something good exists, build the AI components that are specific to your domain. Don't reinvent infrastructure, but don't buy generic AI tools when your problem is domain-specific."

**For Mango today:**

*"For annotation tooling specifically — there are off-the-shelf annotation platforms, but the question is whether they understand oncology data well enough. If your annotation team is correcting AI-harmonised records against cancer-specific ontologies, you probably need something purpose-built. Not from scratch — but a custom interface on top of standard components that speaks your domain language."*

*"That's actually one of the patterns we see a lot: the infrastructure can be standard, but the user-facing layer needs to be domain-specific. Because if it's 3 extra clicks versus what your annotators are used to, they just won't use it — and then the adoption isn't there, and the value isn't there, and it's indistinguishable from a technical failure."*

---

## QUICK REFERENCE: TECHNICAL PHRASES TO USE

| Situation | What to say |
|-----------|-------------|
| They describe messy data | "Is the messiness at the entity level, the relationship level, or the cross-source harmonisation level?" |
| They mention ontology | "What's the ontology coverage like? Are you mapping to SNOMED, ICD, or a custom cancer-specific ontology?" |
| They ask about models | "We'd benchmark early — different tasks need different models. Extraction might want a fine-tuned small model, reasoning wants a frontier model." |
| They mention failed agentic attempts | "What specifically failed — was it accuracy, hallucination, or the agent doing plausible-looking but wrong things?" |
| They ask about cost | "Too early to scope — but our first engagements are typically 8-12 weeks, and we do discovery at our risk." |
| They worry about data privacy | "Everything runs in your environment. The AI goes to the data, not the other way around." |
| They mention multimodal | "Text and structured first, genomics second, imaging last. Each modality is a new app on the same platform." |
| They ask about annotation tooling | "Standard infrastructure, domain-specific interface. Your annotators need to see confidence scores and source citations." |
| They mention clinical trials | "We've got deep experience there under NDA. The key split is admin burden vs clinical judgement — agents accelerate the first, support the second." |
| They ask about accuracy | "Accuracy degrades with novelty. Start with known patterns, prove it there, then expand. We've seen 99% on known drop to 40% on novel." |
