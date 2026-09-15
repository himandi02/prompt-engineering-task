# Iteration Summary: Prompt Testing & Optimization

This report consolidates the prompt engineering experiments, iterations, and evaluation metrics across all four task notebooks in the clothing review analysis pipeline.

---

## 1. Data Analysis

| Prompt Version | Weakness | What Changed | Why It Worked |
| --- | --- | --- | --- |
| `prompt_v1_naive` | Vague command (`"tell me what's going on"`) generated sprawling, unstructured prose, arbitrary sentiment groupings, and unsolicited data mining advice.

 | Defined a product analytics assistant role, required top 3 complaints and praises grouped by theme, added a rating-vs-recommendation check, and mandated two Markdown tables.

 | Explicit formatting constraints and target definitions forced the model to extract empirical themes instead of offering open-ended methodology advice.

 |

* **Quantified Detail:** The naive prompt (`50` characters) yielded generic paragraphs, whereas the improved prompt (`935` characters) constrained the output to structured Markdown tables. When evaluating truncated inputs, the improved prompt strictly adhered to the schema (leaving empty complaint rows) rather than hallucinating content.



---

## 2. Summarization

| Prompt Version | Weakness | What Changed | Why It Worked |
| --- | --- | --- | --- |
| `single_v1` vs. `single_v2` | `single_v1` fragmented the text into an itemized profile that prioritized peripheral styling trivia (e.g., shoe color, accessories) over core review sentiment. | Assigned an e-commerce CX analyst persona, enforced a 1-sentence constraint, capped length at 25 words, and mandated preserving primary praise or complaints.

 | Bounded length constraints forced the model to prune decorative details and preserve high-signal evaluative sentiment. |
| `multi_v1` vs. `multi_v2` | `multi_v1` used conversational padding and ungrounded qualifiers (`"multiple buyers"`, `"one reviewer"`) without verifiable counts. | Assigned a merchandising analyst role, restricted output to exactly 3 bullet points, required review frequency ratios (`X of 8`), and mandated short quotes.

 | Grounding rules prevented speculative generalizations by anchoring executive insights directly in empirical sample counts. |

* **Quantified Detail:** On single-review summarization, `single_v2` condensed detailed feedback into an exact 18-word sentence (strictly under the 25-word cap). On multi-review synthesis, `multi_v2` quantified category themes using sample ratios (e.g., `"2 of 8 reviews"`) paired with verbatim quotes (`"it would cover quadruplets"`, `"it is totally worth it"`, `"this dress is stunning"`).



---

## 3. Classification

| Prompt Version | Weakness | What Changed | Why It Worked |
| --- | --- | --- | --- |
| `classify_v1` | Vague instruction (`"tell me if these reviews are good or bad"`) produced arbitrary conversational labels (`"Bad"`, `"Mixed / Leaning Bad"`), paragraphs of justification, and dropped test rows.

 | Enforced an e-commerce sentiment specialist role, closed 3-label taxonomy (`Positive`, `Negative`, `Neutral`), sequential ordering, and a 3-column Markdown table.

 | Standardizing labels and tabular output transformed unstructured narrative predictions into parseable, evaluable records.

 |

* **Quantified Detail:** `classify_v2` achieved a **6/10 (60%)** baseline match against the rating heuristic (`1–2★ = Negative`, `3★ = Neutral`, `4–5★ = Positive`), with **100% (4/4)** precision on 1–2 star negative reviews and **100% (5/5)** precision identifying items with `Recommended IND = 0`. It surfaced critical text-vs-rating edge cases, such as Review 10 (rated 5★ and recommended, but classified as `Negative` due to severe maternity-fit complaints in the text).



---

## 4. Content Generation

| Prompt Version | Weakness | What Changed | Why It Worked |
| --- | --- | --- | --- |
| `reply_v1` vs. `reply_v2` | `reply_v1` generated generic, open-ended responses lacking specific defect acknowledgment and actionable resolution steps. | Assigned a customer care specialist role, required specific acknowledgment of the described issue, mandated concrete resolution options (refund/exchange), and set an 80-word cap. | Explicit recovery parameters prevented boilerplate corporate apologies and produced concise, actionable remediation. |
| `blurb_v1` vs. `blurb_v2` | `blurb_v1` allowed ungrounded marketing claims, generic copy, and invented product attributes. | Assigned a copywriter role, restricted features strictly to positive themes in the 10 provided reviews, capped output at exactly 3 sentences, and required a call-to-action. | Constraining source material to empirical reviews prevented deceptive copywriting and maintained an upbeat retail voice. |

* **Quantified Detail:** `reply_v2` capped customer recovery messaging strictly under the 80-word limit while embedding concrete return/exchange pathways; `blurb_v2` restricted marketing copy to exactly 3 sentences ending with a single direct call-to-action.

---

## Overall Patterns Across All Four Task Types

* **Role & Persona Definition:** Assigning domain-specific personas (e.g., product analytics assistant, merchandising analyst, customer care specialist) shifted the LLM from conversational tutoring to domain-appropriate tone and conciseness.


* **Output Schemata & Scannability:** Mandating explicit Markdown table structures and bulleted schemas eliminated rambling preambles, transitional filler, and unsolicited methodology explanations.


* **Negative & Boundary Constraints:** Setting explicit caps (word limits, sentence counts, closed label taxonomies) prevented the model from expanding into peripheral details or hallucinating non-existent product features.


* **Grounding in EDA Discoveries:** Incorporating data exploration findings—such as 5-star rating skew, whitespace and line break normalization, and rating-vs-recommendation discrepancies—ensured balanced testing samples and revealed edge cases where customer review text contradicted numeric ratings.