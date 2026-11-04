# Athena's agents

The ready-made agents in the Agent library: one Markdown file each, in one folder per group (`groups.json` lists the
groups in the order the Library shows them). Hiring one copies its text into a new agent, which the user can then
change; this folder is never changed by the app.

These replace The Agency (`library/agency-agents/`). The list of roles started from
[msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) at commit
`b37b5bb33b78f2c1d6567d98e25ca818e3259219` (MIT, see `LICENSE-agency-agents`); the text here is new.
[Where the old agents went](#where-the-old-agents-went) records which old files each agent took over.

Every file follows the standard below. `tests/server/library.test.ts` checks the format; the writing itself is
checked by a reviewer and by trying the agent on real tasks.

## Frontmatter

One `key: value` per line (the dashboard's reader doesn't take YAML lists), so lists are comma-separated.

```markdown
---
name: Code Reviewer
description: Reviews a change, AI-written or not, for bugs, security holes and code that will be hard to keep, says what to fix first and can fix it.
emoji: 👁️
color: "#6b7bff"
specialty: reviews code changes and says what to fix first
group: engineering
tools: files, shell, browser, agents
skills: github, witness-discipline, independent-review
---
```

| Key | What it's for |
|---|---|
| `name`, `emoji` | The card and the hired agent. Familiar job titles |
| `description` | One sentence of about 30 words: what it does and when you'd pick it. The card shows it, and the Brain reads every one when it picks teammates |
| `color` | Hex in quotes, mid-tone, readable on white and on near-black |
| `specialty` | A lower-case phrase with no full stop. It finishes "You are Code Reviewer, …", the line the dashboard puts first in every agent's instructions |
| `group` | An id from `groups.json`, the same as the folder the file is in |
| `tools` | The tool groups it starts with (`TOOL_GROUPS` in `server/tools.ts`). Every agent has `shell` and `agents`, so any agent can run a command (the user approves each one unless autonomy is Full) and ask a teammate. A starting set, not a limit: the user or the Brain can add more, and the agent uses whatever it has |
| `skills` | Optional. Skill slugs it starts with, from the bundled skills (`skills/`) or the dashboard's own |
| `engine` | Optional. Only when the agent needs one engine (the Mail Assistant uses `claude-code` for the Gmail connector) |
| `starter` | `true` on the 8 agents the Home page's "add the starter team" hires |

## Body

60 to 150 lines, wrapped at 120 characters (tables and code blocks aside); most land between 100 and 145. Plain
headings in sentence case, no emoji. The headings are a guide, not a form: a section with nothing specific to say is
left out, and no section repeats another.

- **Opening paragraph (no heading).** What the agent is for, who it works for, and what good work looks like in this
  field. No "You are …": the dashboard says that.
- **How you think.** What an experienced practitioner weighs that a beginner misses: priorities, trade-offs and rules
  of thumb that only fit this role. Most of the value is here.
- **How you work.** What to look at or ask first, the order of work, and how to check the result before handing it in.
- **What you hand back.** What a good answer contains, with a short template or worked example where one helps. A
  default shape, not a form: the agent changes it when the task needs something else.
- **Lines you hold.** What it never does, and when it stops and asks: before deleting, spending money, sending or
  posting, or making a claim it can't back up. Legal, medical, tax and investment agents give information, not
  advice, and say when to see a professional. These lines protect the user; they are not limits on the role.
- **Watch out for.** The usual ways this work goes wrong that the sections above don't already cover. If "How you
  think" says it, don't say it again here.
- **Beyond your role.** The nearby work it takes on itself (a Code Reviewer fixes what it finds and adds the test; a
  LinkedIn Writer makes the X version and the image to go with it). When a different kind of specialist ("a
  security reviewer") would clearly do better, or a second opinion is worth having, it says so or asks a teammate,
  but it doesn't refuse reasonable work because it's outside its title. Write hand-offs as "also get a security
  review", not "security is not your job".

## Roles are a starting point

Models and their tools keep getting better, so the files are written to age well and not to hold a capable agent back.

- **The role is where the agent is strongest, not the edge of what it does.** It does the nearby work a good
  practitioner would also do, and does it well, instead of stopping at its title.
- **Hand off for quality, not for scope.** It brings in another specialist when that specialist would clearly do
  better, or when an independent check matters (security, a second review), not because the work is "not mine".
- **Use what's there.** If it has tools beyond its starting set (the browser, files, images, 3D programs, other
  agents), it uses them when they help.
- **Nothing about the model's own limits.** No "you can't see images", "you can't run code" or "text only": what an
  agent can do comes from its tools at run time, and that changes.
- **Judgment over steps.** Describe what good work looks like and the trade-offs in it, not routines a strong model
  follows anyway. Write steps down only where the order matters (check before publishing, back up before a migration).
- **Able to do more, not adding work nobody asked for.** It does what was asked first, at the size the task needs, and
  offers the nearby extras in a line. It produces them unasked only when the asked-for thing can't work without them.
  (In side-by-side tests, the rewritten agents' only losses came from building more than was asked.)
- **The user's word comes first.** If the user asks an agent to stay in its lane, or a team gives it one part of a
  job, it keeps to that.

## Rules for every file

1. **No invented numbers.** A number appears only when it is a real standard or threshold, it is correct, and it is
   current (WCAG's 4.5:1 contrast for body text, a platform's real character limit). Worked examples may use
   made-up details (a file name, a line number) but never made-up statistics or results; use `[placeholders]`.
2. **No claims of memory or experience.** Nothing like "you remember" or "you've shipped dozens of apps". The
   dashboard gives agents their memory itself.
3. **No tool names that might not exist.** The dashboard tells each agent its tools at run time, and the same text
   runs on Claude Code, Codex, Hermes, pi, OpenClaw and API models, and can be exported as a Claude Code subagent.
   Real programs from the field are fine (`git`, `ffmpeg`, Blender's `bpy`).
4. **Code only when words won't do**: short, correct, in today's idioms, and typed properly.
5. **Facts that change get checked.** Versions, APIs, platform rules and limits are checked against current docs
   when written. A platform's limits come from its own help pages or docs; several other sites agreeing isn't enough.
   Anything that can't be confirmed is left out.
6. **Written for this role.** If you swap the name for another agent's, the text should stop making sense. This is
   about the writing being specific, not about narrowing what the agent may do.
7. **Says what it assumed.** When a task is unclear, it asks one or two real questions, or states its assumption and
   goes on.
8. **Fresh text.** The old Agency files are a list of topics at most. Nothing is copied from them.
9. **Commands that work unattended.** A command the agent is told to run must work when its output is read by a
   program and nothing can be typed in: no pagers, prompts or colour-only output (`git diff --color=always
   --color-moved`, not `--color-moved` alone).

## Where the old agents went

The Agency had 283 agents. 136 of them were folded into these 88 (★ marks the 8 that were Athena's starters, written
in code before). The other 147 were left out and can be added back.

### Engineering

| Agent | Took over |
|---|---|
| Software Architect (`software-architect`) | `engineering-software-architect`, `specialized-master-plan-architect` |
| Backend Architect (`backend-architect`) | `engineering-backend-architect`, `engineering-api-platform-engineer` |
| Frontend Developer (`frontend-developer`) | `engineering-frontend-developer` |
| Senior Developer (`senior-developer`) | `engineering-senior-developer`, `engineering-rapid-prototyper` |
| Mobile App Builder (`mobile-app-builder`) | `engineering-mobile-app-builder`, `engineering-mobile-release-engineer` |
| Code Reviewer (`code-reviewer`) | `engineering-code-reviewer` |
| Minimal Change Engineer (`minimal-change-engineer`) | `engineering-minimal-change-engineer` |
| Codebase Guide (`codebase-guide`) | `engineering-codebase-onboarding-engineer`, `specialized-codebase-archaeologist` |
| DevOps Automator (`devops-automator`) | `engineering-devops-automator`, `engineering-git-workflow-master`, `engineering-platform-engineer` |
| Site Reliability Engineer (`site-reliability-engineer`) | `engineering-sre`, `engineering-incident-response-commander`, `support-infrastructure-maintainer` |
| Database Optimizer (`database-optimizer`) | `engineering-database-optimizer`, `engineering-database-reliability-engineer` |
| Data Engineer (`data-engineer`) | `engineering-data-engineer` |
| AI Engineer (`ai-engineer`) | `engineering-ai-engineer`, `engineering-rag-pipeline-engineer` |
| Prompt Engineer (`prompt-engineer`) | `engineering-prompt-engineer` |
| MCP Builder (`mcp-builder`) | `specialized-mcp-builder` |
| Technical Writer (`technical-writer`) | `engineering-technical-writer` |
| Reality Checker (`reality-checker`) | `testing-reality-checker`, `testing-evidence-collector` |
| API Tester (`api-tester`) | `testing-api-tester` |
| Test Automation Engineer (`test-automation-engineer`) | `testing-test-automation-engineer`, `testing-test-results-analyzer` |
| Accessibility Auditor (`accessibility-auditor`) | `testing-accessibility-auditor`, `engineering-section-508-specialist` |
| Performance Benchmarker (`performance-benchmarker`) | `testing-performance-benchmarker` |
| Application Security Engineer (`application-security-engineer`) | `security-appsec-engineer`, `security-senior-secops` |
| AI Code Security Auditor (`ai-code-security-auditor`) | `security-ai-generated-code-auditor` |
| Security Architect (`security-architect`) | `security-architect`, `security-cloud-security-architect` |
| Penetration Tester (`penetration-tester`) | `security-penetration-tester` |
| Secrets Hygiene Engineer (`secrets-hygiene-engineer`) | `security-secrets-credential-engineer` |

### Design

| Agent | Took over |
|---|---|
| UI Designer (`ui-designer`) | `design-ui-designer` |
| UX Architect (`ux-architect`) | `design-ux-architect` |
| UX Researcher (`ux-researcher`) | `design-ux-researcher`, `design-persona-walkthrough` |
| Brand Guardian (`brand-guardian`) | `design-brand-guardian`, `design-visual-storyteller` |
| UI Finish Reviewer (`ui-finish-reviewer`) | `design-ui-finish-gate-reviewer` |

### Images, video and 3D

| Agent | Took over |
|---|---|
| ★ Image Maker (`image-maker`) | Athena starter, `design-image-prompt-engineer`, `design-inclusive-visuals-specialist` |
| ★ 3D Artist (`3d-artist`) | Athena starter |
| ★ Video Editor (`video-editor`) | Athena starter, `marketing-short-video-editing-coach` |

### Games and the voxel world

| Agent | Took over |
|---|---|
| ★ World Builder (`world-builder`) | Athena starter |
| Game Designer (`game-designer`) | `game-designer`, `economy-designer` |
| Narrative Designer (`narrative-designer`) | `narrative-designer` |
| Technical Artist (`technical-artist`) | `technical-artist` |
| Blender Add-on Engineer (`blender-add-on-engineer`) | `blender-addon-engineer` |
| Unity Developer (`unity-developer`) | `unity-architect`, `unity-editor-tool-developer`, `unity-multiplayer-engineer`, `unity-shader-graph-artist` |
| Unreal Developer (`unreal-developer`) | `unreal-systems-engineer`, `unreal-multiplayer-architect`, `unreal-technical-artist`, `unreal-world-builder` |
| Godot Developer (`godot-developer`) | `godot-gameplay-scripter`, `godot-multiplayer-engineer`, `godot-shader-developer` |
| Roblox Developer (`roblox-developer`) | `roblox-systems-scripter`, `roblox-experience-designer`, `roblox-avatar-creator` |

### Marketing and social

| Agent | Took over |
|---|---|
| ★ X Writer (`x-writer`) | Athena starter, `marketing-twitter-engager`, `marketing-x-twitter-intelligence-analyst` |
| Content Creator (`content-creator`) | `marketing-content-creator` |
| SEO Specialist (`seo-specialist`) | `marketing-seo-specialist` |
| AI Search Optimizer (`ai-search-optimizer`) | `marketing-ai-citation-strategist`, `marketing-aeo-foundations`, `marketing-agentic-search-optimizer` |
| Social Media Strategist (`social-media-strategist`) | `marketing-social-media-strategist`, `marketing-instagram-curator` |
| LinkedIn Writer (`linkedin-writer`) | `marketing-linkedin-content-creator` |
| TikTok Strategist (`tiktok-strategist`) | `marketing-tiktok-strategist`, `marketing-carousel-growth-engine` |
| Reddit Community Builder (`reddit-community-builder`) | `marketing-reddit-community-builder` |
| YouTube Strategist (`youtube-strategist`) | `marketing-video-optimization-specialist` |
| Email Marketer (`email-marketer`) | `marketing-email-strategist` |
| Growth Hacker (`growth-hacker`) | `marketing-growth-hacker` |
| PR Manager (`pr-manager`) | `marketing-pr-communications-manager` |
| Book Co-Author (`book-co-author`) | `marketing-book-co-author` |
| Search Ads Strategist (`search-ads-strategist`) | `paid-media-ppc-strategist`, `paid-media-search-query-analyst`, `paid-media-auditor`, `paid-media-tracking-specialist` |
| Paid Social Strategist (`paid-social-strategist`) | `paid-media-paid-social-strategist`, `paid-media-creative-strategist` |

### Sales and support

| Agent | Took over |
|---|---|
| Outbound Strategist (`outbound-strategist`) | `sales-outbound-strategist`, `sales-outreach` |
| Deal Strategist (`deal-strategist`) | `sales-deal-strategist`, `sales-discovery-coach` |
| Proposal Writer (`proposal-writer`) | `sales-proposal-strategist` |
| Offer Strategist (`offer-strategist`) | `sales-offer-lead-gen-strategist` |
| Support Responder (`support-responder`) | `support-support-responder`, `customer-service`, `customer-success-manager` |

### Product and projects

| Agent | Took over |
|---|---|
| Product Manager (`product-manager`) | `product-manager`, `product-sprint-prioritizer` |
| Feedback Synthesizer (`feedback-synthesizer`) | `product-feedback-synthesizer` |
| Trend Researcher (`trend-researcher`) | `product-trend-researcher` |
| Project Manager (`project-manager`) | `project-manager-senior`, `project-management-project-shepherd`, `project-management-studio-producer` |
| Meeting Notes Taker (`meeting-notes-taker`) | `project-management-meeting-notes-specialist` |

### Research and analysis

| Agent | Took over |
|---|---|
| ★ Researcher (`researcher`) | Athena starter, `research-synthesist` |
| Data Analyst (`data-analyst`) | `support-analytics-reporter`, `engineering-data-visualization-engineer` |
| Statistician (`statistician`) | `academic-statistician` |
| Executive Summary Writer (`executive-summary-writer`) | `support-executive-summary-generator` |
| Business Strategist (`business-strategist`) | `business-strategist` |
| Tool Evaluator (`tool-evaluator`) | `testing-tool-evaluator` |

### Business and finance

| Agent | Took over |
|---|---|
| ★ Coordinator (`coordinator`) | Athena starter, `agents-orchestrator` |
| ★ Mail Assistant (`mail-assistant`) | Athena starter |
| Chief of Staff (`chief-of-staff`) | `specialized-chief-of-staff` |
| Operations Manager (`operations-manager`) | `operations-manager`, `testing-workflow-optimizer` |
| Legal Document Reviewer (`legal-document-reviewer`) | `legal-document-review`, `support-legal-compliance-checker` |
| Grant Writer (`grant-writer`) | `grant-writer` |
| Resume Tailor (`resume-tailor`) | `resume-tailor` |
| Translator (`translator`) | `language-translator` |
| Document Maker (`document-maker`) | `specialized-document-generator` |
| Notes Steward (`notes-steward`) | `zk-steward` |
| Financial Analyst (`financial-analyst`) | `finance-financial-analyst`, `finance-fpa-analyst`, `support-finance-tracker` |
| Bookkeeper (`bookkeeper`) | `finance-bookkeeper-controller` |
| Investment Researcher (`investment-researcher`) | `finance-investment-researcher` |
| Pricing Analyst (`pricing-analyst`) | `specialized-pricing-analyst` |

### Left out (147)

- **academic**: `academic-anthropologist`, `academic-geographer`, `academic-historian`, `academic-narratologist`, `academic-psychologist`
- **design**: `design-whimsy-injector`
- **engineering**: `engineering-ai-data-remediation-engineer`, `engineering-ats-validator-architect`, `engineering-autonomous-optimization-architect`, `engineering-china-network-engineer`, `engineering-cms-developer`, `engineering-desktop-app-engineer`, `engineering-developer-tooling-engineer`, `engineering-drupal-performance`, `engineering-drupal-shopping-cart`, `engineering-electrical-engineer`, `engineering-email-intelligence-engineer`, `engineering-embedded-firmware-engineer`, `engineering-feishu-integration-developer`, `engineering-filament-optimization-specialist`, `engineering-finops-engineer`, `engineering-gaussdb-expert`, `engineering-i18n-engineer`, `engineering-identity-access-engineer`, `engineering-iot-fleet-engineer`, `engineering-it-service-manager`, `engineering-knowledge-graph-engineer`, `engineering-llm-post-training-engineer`, `engineering-multi-agent-systems-architect`, `engineering-network-engineer`, `engineering-orgscript-engineer`, `engineering-payments-billing-engineer`, `engineering-pdf-engine-architect`, `engineering-privacy-engineer`, `engineering-realtime-collaboration-engineer`, `engineering-rust-refactoring-specialist`, `engineering-search-relevance-engineer`, `engineering-servicenow-developer-mentor`, `engineering-solidity-smart-contract-engineer`, `engineering-universal-document-compiler`, `engineering-uswds-developer`, `engineering-video-streaming-engineer`, `engineering-voice-ai-integration-engineer`, `engineering-webassembly-engineer`, `engineering-wechat-mini-program-developer`, `engineering-wordpress-performance`, `engineering-wordpress-shopping-cart`
- **finance**: `finance-tax-strategist`
- **game-development**: `game-audio-engineer`, `level-designer`
- **gis**: `gis-3d-scene-developer`, `gis-analyst`, `gis-bim-specialist`, `gis-cartography-designer`, `gis-drone-reality-mapping`, `gis-geoai-ml-engineer`, `gis-geoprocessing-specialist`, `gis-qa-engineer`, `gis-solution-engineer`, `gis-spatial-data-engineer`, `gis-spatial-data-scientist`, `gis-technical-consultant`, `gis-web-gis-developer`
- **healthcare**: `healthcare-clinical-evidence-agent`, `healthcare-innovation-strategist`, `healthcare-sovereign-health-systems-agent`
- **marketing**: `marketing-app-store-optimizer`, `marketing-baidu-seo-specialist`, `marketing-bilibili-content-strategist`, `marketing-china-ecommerce-operator`, `marketing-china-market-localization-strategist`, `marketing-cross-border-ecommerce`, `marketing-developer-community-builder`, `marketing-douyin-strategist`, `marketing-global-podcast-strategist`, `marketing-kuaishou-strategist`, `marketing-livestream-commerce-coach`, `marketing-multi-platform-publisher`, `marketing-podcast-strategist`, `marketing-private-domain-operator`, `marketing-wechat-official-account`, `marketing-weibo-strategist`, `marketing-xiaohongshu-specialist`, `marketing-zhihu-strategist`
- **paid-media**: `paid-media-programmatic-buyer`
- **product**: `product-behavioral-nudge-engine`, `product-dx-engineer`
- **project-management**: `project-management-experiment-tracker`, `project-management-jira-workflow-steward`, `project-management-studio-operations`
- **sales**: `sales-account-strategist`, `sales-coach`, `sales-engineer`, `sales-pipeline-analyst`
- **security**: `security-blockchain-security-auditor`, `security-compliance-auditor`, `security-incident-responder`, `security-threat-detection-engineer`, `security-threat-intelligence-analyst`
- **spatial-computing**: `macos-spatial-metal-engineer`, `terminal-integration-specialist`, `visionos-spatial-engineer`, `xr-cockpit-interaction-specialist`, `xr-immersive-developer`, `xr-interface-architect`
- **specialized**: `accounts-payable-agent`, `agentic-identity-trust`, `automation-governance-architect`, `change-management-consultant`, `chief-financial-officer`, `corporate-training-designer`, `data-consolidation-agent`, `data-privacy-officer`, `esg-sustainability-officer`, `government-digital-presales-consultant`, `healthcare-aging-parent-care-companion`, `healthcare-customer-service`, `healthcare-marketing-compliance`, `hospitality-guest-services`, `hr-onboarding`, `identity-graph-operator`, `legal-billing-time-tracking`, `legal-client-intake`, `loan-officer-assistant`, `lsp-index-engineer`, `ma-integration-manager`, `medical-billing-coding-specialist`, `organizational-psychologist`, `personal-growth-mentor`, `real-estate-buyer-seller`, `recruitment-specialist`, `report-distribution-agent`, `retail-customer-returns`, `sales-data-extraction-agent`, `specialized-civil-engineer`, `specialized-cultural-intelligence-strategist`, `specialized-developer-advocate`, `specialized-fedramp-rmf-compliance`, `specialized-focus-music-architect`, `specialized-french-consulting-market`, `specialized-korean-business-navigator`, `specialized-model-qa`, `specialized-salesforce-architect`, `specialized-strategy-duel-agent`, `specialized-workflow-architect`, `study-abroad-advisor`, `supply-chain-strategist`
