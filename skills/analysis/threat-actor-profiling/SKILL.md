---
name: threat-actor-profiling
description: Build structured threat actor profiles using the 5W1H framework and the Diamond Model. Use this skill whenever the user wants to profile a threat actor, create a TA report, analyze an APT group, build an adversary profile, assess threat actor capability, map TTPs to MITRE ATT&CK for a specific group, or produce any intelligence deliverable about a threat actor. Also trigger when the user mentions threat actor names (e.g. APT29, Lazarus, FIN7), asks about victimology, modus operandi, or wants to structure threat intelligence around an adversary. This skill applies to both internal tracking profiles and incident-driven analytical deliverables.
---

# Threat Actor Profiling Skill

Generate structured, actionable threat actor profiles following a deliverable-first methodology grounded in the 5W1H framework and the Diamond Model of Intrusion Analysis.

## When to Use

- User asks to profile or research a threat actor / APT group
- User needs to produce an intelligence deliverable about an adversary
- User wants to structure existing threat data into a TA profile
- User is responding to an RFI or incident involving a known threat actor
- User wants to assess capability, intent, or victimology of a group

## Core Workflow

Follow these six steps in order. Each step builds toward a complete profile.

### Step 1: Define Scope and Purpose

Before collecting any data, clarify:

1. **Profile type**: Is this for continuous internal tracking or an incident-driven analytical deliverable?
2. **Audience**: Who will consume this? (CTI team, SOC, IR, executives, legal, comms)
3. **Trigger**: Is this RFI-driven, incident-driven, or proactive tracking?

Use this decision matrix:

| Purpose | Audience | Focus | Risk to Avoid |
|---|---|---|---|
| Internal tracking | CTI, SOC, IR | Structured IOCs and TTPs, minimal narrative | Building a library nobody uses |
| Analytical deliverable | Management, Legal, Comms, IR | Timelines, impact analysis, recommendations | Unfocused data collection |

### Step 2: Collect Data Sources

Gather from two categories:

**Internal telemetry** (if incident-related):
- SIEM logs
- EDR/XDR alerts
- IAM / Active Directory events
- NDR and NetFlow data
- Cloud control plane and SaaS logs
- Honeypot data
- Vulnerability management systems

**External intelligence**:
- TIPs (OpenCTI, ThreatConnect, CrowdStrike Falcon, etc.)
- Content aggregators (Recorded Future, Feedly, Inoreader)
- OSINT tools (VirusTotal, urlscan.io, any.run, Shodan, Censys, abuse.ch)
- Sharing communities (ISACs, FIRST, bilateral partners)

**Evaluate source quality** using the Admiralty Code:
- Source Reliability: A (completely reliable) through F (cannot be judged)
- Information Credibility: 1 (confirmed) through 6 (cannot be judged)

Assign a letter-number pair to each source (e.g., B2 = usually reliable, probably true).

### Step 3: Apply 5W1H and the Diamond Model

Structure the profile using these sections:

#### 3a. Identity and Attribution (WHO)
- **Group name and aliases**: Primary name, community aliases, sub-groups, known rebrands. Use a single internal identifier.
- **Type of actor**: Cybercrime, state-sponsored, hacktivist, ransomware affiliate, etc.

#### 3b. Motive and Objective (WHY)
- **Motive**: Financial gain, political/ideological, thrill-seeking
- **Objective**: Data theft, disruption, unauthorized access, espionage, extortion

#### 3c. Victimology (WHO is targeted)
- **Sector and industry mapping**: Use NACE or NAICS codes if formal classification is needed
- **Geographic targeting**: Regions, nations, geopolitical alignment
- **Technology stack profiling**: Targeted software, unpatched systems, specific platforms
- **Intent toward your organization**: Direct, Competitors, Industry, Opportunistic

#### 3d. Capability Assessment (HOW capable)

Use a three-level scale:

| Level | Description | Tooling | Resources | Examples |
|---|---|---|---|---|
| High | Nation-state backed, highly skilled | Custom tooling, zero-days, long dwell time | Extensive/unlimited funding | APT28, Lazarus |
| Moderate | Skilled, no direct state sponsorship | Mix of open-source and limited custom tools | Substantial but unclear funding | FIN7, Wizard Spider |
| Low | Basic attacks, script-level | Commodity/public tools only | Minimal | Hacktivists, script kiddies |

Optionally use five levels (High, Medium-High, Moderate, Medium-Low, Low) for more granular assessment.

#### 3e. Modus Operandi (HOW they operate)
- **Known campaigns**: High-level description of major operations
- **Key TTPs mapped to MITRE ATT&CK**:
  - Initial access vectors
  - Lateral movement methods
  - Post-exploitation behaviors
  - Data exfiltration methods
- **Common tools and malware families**
- **Operational infrastructure**: Hosting patterns, preferred providers, regions
- **Extortion and negotiation tactics** (if applicable): Leak sites, communication style, ransom range, flexibility

#### 3f. Activity Timeline (WHEN)
- **Historical timeline**: Past campaigns and major events (prioritize those relevant to your org/industry)
- **During incident**: Living timeline with daily updates, behavior changes, and source references

#### 3g. Technical Evidence (Appendices)
In STIX 2.1 format where possible:
- **Atomic indicators**: IPs, domains, URLs, email addresses, file hashes, Bitcoin wallets
- **Tooling catalog**: Malware families with descriptions

### Step 4: Provide Analysis (What Next, So What, Now What)

This step is required for analytical deliverables. Skip for internal tracking profiles.

**Forecast (What Next?)**
- Will they escalate?
- Likely pivot targets?
- Data leak probability?
- Timeframe estimates

**Implications (So What?)**
- Impact on core business functions
- Operational disruption potential
- Data exposure risk
- Customer impact

**Recommendations (Now What?)**
- Defensive actions (prioritized)
- Detection priorities
- Playbook updates needed
- Specific steps for IR, SOC, and leadership

### Step 5: Document References

Maintain a transparent audit trail:
- Internal cross-references (incident reports, ticket numbers, SIEM references)
- Vendor reports (title, date, permanent link or archive.org snapshot)
- Community contributions (maintain TLP standards)
- Screenshots of ephemeral sources (X posts, Telegram messages)

### Step 6: Tailor the Deliverable

Produce the right output for the audience:

| Audience | Focus | Include | Exclude |
|---|---|---|---|
| Executive leadership | So What | Business risk, financial impact, high-level mitigation | Raw IOCs, technical TTPs |
| IR / SOC team | Now What | Detection logic, TTPs, huntable indicators | Strategic narrative |
| Threat Hunting / Red Team | Modus operandi | Technical evidence, kill chain detail | Business impact analysis |

**Executive summary**: Write it last. Use BLUF (Bottom Line Up Front) format. Place the most critical findings at the top.

**Cut-off date**: Always state the date the analysis represents. Intelligence is perishable.

## Output Format: Visual Diagram

The primary output of this skill is an **interactive visual diagram** rendered inline using the Visualizer (show_widget), NOT a lengthy markdown document.

### Design Principles

- Use the `visualize:read_me` tool (module: `diagram`) before generating, then use `visualize:show_widget`
- The diagram should be a single-page visual summary of the threat actor profile
- Use a dark theme consistent with threat intelligence tooling (dark background, accent colors for severity)
- The layout should be a structured card-based or panel-based design

### Required Visual Sections

The diagram must include these panels/cards arranged in a readable layout:

1. **Header bar**: Threat actor name, aliases, type, capability level (color-coded badge), cut-off date
2. **Diamond Model visualization**: Render as a classic diamond (rotated square) with four vertices connected by lines:
   - **Top vertex**: Adversary (name, type, origin country)
   - **Right vertex**: Infrastructure (C2 servers, hosting providers, domains)
   - **Bottom vertex**: Victim (targeted sectors, regions, org proximity)
   - **Left vertex**: Capability (tooling, malware families, exploit types)
   - Draw solid lines connecting all four vertices to form the diamond shape
   - On each connecting edge, annotate relevant meta-features: timestamps, operational phase, confidence level (Admiralty Code badge)
   - Center of the diamond: display the campaign or event name that ties the vertices together
   - Use a subtle glow or accent border on the diamond to make it stand out from the rest of the dashboard
3. **Motive and Objective**: Short tags or badges showing motive type and specific objectives
4. **Victimology panel**: Targeted sectors, regions, and proximity to the user's org (if known)
5. **Kill Chain / TTPs panel**: Key MITRE ATT&CK techniques grouped by phase (Initial Access, Execution, Persistence, etc.) shown as labeled badges or a mini flow
6. **Activity Timeline**: Horizontal timeline bar showing major campaigns/events with dates
7. **Capability gauge**: Visual indicator (High/Moderate/Low) with supporting detail
8. **So What / Now What panel** (for analytical deliverables only): Key implications and top 3 prioritized recommendations
9. **Key IOCs summary** (if available): Small table or list of critical indicators
10. **Sources and confidence**: Admiralty Code ratings shown as letter-number badges

### Color Coding

- **Capability High**: Red (#ef4444)
- **Capability Moderate**: Orange (#f59e0b) 
- **Capability Low**: Green (#22c55e)
- **State-sponsored**: Purple badge
- **Cybercrime**: Red badge
- **Hacktivist**: Blue badge
- Use CSS variables from the Visualizer design system for all other colors

### Sizing

- Target viewport: 900x700 minimum
- Use scrollable sections if content overflows
- Cards should have rounded corners, subtle borders, and clear spacing

### Interactivity (optional but encouraged)

- Clickable MITRE ATT&CK technique badges that show descriptions on hover
- Expandable panels for detailed IOC lists
- Timeline hover showing campaign details

### Fallback

If the user explicitly asks for markdown, a document, or a file export, fall back to a structured markdown document using this template:

```
# Threat Actor Profile: [NAME]

**Cut-off Date**: [DATE]
**Classification**: [TLP level]
**Profile Type**: [Internal Tracking | Analytical Deliverable]
**Prepared for**: [Audience]

## Executive Summary
## 1. Identity and Attribution
## 2. Motive and Objective
## 3. Victimology
## 4. Capability Assessment
## 5. Modus Operandi
## 6. Activity Timeline
## 7. Forecast, Implications, and Recommendations
## 8. Technical Evidence (Appendix)
## 9. References
```

## Tips

- If the user provides a threat actor name, search the web for recent activity before building the profile
- Cross-reference aliases across naming conventions (e.g., Microsoft, Mandiant, CrowdStrike naming schemes)
- For ransomware groups, always check for leak site activity and negotiation patterns
- When data is sparse, explicitly state gaps and confidence levels rather than guessing
- Use the Admiralty Code ratings in the references section to signal source quality
- Always call `visualize:read_me` with module `diagram` before generating the visual output
