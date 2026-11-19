---
icon: fas fa-info-circle
order: 1
---

I'm a software engineer in Cardiff with nearly thirty years of building for the web. Since 2012 I've built and run the whole technical platform at [Photosymbols](https://www.photosymbols.com), full-time since 2022, and since 2024 I've turned it into an AI product used by public-sector organisations across the UK.

## Now: Photosymbols

Photosymbols makes information easier to understand for people with learning disabilities, through Easy Read and a library of real photos. I look after everything technical:

- **The product.** The photo library, EasyMaker (documents into Easy Read packs), a block editor, the Easy Read Standard checker, surveys in Easy Read and a community area for customers.
- **The AI.** Language-model rewriting into plain English and Easy Read, through one gateway over Anthropic, OpenAI and Google models that records the cost of every call. Hybrid semantic photo search with pgvector. AI image generation and editing. Personal data is redacted before text leaves for a model and restored afterwards.
- **Compliance.** PDF/UA documents, WCAG 2.2 AA with automated accessibility tests, and AI provenance marking for the EU AI Act.
- **The shop.** A Shopify integration I've run since 2015: subscriptions, multi-seat licences, trials and renewals.
- **Running it.** Rails 8 upgrades on both apps, deployment, monitoring and incidents. I cut the test suite from about 210 seconds to 60 by profiling it, and halved the monthly language-model bill by tracing it to one feature.

## How I work

Tests first. A short written design before anything large, then a plan, then the code. These days much of the code is written with AI coding agents working in parallel, so most of my effort goes into the parts that make that safe: clear specs, review gates, and checking that a change really works before it ships.

## Before

I started in 1997 writing Perl and C++ for Companies House, including WebCHeck and the award-winning Companies House Direct. In the 2000s I moved to Java as a project lead and team lead, on the European Business Register, Cardiff University's website and Companies House's £40m CHIPS project. In 2007 I started my own software company, and in 2010 I went freelance with Ruby on Rails.

## Tools I use

- **Ruby on Rails** (8.x), Hotwire, Minitest, Solid Queue
- **PostgreSQL** with pgvector, and SQLite
- **AI:** Claude, OpenAI and Gemini models, RubyLLM, MCP servers, retrieval and embeddings, model comparison and LLM-as-judge harnesses
- **Accessibility:** WCAG 2.2, PDF/UA, Easy Read
- **Ops:** Linux, Capistrano, Puma, Sentry

Want to talk? Email [martin@martinwood.org](mailto:martin@martinwood.org).
