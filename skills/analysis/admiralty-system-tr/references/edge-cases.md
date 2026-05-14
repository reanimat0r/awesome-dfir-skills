# Admiralty System: Edge Cases

Load this file when the main SKILL.md examples do not cleanly cover the situation. These six scenarios are the most common edge cases in CTI and OSINT work where a naive A-F / 1-6 read leads to the wrong answer.

## 1. Two vendors with contradictory attributions on the same campaign

**Scenario**: Mandiant attributes a campaign to APT29. Kaspersky attributes the same campaign to a different actor, "TURLA-adjacent". Both publish technical reports with overlapping IoCs.

**Why it is tricky**: Two A-rated sources contradicting each other does not average out to B. The information credibility does not collapse to 4 just because there is disagreement. The two claims are different, not opposed.

**How to rate**:
- Rate each vendor as a source independently. Both can be A.
- Rate each claim independently. Mandiant's claim "this is APT29" is one piece of information. Kaspersky's claim "this is TURLA-adjacent" is a separate piece of information.
- Both can be A3 (possibly true, plausible from a reliable source, no independent corroboration of the attribution itself).
- The overlap in IoCs is A1 (confirmed by independent sources). The attribution layered on top is not.

**Pitfall**: Do not let the disagreement push you to F or 6. The disagreement is signal, not noise. It tells you the attribution layer is not yet confirmed even when the technical evidence is solid.

## 2. Sensor or honeypot data (system as source)

**Scenario**: A honeypot in your sensor network captures exploitation attempts and the captured payload matches a published CVE. The honeypot report is fully automated, no human involved.

**Why it is tricky**: A system feels objective and tempts you to rate it A1. But systems can be poisoned, misconfigured, or compromised, and they also have a track record like any source.

**How to rate**:
- Rate the sensor as a source. How long has it been deployed? Have its captures been corroborated against external feeds? Has it ever produced false positives? A long-running honeypot with externally validated captures can be B or A. A new sensor with no track record is C or D.
- Rate the information separately. Does the captured payload validate against the published CVE? Can you reproduce the exploitation in a lab? Has another sensor in another region captured the same payload?
- Cross-check internal logs against external logs. If the sensor reports an exploit attempt but the upstream firewall does not see the matching traffic, the sensor is potentially compromised.

**Pitfall**: Treating system output as automatically A1 because it is "machine generated". The machine is still a source. Rate it like one.

## 3. Aggregated feed data (origin cannot be traced)

**Scenario**: A commercial threat feed pushes 200 IoCs per day. The feed aggregates from 30 upstream sources. You cannot tell which upstream provided which IoC.

**Why it is tricky**: You cannot rate the source because you do not know what the source actually is.

**How to rate**:
- The aggregator is the source you can see. Rate the aggregator on their history of accuracy and false-positive rate. This is typically C or D for most commercial feeds because they do not filter.
- The information cannot be rated higher than the weakest upstream. If you do not know the upstreams, the information is at best 3 (possibly true) and often 4 (doubtful).
- Insist on provenance from the feed vendor. Any vendor unwilling to disclose upstream sources should drop a full letter grade.

**Pitfall**: Trusting feed data because the vendor is well-known. The vendor's reputation does not transfer to the underlying claims if the vendor cannot trace them.

## 4. Law enforcement attribution with sealed evidence

**Scenario**: The FBI or AFP publicly attributes a campaign to a named actor or country, citing classified or sealed evidence not available to you.

**Why it is tricky**: You are being asked to trust a high-reputation source on information you cannot independently verify. The institutional history of the source is strong but the specific evidence is invisible.

**How to rate**:
- Source rating reflects the institutional track record. FBI on cyber attribution is typically B or A depending on the case. Note: track records vary by case type. They are stronger on financial cybercrime than on state-sponsored attribution.
- Information rating cannot rise above what you can verify. Without access to the sealed evidence, the information is at best 3 (possibly true) and usually 4 (doubtful from external view).
- An indictment is stronger than a press release because it carries legal weight and can be challenged in court. Treat indictments as 2 or 3. Treat press releases as 3 or 4.

**Pitfall**: Letting the institutional weight inflate the information rating. Source and information are separate axes. An A source making an unverifiable claim is still A6 if you genuinely cannot judge the claim.

## 5. Insider leak (hostile motivation, technically accurate information)

**Scenario**: A disgruntled employee leaks internal documents from a company. The documents are technically accurate but the leaker's motivation is revenge, financial gain, or political damage.

**Why it is tricky**: The motivation tempts you to rate the source low (D or E), but the documents themselves are real.

**How to rate**:
- Source rating reflects reliability, which is about whether the information they provide is accurate, not whether their motivation is pure. A leaker with verifiable insider access whose documents have been validated as authentic is C or B. A leaker whose past leaks have been falsified or selectively edited is E.
- Information rating reflects whether the data is true. If the documents check out (metadata, internal references, formatting match), they can be 2 or 1 even from a hostile source.
- Flag the motivation in your reasoning. A B2 from a hostile leaker is still B2, but stakeholders need to know the leak is selective by design and not a complete view.

**Pitfall**: Conflating motivation with reliability. Reliability is about accuracy. Motivation is context for interpreting what the source chose to share and what they may have withheld.

## 6. Recycled-data detection (especially Australian and US breach claims)

**Scenario**: An actor on BreachForums claims a fresh breach against a major Australian retailer. The data sample includes 2M records with name, email, phone, DOB.

**Why it is tricky**: Australia has had several large public breaches in recent years (Optus 2022, Medibank 2022, Latitude 2023, Dymocks 2023, MediSecure 2024). US has more. Repackaged data from these is a common scam. The sample data will look real because it IS real, just not from where the actor claims.

**How to rate**:
- Hash-check the sample against known prior leaks before assigning any rating. Use Have I Been Pwned, internal CTI databases, and previous samples you have collected.
- If the sample matches a known prior breach in part or whole, source rating drops to E and information rating drops to 5.
- If hash-check is clean, proceed with the normal workflow. Source rating depends on actor track record. Information rating depends on whether the data structure matches what the claimed victim would actually export.

**Pitfall**: Assuming "the data looks real" is positive evidence the breach is real. Recycled data is real data. The question is whether it is FROM the claimed victim.

## Cross-cutting rules

- When in doubt between two letters or numbers, pick the lower one. The system is designed to flag uncertainty honestly, not to look confident.
- Always re-rate every post in a case independently, even when the actor is the same. Source reputation can lift the floor, but each post stands on its own.
- The four colour states from the SKILL.md workflow (confirmed green, partially validated yellow, pending red) apply to claims, not to whole reports. A single report can contain green, yellow, and red claims at the same time. Break it apart.
