# paperclip-pixeeplay

Deployment of [Paperclip](https://paperclip.ing)—an open-source AI agent orchestrator (MIT license)—onto a Pixeeplay OVH VPS via Coolify, featuring the **"Pixeeplay Studio" agent company**: an agency run by AI agents that takes a business/client brief and delivers, versions, and redeploys a complete solution—including websites, back-offices, and apps for iOS, macOS, watchOS, tvOS, and Android—all while adapting to the client’s specific concept, visual identity, business domain, and communication style.

```
deploy/      docker-compose.yml (for Coolify), .env.example, COOLIFY.md (step-by-step guide), install-vps.sh (SSH fallback)
company/     Importable Paperclip package (agentcompanies/v1 spec): COMPANY.md, agents/, teams/, projects/, tasks/, skills/, .paperclip.yaml
templates/   Brief template and an example of a standardized brief (Maison Dupain)
```

## Installing Paperclip

Follow the instructions in `deploy/COOLIFY.md` (approx. 10 minutes): create a "Docker Compose Empty" resource in Coolify, set the domain to `paperclip.pixeeplay.com`, configure the `CLAUDE_CODE_OAUTH_TOKEN` (requires a Claude Max subscription) and `GH_TOKEN` variables, initialize the first admin account via `pnpm paperclipai auth bootstrap-ceo`, and then disable further registrations. ## Importing the Company

In Paperclip → **Company → Import** → GitHub:

```
https://github.com/pixeeplay/paperclip-pixeeplay/tree/main/company
```

(The repository must be public for URL-based import; otherwise, from the service's Coolify terminal: run `git clone` (using `GH_TOKEN`) into `/paperclip/import`, then execute `pnpm paperclipai company import /paperclip/import/company --target new --new-company-name "Pixeeplay Studio" --yes`.)

After importing: configure the company secrets (`GH_TOKEN`, `COOLIFY_URL`, `COOLIFY_API_TOKEN`, `COOLIFY_SERVER_UUID`, `COOLIFY_PROJECT_UUID`), test the adapter for each agent, enable the **CEO**'s heartbeat (other agents wake up upon assignment), and submit an initial brief to the **Client Onboarding** project.

## The Organization

| Agent | Role | Model | Skills |
|---|---|---|---|
| **Studio Director (CEO)** | Receives briefs, opens projects, delegates, reports to the board | Opus 5.5 | Client business brief, functional roadmap/versioning |
| **Chief Product Officer (CPO)** | Functional roadmap, versioning schedule, promise-vs-delivery compliance | Opus 5.5 | Brief, functional roadmap, QA/acceptance testing |
| **Art Director** | Brand guidelines → W3C tokens → CSS/Swift/Kotlin, mockups | Sonnet 5 | Client brand guidelines |
| **Communications Manager** | Tone of voice, content, emails, app store listings, local SEO, French legal compliance | Sonnet 5 | Business communications |
| **CTO** | Architecture, repositories, task breakdown, code review, merging | Opus 5.5 | github-pr-workflow, site-factory-web, deploy-coolify-github |
| **Lead Web** | Next.js + API + back-office, Odoo/PrestaShop/Pixee PIM integrations | Sonnet 5 | site-factory-web |
| **Lead Apple** | Multi-target SwiftUI, macOS CI, TestFlight | Sonnet 5 | apple-multiplateforme |
| **Lead Android** | Kotlin/Compose, CI, internal Play Store track | Sonnet 5 | android-compose |
| **DevOps & Release** | Coolify, domains, versions/tags, changelog, monitoring | Sonnet 5 | deploy-coolify-github, plan-de-fonctions-versioning |
| **QA** | User acceptance testing (UAT) with evidence, verdict by criterion | Sonnet 5 | qa-recette |

Two units that can be imported separately: `teams/produit-design` (CPO + Art Director + Communications) and `teams/ingenierie` (CTO + leads + DevOps + QA).

## "New Client" Workflow

Brief submitted → standardized `BRIEF.md` → `PLAN-FONCTIONS.md` + `VERSIONS.md` → `design/tokens.json` + `DESIGN.md` → `COMMUNICATION.md` + `content/` → `pixeeplay/client-<slug>` repository scaffolded → v0.1 site deployed at `https://<slug>.pixeeplay.app` → apps (if requested) → UAT → board approval → production.

Anything that incurs costs, involves public publishing, or affects the production environment of an existing client requires board approval (Arnaud).

## Evolving Skills

Skills are defined as `SKILL.md` files, compatible with Agent Skills / skills.sh. A new business function or a new reusable client = a "skill" (or a reference in `brief-metier-client/references/familles-metier.md`) added via a PR; Paperclip re-imports the package in `existing` mode (using a `replace` collision strategy) to perform an update.

## License

MIT for this repository. Paperclip is © Paperclip Labs, Inc., MIT.
