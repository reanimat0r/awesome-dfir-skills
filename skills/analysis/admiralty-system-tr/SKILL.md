---
name: admiralty-system
description: Apply the NATO Admiralty System (AJP-2.1) to assess source reliability and information credibility in cyber threat intelligence, OSINT, and breach analysis. Use this skill whenever you need to evaluate a CTI report, breach claim, dark web forum post, threat actor advertisement, vendor blog, social media intel claim, leaked database listing, or any source plus information pair where trust matters. Trigger phrases include "assess this source", "rate this report", "is this breach real", "evaluate credibility", "source assessment", "should I trust this claim", "admiralty rating", "A1 to F6", and any review of CTI or OSINT material where you need to decide how much weight to give it. Use proactively when the user shares a breach post, threat actor claim, or vendor report and asks for analysis, even if they do not explicitly mention the Admiralty System. Also use when teaching, building courseware, or producing a training example around source evaluation.
---

# Admiralty System for CTI

The Admiralty System (NATO AJP-2.1) is the gold standard for assessing intelligence. The British Royal Navy developed it in the early 20th century. NATO intelligence communities now use it worldwide, and it is gaining ground in cyber threat intelligence.

It rates two things, separately:

- **Source Reliability** (A to F): how trustworthy the origin is
- **Information Credibility** (1 to 6): how trustworthy the data is, independent of the source

The combined output is an alphanumeric code such as A1, B3, F6.

## Core rule: assess source and information SEPARATELY

This is the single most common mistake. A source who is reliable on malware analysis is not automatically reliable on geopolitics. A piece of information can be true from a sketchy source, or false from a usually reliable one. Always rate them independently, never in lockstep.

Second common mistake: confusing "two vendors reported it" with two independent sources. If both vendors pull from the same original dataset (the same breach forum post, the same scan, the same leak), that is ONE source. Independent corroboration means independent collection.

## Source Reliability Scale (A to F)

| Code | Label | Use when |
|------|-------|----------|
| A | Completely Reliable | Source has consistently delivered accurate information over a long history on this specific topic. Reserve this rating; it is rare. |
| B | Usually Reliable | Source has a strong track record with only minor lapses on this topic |
| C | Fairly Reliable | Source has produced accurate information before but the track record is shorter or mixed |
| D | Not Usually Reliable | Source has been wrong more often than right, but cannot be fully dismissed |
| E | Unreliable | Source has a clear history of inaccuracy, deception, or manipulation |
| F | Reliability Cannot be Judged | New source, no track record, or insufficient access to assess |

Attributes to weigh:

- History of accuracy on this specific topic (not other topics)
- Proximity to the event (firsthand, secondhand, hearsay)
- Technical skill in the domain
- Motivation (financial gain, reputation, ideology, deception)
- Identity corroboration (links to known infrastructure or aliases)
- Forum reputation score, post count, join date
- Administrator or moderator status on the forum
- Documented links to other known threat actors or breaches

## Information Credibility Scale (1 to 6)

| Code | Label | Use when |
|------|-------|----------|
| 1 | Confirmed by Other Sources | Information is corroborated by multiple genuinely independent sources |
| 2 | Probably True | Logical, consistent with known facts, and partly corroborated |
| 3 | Possibly True | Logical and plausible but not independently confirmed |
| 4 | Doubtful | Possible but unconfirmed and partly inconsistent with other facts |
| 5 | Improbable | Contradicts known facts or is internally inconsistent |
| 6 | Truth Cannot be Judged | Insufficient information to assess at all |

Attributes to weigh:

- Internal logical consistency
- Fit with prior known facts
- Independent corroboration (genuinely independent, not echoed)
- Technical plausibility (does the claimed attack chain actually work)
- Presence of sample data, hashes, or other concrete proof
- Time elapsed since the event
- Whether the data could be recycled from earlier breaches

## Workflow

When asked to assess a source or information pair, follow this sequence:

### 1. Extract structured metadata

For each post, report, blog, or claim, pull out these fields:

- Case name
- Forum or platform
- Post content (what is being claimed)
- Date of post
- Author handle
- Communication identifiers (XMPP, Telegram, email, Tox)
- Associated handles or known aliases
- Forum section
- Post link (defanged)
- Author first seen date
- Author involvement in other cases
- Author forum reputation
- Author observed in other forums

This structured extraction forces you to look at the evidence before you score anything.

### 2. Rate the source (A to F)

Walk through the source attributes. Justify the letter with one or two sentences of reasoning. If the user is tracking the same case across multiple posts, rate each post's source independently. Ratings can change as a track record builds.

### 3. Rate the information (1 to 6)

Walk through the information attributes. Has anyone independently corroborated this? Is the technical claim plausible? Is the sample data verifiable? Could it be recycled data from an older breach? Justify the number with one or two sentences.

### 4. Recommend next steps

For low-confidence ratings (E5, F6, D4, and similar), suggest specific verification actions:

- Cross-check on other forums or marketplaces
- Pull a sample of the data and validate against known schemas
- Check the original infrastructure (login endpoints, archived snapshots, Wayback, FOFA)
- Compare timestamps with public incident announcements
- Hash-check leaked samples against known prior leaks to detect recycled data
- Look for admin endorsement or community vouching

### 5. Suggest SATs when relevant

When you are looking at a developing case with multiple posts over time, recommend two structured analytic techniques:

- **Timeline**: showing how source and information ratings evolve over each post
- **Link chart**: showing relationships between aliases, infrastructure, and forums, with each edge colour-coded as confirmed, partially validated, or pending validation

These externalise your reasoning and let peers challenge it. They are also what a stakeholder needs to follow the analysis.

### 6. Always flag alternative hypotheses

Before locking in an assessment, list at least two competing explanations:

- The actor really did breach the company
- The actor is recycling old or unrelated data
- The actor is aggregating multiple smaller leaks and rebranding them as one
- The data is from a third-party processor, not the named company
- The post is a reputation play with no real data behind it
- The post is misinformation or disinformation

The Admiralty rating should reflect the most likely hypothesis AFTER you consider the alternatives, not before.

## Output Format

Always produce a structured assessment table. Use this template:

```
| Field | Value |
|-------|-------|
| Case Name | ... |
| Forum / Platform | ... |
| Post Content | ... |
| Date of Post | ... |
| Author | ... |
| Communication Identifiers | ... |
| Associated With | ... |
| Forum Section | ... |
| Post Link | ... |
| Author First Seen | ... |
| Author Involved in Other Cases | ... |
| Author Forum Reputation | ... |
| Author Observed in Other Forums | ... |
| Source Reliability | X (label) |
| Information Credibility | Y (label) |
| Reasoning (source) | ... |
| Reasoning (information) | ... |
| Alternative Hypotheses | ... |
| Recommended Next Steps | ... |
```

When tracking the same case over time, add a final section:

- **Timeline of ratings**: post 1 (date, rating), post 2 (date, rating), and so on
- **Observed trend**: for example, rating climbing from F6 to B2 as corroboration arrives

## Hard constraints

- NEVER conflate the source rating with the information rating. They are independent axes.
- NEVER count two reports using the same underlying data as independent corroboration.
- NEVER give an A rating casually. A is reserved for sources with a long, well-documented, topic-specific track record.
- When unsure, prefer F or 6 over guessing. "Cannot be judged" is a valid and honest rating.
- ALWAYS justify the rating with a short written reason. A bare letter-number score is not enough.
- ALWAYS preserve the system as defined (NATO AJP-2.1). Do not invent custom variants. Otherwise teams cannot compare assessments and the system collapses into noise.

## When to flag uncertainty in the output

If the rating is D, E, F, 4, 5, or 6, wrap the assessment in caveats:

- "We assess this to be [rating] based on available information, with the following limitations..."
- "This rating may change as the case develops"
- "Pending [verification step], this assessment should be treated as preliminary"

This protects the reader from treating a preliminary rating as a confirmed fact, and it protects you when the case evolves.

## Examples

### Example 1: New dark web forum post

**Input**: A new user "SpidermanData" on Exploit[.]in posts a sale for 560M Ticketmaster user records at $500,000. Account has 2 posts and joined yesterday.

**Output**: Source = F (reliability cannot be judged, no track record). Information = 6 (truth cannot be judged, no sample provided, no corroboration). Next steps: monitor for sample drop, cross-check BreachForums, watch for admin endorsement, check XMPP identifier against known actor inventories.

### Example 2: Established threat actor reposts the claim

**Input**: ShinyHunters (BreachForums administrator, reputation score 1,087, known for Tokopedia, AT&T, Santander breaches) reposts the same Ticketmaster claim with folder size data showing 1.3TB.

**Output**: Source = B (usually reliable, strong track record on breach claims, admin status). Information = 3 (possibly true, structural metadata consistent with a real breach, but no public sample yet). Next steps: pull the folder structure and compare against known Ticketmaster export schemas.

### Example 3: Vendor report citing a single dark web source

**Input**: Vendor X publishes a report titled "Biggest supply chain hack of 2025" based on a BreachForums post from a new actor "rose87168".

**Output**: The vendor source rating depends on the vendor's own track record (rate it separately). The underlying claim source remains the new actor (F). The vendor putting their badge on the claim does NOT upgrade the original source rating. Information credibility depends on what the vendor independently validated versus what they just repeated. Build a link chart and colour each claim: confirmed (green), partially validated (yellow), pending validation (red). Push back on any claim that sits in yellow or red but is presented as fact.

### Example 4: Trusted vendor with multi-source corroboration

**Input**: A top-tier CTI vendor with a long, accurate track record on this threat actor publishes a report on an actively exploited zero-day. Three independent vendors confirm the same exploit chain from their own telemetry.

**Output**: Source = A (completely reliable, long topic-specific history). Information = 1 (confirmed by multiple independent sources, each with their own collection). Note: even at A1, state the limitations and the assessment date, because the situation can shift.

## Notes on terminology

- "Information" in this context means the data and claims being conveyed, not the platform or channel.
- "Source" means the originator of the claim, which can be a person, a forum account, a system, or a sensor.
- A system or sensor is still a source, and logs can still be tampered with. Cross-check internal and external logs for consistency before trusting either.
- Treat each post by the same actor as a separate observation. A B-rated actor can drop an F-rated post if the specific claim is implausible.

## Bundled Resources

This skill ships with two extra files. Load them only in the specific situations below, not on every assessment.

### references/edge-cases.md

Read this file when the situation does not cleanly match the four examples in SKILL.md. Covers six tricky scenarios:

- Two vendors with contradictory attributions on the same campaign
- Sensor or honeypot data (system as source)
- Aggregated feed data where origin cannot be traced
- Law enforcement attribution with sealed evidence
- Insider leaks where motivation is hostile but information is accurate
- Recycled-data detection in breach claims (Australian and US examples)

If you are rating any of the above, read the file before producing your assessment.

### assets/case-tracking-template.csv

Use this file when the user is tracking the same case across multiple posts over time. It is a CSV with all assessment fields as columns and one example row populated with the SpidermanData Ticketmaster post.

When the user says they want to track a case, hand them a copy of this file populated with rows for each post observed so far. This mirrors the Freddy Murstad / Sean O'Connor workflow: one row per post, ratings change as the case develops, the timeline of ratings becomes the audit trail.
