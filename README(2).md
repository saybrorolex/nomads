# Waypoint

**Plan your journey around what matters to you.**

Waypoint is an AI-powered travel planning and decision-support project. It aims to bring transport discovery, accommodation matching, budget calculations, and trip management into one conversational experience.

Instead of showing only the cheapest option, Waypoint will help travellers understand which option fits their budget, time, comfort, and personal requirements.

> **Project status:** Planning and architecture. This README describes the intended system; the features below are not claims of completed functionality. Travel providers are being evaluated, and setup instructions will be completed once the application scaffold is available.

## Why Waypoint?

Planning a trip often means switching between booking sites, maps, reviews, and spreadsheets. A low ticket price can hide a long transfer, an inconvenient arrival time, or additional local travel costs. A hotel within budget may still miss an important accessibility or food requirement.

Waypoint aims to connect these decisions and explain their trade-offs using the traveller's actual priorities.

## Planned core features

| Capability | Intended behaviour |
| --- | --- |
| Guided trip requirements | Collect origin, destination, dates, travellers, budget, transport choices, and accommodation needs through a form and conversation. |
| Planning agent | Ask for missing details, use search tools, and update the plan when requirements change. |
| Transport discovery | Search flights and supported train or bus options. Mixed-mode journeys are a core objective, with initial coverage limited by available providers. |
| Accommodation matching | Compare properties by price, location, amenities, food preferences, and available accessibility information. |
| Unified comparison | Compare estimated total cost, journey time, stops, transfers, and fit with the user's requirements. |
| Budget optimisation | Calculate transport, accommodation, and other available expense totals using deterministic Python code. |
| Local transport assistance | Use supported location and routing data to help plan movement within a destination. |
| Saved trips and memory | Save structured requirements, selected options, and relevant conversation context for later edits. |
| Source transparency | Label retrieved facts, user inputs, estimates, and unverified information, with sources and retrieval times where available. |

### Constraints and personalisation

The system will distinguish **hard constraints**, such as a fixed budget or arrival deadline, from **soft preferences**, such as breakfast, fewer transfers, or a quiet location. A result with an unverified essential requirement should not be presented as a confirmed match.

Personalised ranking is part of the core system. Review analysis, when permitted data is available, will add supporting context.

## Planned enhancements

**Complementary features:** destination weather, destination information, itinerary and activity planning, currency conversion, multilingual interaction, review-informed recommendations, source-linked travel requirements checks, saved searches, and price history.

**Advanced extensions:** scheduled price monitoring, explicitly enabled email alerts, later WhatsApp alerts, credit card rewards estimation from a small verified catalogue, policy-change monitoring, and optional OCR-assisted document analysis.

These extensions will follow the working core. Their inclusion in the eight-week build depends on integration access and development progress.

## Example experience

A traveller could ask:

> “Help me plan a trip from Bengaluru to Hyderabad. My total budget is ₹15,000. I can take a flight, train, or bus, but I need to arrive before noon. Find a hotel near public transport with breakfast.”

Waypoint would ask for missing dates and traveller details, retrieve supported options, filter against the arrival deadline and budget, compare suitable journeys and hotels, and calculate an estimated total. The traveller could then change a preference and request an updated comparison.

This is an illustrative workflow, not a live quote or implemented demonstration.

## Intended technology stack

| Layer | Planned choice |
| --- | --- |
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend | Python and FastAPI |
| Agent orchestration | LangGraph |
| Local AI inference | Ollama with a compatible model selected through testing |
| Optional hosted AI | Gemini API, subject to free-tier availability and limits |
| Persistent storage | PostgreSQL; local development or Supabase hosting |
| Version control | GitHub with feature branches and pull requests |
| Deployment candidates | Vercel for the frontend and Render for the backend |
| Agent tracing | LangSmith, with application logs for API and server errors |

The model adapter will allow local and hosted inference without changing the core trip workflow. Ollama requires a running machine to serve its model; deploying the web application does not automatically host local inference.

The project prioritises free tools and free tiers. Service access, usage limits, data terms, and deployment suitability must be verified before adoption.

### API candidates under evaluation

| Integration | Candidate | Decision still needed |
| --- | --- | --- |
| Flights and hotels | Amadeus | Validate access, test-data limitations, and relevant route/property coverage. |
| Places and routing | Geoapify | Validate supported routing modes and destination coverage. |
| Weather | Open-Meteo | Validate forecast horizon and permitted usage. |
| Currency | Frankfurter | Validate supported currencies and rate freshness. |
| Trains and buses | Not selected | Identify permitted, accessible providers with useful India coverage. |
| Reviews and travel requirements | Not selected | Identify permitted review sources and authoritative requirements sources. |

Provider names are candidates, not confirmed integrations. Limited or demonstration data must be labelled accordingly.

## Planned architecture

The Next.js interface will send structured trip inputs and chat messages to FastAPI. LangGraph will manage clarification, tool selection, and trip state. Provider adapters will normalise external search results into shared transport and accommodation formats.

Application code will enforce constraints, calculate costs, and rank candidates. The language model will help interpret requests and explain the resulting trade-offs. PostgreSQL will store saved trip data and relevant context.

Tool failures will produce clear messages. Missing prices or unsupported routes must remain visible instead of being replaced with invented results.

## Proposed repository layout

```text
waypoint/
├── frontend/          # Next.js interface
├── backend/
│   ├── app/
│   │   ├── api/       # FastAPI routes
│   │   ├── agent/     # LangGraph workflow and model adapters
│   │   ├── providers/ # External API adapters
│   │   ├── services/  # Comparison, constraints, and budget logic
│   │   └── db/        # Database models and persistence
│   └── tests/
├── docs/              # Architecture, API research, and project documentation
└── README.md
```

This is a proposed structure; folders should be added as implementation begins.

## Development setup status

There is no runnable scaffold documented yet. Exact installation commands, dependency versions, environment variable names, database migrations, and startup commands will be added after they have been tested.

For now, prepare Git, VS Code, Node.js with npm, Python with virtual environment support, and PostgreSQL access. Install Ollama if using local inference. Each collaborator should use their own local environment and keep credentials outside version control.

The first verified setup guide should cover:

1. Cloning the repository and installing frontend/backend dependencies.
2. Creating local configuration from a committed example file containing no secrets.
3. Configuring the selected model, database, and approved API credentials.
4. Applying database migrations and starting the frontend and backend.
5. Running checks and a documented sample trip request.

## Development roadmap

| Period | Target |
| --- | --- |
| Weeks 1–2 | Repository, environment, early deployed prototype, basic function-calling agent, logs, and CI basics. Include the brief's inherited document-upload/RAG foundation milestone. |
| Weeks 3–4 | LangGraph state and tool workflow, initial integrations, error handling, and meaningful tests. |
| Weeks 5–6 | Travel discovery, PostgreSQL persistence, comparison, budget calculation, and trip revision. |
| Weeks 7–8 | UI polish, deployment, monitoring, security review, documentation, and final demonstration. |

Waypoint follows the advanced Track B direction: LangGraph orchestration, a professional web interface, PostgreSQL, and multiple integrations. The target is at least four external integrations overall, including at least three domain-specific APIs. Coverage and feature commitments will be refined after provider testing.

## Collaboration

The project is being developed by a three-person student team. API evaluation is currently being handled by teammates while architecture and documentation are developed alongside it.

Use GitHub Issues for scoped tasks, feature branches for changes, and pull requests for review. Record provider findings and architecture decisions in `docs/`. Individual contributions will be documented for the final presentation.

## Trust and user control

- Keep API keys on the backend and out of Git history.
- Minimise stored personal data and provide user-controlled trip management.
- Treat external text and reviews as data, not agent instructions.
- Recheck time-sensitive prices and requirements before decisions.
- Do not guarantee a room assignment or accessibility feature that cannot be confirmed.
- Never collect card numbers, CVVs, or banking passwords for rewards estimation.
- Require explicit opt-in for monitoring and notifications.
- Keep purchases, reservations, and booking changes under the user's control.

## Validation and final deliverables

Planned checks cover requirement collection, constraint enforcement, budget arithmetic, provider failures, saved-trip isolation, conversation updates, and agent tool use. Monitoring and benchmarks should report measured results rather than assumed performance.

Final deliverables will include source code, tested setup instructions, architecture and API documentation, a working deployment, an 8–10 minute Track B demo, individual contributions, actual challenges and learnings, monitoring/benchmark evidence, a security assessment, and a technical blog post.

The live demo URL and final video link will be added when available. A repository licence has not yet been selected.
