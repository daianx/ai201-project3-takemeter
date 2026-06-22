# TakeMeter — planning.md
## AI201 · Project 3

---
## Community Choice

**Community:** The online book reviewing community (specifically aggregating from Goodreads, StoryGraph, and LibraryThing).

**Rationale:** I've been trying to read more, and book reviews are a good way to look for books to read. They are spaces with a lot of variation, where professional literary critique sits side-by-side with pure emotional venting. What makes a "good take" here is highly debated: some readers want objective analysis of prose and pacing, while others purely want to know if the book will make them cry. This discourse is incredibly varied in quality, length, and intent, making it a rich target for classification. Additionally, book reviews are long enough to contain classifiable features (unlike tweet-length posts) but short enough to remain a single unit of discourse.

Using three platforms deliberately introduces cross-platform variation. GoodReads skews toward casual readers and hype-driven reviews; StoryGraph attracts a more analytically inclined readership; LibraryThing has an older, catalog-oriented user base that tends toward descriptive reviews. This cross-platform variation should produce a naturally balanced label distribution rather than requiring heavy oversampling of underrepresented classes.

Finally, book reviews are public, text-only, and exist in large volume — making data collection feasible without authentication or scraping infrastructure.

---

## Label Taxonomy

This taxonomy classifies book reviews along a single axis: **what kind of engagement the reviewer demonstrates with the book**. Each label captures a meaningfully different mode of review writing that would be recognized as distinct by active participants in book communities.

| Label             | Definition                                                                                                                                                                                             |
| :---------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Analytical**    | The review discusses craft elements — pacing, prose style, narrative structure, theme, or comparative context. The argument is about the book as a made thing, not primarily the reader's feelings.    |
| **Personal**      | The review centers the reader's emotional or biographical response: how the book affected them, what it reminded them of, how it connects to their life. Specific to the reader, not a craft argument. |
| **Summary-heavy** | The review is primarily plot retelling with minimal evaluative content. Often "here's what happens" with a brief verdict tacked on at the end.                                                         |
| **Reactive**      | The review is a polarized, uncritical take — strong positive or negative without elaboration. Star ratings narrated as prose, hype reactions, emojis and rage-quit posts fall here.                    |

### **Labels
---

### Label 1: Analytical

**Definition:** The review makes an arguable claim about the book as a crafted object, supported by reference to at least one specific element of the text. Analytical reviews discuss craft dimensions such as pacing, prose style, narrative structure, character development, thematic content, or how this book compares to the author's other work or similar books. The central argument is about *what the book does and how well it does it*, not primarily about how the reader felt reading it.

**Example 1:**
> "Martínez's prose is precise to the point of being cold, which works in the first two-thirds — the detachment mirrors the narrator's dissociation — but the final act demands emotional warmth the writing simply can't deliver. It's a structural mismatch between style and intention, and it keeps an otherwise excellent novel from landing."

**Example 2:**
> "The dual timeline structure that worked so well in her debut is starting to feel like a formula here. The 1940s thread is genuinely compelling; the present-day frame feels like obligation. I kept wishing she'd trusted the historical story to carry the whole novel on its own."

---

### Label 2: Personal

**Definition:** The review centers the reader's emotional, biographical, or situational response to the book. Personal reviews are specific to the reviewer's own life — they explain *why this book mattered to this reader*, not why it is good or bad as a crafted object. The review may express strong feeling, but the feeling is grounded in a particular personal anchor: a memory, a relationship, a life circumstance, or a deeply specific emotional reaction tied to the reviewer's experience.

**Example 1:**
> "This book reminded me so much of my grandmother. I cried for hours because the main character's journey perfectly mirrored my own childhood trauma. It felt like the author was reading my diary."  

**Example 2:**
> "As someone who grew up in a small fishing town, the scenes in the harbor felt so specific and so true that I kept having to put the book down. It wasn't nostalgia exactly — more like being seen in a way I didn't expect fiction to be capable of."

---

### Label 3: Summary-heavy

**Definition:** The review is dominated by plot retelling or description of the book's premise, with minimal evaluative content. Summary-heavy reviews tell the reader *what happens* more than they tell the reader *whether or how well it works*. A verdict sentence ("I enjoyed it" or "not for me") may appear, but it is not supported or elaborated. These reviews are often written shortly after finishing the book, in a "here's what I just read" mode.

**Example 1:**
> "This is about a woman named Clara who moves back to her hometown after a divorce and starts uncovering secrets about her family's past. There are flashbacks to her grandmother's life in the 1950s. A mystery slowly develops around a disappearance. I thought it was pretty good. Would recommend to fans of historical fiction."

**Example 2:**
> "Set in near-future Seoul, the story follows two brothers on opposite sides of a political movement. It deals with themes of loyalty and identity. The ending was surprising. Gave it 4 stars."

---

### Label 4: Reactive

**Definition:** The review expresses a strong positive or negative response without substantiation or personal grounding. Reactive reviews are characterized by high emotional intensity and low specificity — they declare rather than argue or explain. This includes hype posts ("absolutely devoured this in one sitting"), rage-quit announcements ("DNF'd at 30%, life is too short"), and star-rating-as-prose reviews ("five stars, no notes, a masterpiece"). The review may be enthusiastic or dismissive, but it does not anchor the reaction to anything specific about the book or the reviewer's life.

**Example 1:**
> "OBSESSED. Cannot stop thinking about this book. Recommended it to literally everyone I know. Just trust me and read it."

**Example 2:**
> "DNF at 40%. Boring, slow, nothing happens. I don't understand the hype at all. Not for me."

---

## Edge Cases and Decision Rules

### Edge case 1: Reactive vs Personal

Both labels involve emotional language, and the boundary is a matter of specificity. This is the most likely source of inter-annotator disagreement.

**Case 1: Intensity without grounding**
> "I sobbed through the last fifty pages. This book broke me."

**Decision: Reactive.** The emotional reaction is declared but not anchored to anything specific about the reviewer's life or the book's content. The intensity is the whole review.

**Case 2: Emotion anchored to a biographical fact**
> "I sobbed through the last fifty pages. I lost my sister two years ago and I wasn't prepared for how directly this book would speak to that."

**Decision: Personal.** Same emotional intensity, but now grounded in a specific life event. The biographical anchor is what makes this *this* reviewer's review, not a generic reaction any reader might post.

**Case 3: The genuinely hard one**
> "This book absolutely wrecked me. I wasn't expecting it to hit so close to home — something about the way the mother-daughter relationship is written just got me."

**Decision: Reactive — but document it.** "Hit close to home" gestures toward personal grounding but doesn't supply it. There's no named relationship, event, or circumstance. The vagueness keeps it Reactive. If the reviewer had followed up with *why* it hit close to home, it would cross into Personal.

**Underlying decision rule:** A personal anchor must be specific enough that it couldn't apply to any reader. "This reminded me of my own grief" is still too vague — Reactive. "This reminded me of the year my mother stopped recognizing me" is specific — Personal.

**Quick test:** Ask "Could this sentence appear in a review of any emotionally resonant book, by any reader?" If yes, it's Reactive. If the specificity makes it *this* reader's review, it's Personal.

---

### Edge case 2: Analytical vs Summary-heavy

Some reviews summarize plot as a vehicle for making a craft argument. The plot retelling is in service of an evaluative claim, not an end in itself.

**Case 1: Summary that supports a craft claim**
> "The first act is slow — we spend 80 pages establishing setting before anything happens — which makes the payoff feel unearned. The ratio of setup to resolution is off."

**Decision: Analytical.** The page-count observation is evidence for a craft argument about pacing. The summary is doing argumentative work.

**Case 2: Summary with a verdict tacked on**
> "This follows a woman returning to her hometown after a divorce, uncovering secrets about her family's past. There are flashbacks to the 1950s and a slow-building mystery. I thought it was pretty good overall."

**Decision: Summary-heavy.** The verdict ("pretty good") appears but is not supported or elaborated. The bulk of the review is plot description that exists on its own terms.

**Case 3: The genuinely hard one**
> "The story alternates between two sisters in the 1940s and the present day. The historical thread is compelling — the author clearly did her research — but the modern thread drags. It felt uneven."

**Decision: Analytical — but document it.** There's a structural claim (uneven dual timeline) supported by a brief evaluative distinction between the two threads. The summary is there, but it's in service of the argument. Remove the summary sentences and a craft claim still stands: "The historical thread is more compelling than the modern thread; it felt uneven."

**Underlying decision rule:** If the summary is used to support a craft claim, classify as **Analytical**. If the summary exists on its own with only a verdict sentence attached, classify as **Summary-heavy**.

**Strip test:** Remove the plot-description sentences. Does a clear evaluative argument remain? If yes, it's Analytical. If what's left is only a one-line verdict or nothing at all, it's Summary-heavy.

---

### Edge case 3: Analytical vs Personal hybrids

Reviews that mix craft observation with personal response are common and require a consistent decision rule.

**Case 1: Personal hook, then craft argument**
> "I've always struggled with books about grief — they either feel manipulative or sanitized. This one didn't. The fragmented chapter structure mirrors the way grief actually works: non-linear, repetitive, suddenly interrupted by mundane details. It's the most formally honest treatment of loss I've read."

**Decision: Analytical.** The personal opener ("I've always struggled with...") is one sentence establishing credibility. The rest is a craft argument about structure and formal choices. The personal hook is throat-clearing, not the substance.

**Case 2: Craft observation grounded in personal reaction**
> "The prose style reminded me of Marilynne Robinson — long, meditative sentences that reward patience. I'm the kind of reader who loves that. I know it won't work for everyone."

**Decision: Personal.** There's a craft-adjacent observation (prose comparison), but the reviewer immediately turns it inward: "I'm the kind of reader who loves that." The argument is about fit between book and reader, not whether the prose is good. Analytical reviews argue the case to everyone; Personal reviews argue the case for themselves.

**Case 3: The genuinely hard one**
> "The ending frustrated me because the reconciliation feels unearned — we spend 300 pages watching these characters wound each other and then two scenes of vulnerability are supposed to resolve it. I wonder if that's my own baggage about forgiveness talking, but I don't think so."

**Decision: Analytical — but document it.** The reviewer flags their own subjectivity, which looks Personal. But the actual claim ("reconciliation feels unearned because the page ratio is wrong") is a craft argument any reader could evaluate independently. The self-doubt is intellectual honesty, not personal grounding. Classify Analytical, note as a difficult case.

**Underlying decision rule:** The distinction is about who the argument is addressed to. Analytical reviews make claims a third party could evaluate: *is the pacing actually slow? is the structure actually fragmented?* Personal reviews make claims only the reviewer can verify: *did this remind them of their grandmother? are they a patient reader?*

**Strip test:** Remove all "I" sentences from the review. Does a clear craft argument survive? If yes, classify as **Analytical**. If what remains is plot summary or impressions without an arguable claim, classify as **Personal**.

---

### Edge case 4: Reactive vs Summary-heavy

Both are low-engagement labels, but for different reasons — Reactive reviews are high intensity with low content; Summary-heavy reviews are low intensity with moderate content (plot). The confusion is most likely with short reviews.

**Case 1: Pure intensity, no content**
> "This book was an absolute disaster. I genuinely cannot believe it won an award. Avoid."

**Decision: Reactive.** There is no plot description, no craft claim, no personal anchor. The review is entirely a verdict delivered at high volume.

**Case 2: Plot description with a flat verdict**
> "This follows a detective in 1920s Chicago investigating a string of disappearances. There's a love interest subplot and a twist ending. It was okay. Not sure I'd recommend it."

**Decision: Summary-heavy.** The verdict is present ("it was okay") but the bulk of the review is plot. The tone is flat, not intense — this is a "here's what I read" post, not a polarized reaction.

**Case 3: The genuinely hard one**
> "Nothing happens for 300 pages. The ending comes out of nowhere. I don't know what I just read."

**Decision: Reactive — but document it.** "Nothing happens for 300 pages" could look like a craft observation about pacing, but there's no argument attached — no explanation of what should have happened or why the pacing fails. It's a frustration declaration with plot-adjacent language. If the reviewer had followed with "the setup-to-resolution ratio is completely off," it would shift to Analytical.

**Underlying decision rule:** Reactive reviews declare a verdict at high emotional intensity. Summary-heavy reviews describe what the book contains at low emotional intensity. If the review has plot content but a strong emotional charge, ask: is the plot description in service of the reaction, or is it the main content? If it exists mainly to justify the outrage or enthusiasm, it's Reactive.

**Quick test:** Is the dominant register *intensity* or *description*? Intensity → Reactive. Description → Summary-heavy.

---

### Edge case 5: Personal vs Summary-heavy

Some reviewers retell plot while weaving in their own experience — "here's what happened in the book and here's how I felt about it as it unfolded." This can look like Summary-heavy with emotional color, or Personal with a lot of plot scaffolding.

**Case 1: Plot retelling with emotional reactions inserted**
> "The book opens with the protagonist leaving her marriage. I found that part slow. Then she moves to a new city and starts rebuilding — that section I loved. The ending was bittersweet. Overall a quiet, gentle read."

**Decision: Summary-heavy.** The emotional reactions ("I found that part slow," "that section I loved") are present but follow the plot beat by beat without any personal anchor. This is a reading log, not a personal reflection.

**Case 2: Personal experience framed through plot**
> "The scenes where she cares for her aging mother hit differently for me — I'm going through that right now with my own mom. I had to put the book down twice. The author gets the exhaustion exactly right."

**Decision: Personal.** The plot is mentioned but only as a hook to ground a specific, ongoing personal circumstance. The review is really about the reviewer's life; the book is the occasion.

**Case 3: The genuinely hard one**
> "I read this during a really hard week and the story of her recovery kind of mirrored what I was going through. The middle section dragged a bit but the ending felt earned. I gave it four stars."

**Decision: Personal — but document it.** "A really hard week" is vague as a personal anchor, but the reviewer explicitly connects their life circumstance to the reading experience ("kind of mirrored what I was going through"). The craft observation ("middle section dragged") is a single sentence that doesn't constitute an Analytical argument. The dominant register is personal resonance.

**Underlying decision rule:** Ask what the review is *about*. If the plot description is the frame and the personal experience is the point, it's Personal. If the reviewer's reactions are just color commentary on a plot walkthrough, it's Summary-heavy.

**Quick test:** Could you remove all plot description sentences and still understand why this book mattered to this reviewer? If yes, it's Personal. If removing the plot leaves almost nothing, it's Summary-heavy.

---

### Edge case 6: Reactive vs Analytical

This boundary is rarely ambiguous in practice — Reactive reviews lack arguable claims and Analytical reviews require them. The confusion arises when a reviewer delivers a strong evaluative judgment with just enough specificity to look like a craft argument.

**Case 1: Strong verdict with no support**
> "The writing is gorgeous. One of the best books I've read this year. A masterpiece."

**Decision: Reactive.** "The writing is gorgeous" names a dimension (prose) but makes no claim that could be evaluated or disagreed with. This is enthusiasm with craft-adjacent vocabulary.

**Case 2: Specific craft claim with elaboration**
> "The writing is gorgeous — Aciman-level sentences that slow you down in the best way. Every paragraph feels considered. It's a rare book where the prose itself is the argument."

**Decision: Analytical.** The claim is now specific and arguable: the prose is the primary vehicle of meaning, not just decoration. A reader could disagree with this assessment and point to evidence.

**Case 3: The genuinely hard one**
> "The pacing is terrible. It completely ruins what could have been a great book."

**Decision: Reactive — but document it.** "The pacing is terrible" names a craft dimension and sounds like an Analytical claim, but it's not supported or elaborated. There's no explanation of where the pacing fails or why. Compare to: "The pacing is terrible — the entire second act is setup for a twist that arrives in the last ten pages, leaving no room for the emotional payoff to land." That version is Analytical.

**Underlying decision rule:** A craft observation only qualifies as Analytical if it is *arguable and supported* — meaning a reader could examine the evidence and agree or disagree. A label ("the pacing is terrible") without evidence is still Reactive, even if it uses craft vocabulary.

**Quick test:** Could a reader push back on this claim with evidence from the book? If the claim is specific enough to be disputed, it's Analytical. If it's a verdict dressed in craft language, it's Reactive.

---

## Data Collection Plan

* **Sources:** Goodreads, StoryGraph, LibraryThing.  
* **Methodology:** Base selection on the "Goodreads Choice Awards: Readers' Favorite Books 2025".  
* **Sample Size:** Select the top 3 books from 10 categories (Fiction, Historical Fiction, Mystery/Thriller, Romance, Fantasy, Science Fiction, Horror, Nonfiction, Memoir, History/Biography). I removed the Audiobook, Romantasy, and YA categories since they overlap with other categories.  
* **Target:** 10 categories \* 3 books \* 3 reviews per book \* 3 sources \= \~270 reviews.  
* **Collection Format:** CSV with columns: book\_title, source, category, url, rating, text, ai_genearted_label, ai_reasoning, label, difficult, reasoning, label_2.
	* book_title: title of the book
	* source: GoodReads, The StoryGraph, or LibraryThing
	* category: One of the following categories
		* Fiction
		* Historical Fiction
		* Mystery & Thriller
		* Romance
		* Fantasy
		* Science Fiction
		* Horror
		* Nonfiction
		* Memoir
		* History & Biography
	* url: URL of the reviews for the book
	* rating: Rating given by the review 
	* text: The review
	* ai_generated_label: AI pre-batched label done before the human review
	* ai_reasoning: reasoning given for the ai_generated_label decision
	* label: human given label
	* difficult: Boolean to identify reviews that were difficult to give a label to
	* reasoning: reasoning for the label given
	* label_2: second reviewer for 36 of the reviews

**Collection strategy:**
- On each platform, for each book, select 3 reviews.
- Avoid long, drawn-out essays
- Aim to sample across star ratings — include 3-star reviews, not only 4-5 star and 1-2 star reviews, to capture Summary-heavy and ambivalent content that tends to cluster in the middle.
- Collect reviews as text, stripping images and attestations ("This book was provided for free for an honest review") before annotation.

**Annotation process:** Label each review independently before checking distribution. For ambiguous cases, apply the decision rules following the Edge Cases and Decision Rules section. Document any review that required more than 30 seconds of deliberation as a "difficult case" for the inter-annotator reliability section.

---

## Evaluation Metrics and Reasoning

**Primary metric: Macro F1**

* **Primary Metric:** Per-class F1 score. Overall accuracy is insufficient because the natural distribution of online reviews skews heavily toward "Reactive" and "Summary-heavy." F1 will ensure the model is actually learning the nuanced boundary of "Analytical" and "Personal" without just predicting the majority class.  
* **Secondary Metric:** Confusion Matrix analysis, specifically monitoring the false positive rate between "Reactive" and "Personal" (ensuring the model can tell a deep emotional connection from uncritical hype).

---

## Definition of "Good Enough" Performance

The classifier will be considered genuinely useful if it achieves an **F1 score of \>0.70 across all four classes**. For a community tool, it must be able to reliably filter out "Summary-heavy" and "Reactive" posts so users can intentionally sort for "Analytical" or "Personal" takes.

Reasoning: This task involves real subjective ambiguity, particularly at the Reactive/Personal boundary. A macro F1 of 0.70 indicates the model has learned meaningful distinctions across all four labels — well above random chance (0.25) and above a majority-class baseline. A model that performs below 0.70 likely has a label collapse problem (Reactive and Personal are being merged) and the taxonomy or data should be revisited.

---

## AI Tool Plan


**Intended use 1 — Brainstorm categories:**
Tool: Claude Opus 3/Gemini 3.1 Pro
Expectation: I provided the data sources, collection plan and columns to collect. I asked LLM what labels would work for this project. I also asked the LLM to identify and generate examples of the edge cases. 
Verification: I generated 5 options and chose the one that worked. I did ask the AI to combine categories. 

**Intended use 2 — **Label Stress-Testing:**
Tool: Claude Opus 3
Expectation: Before manual annotation, I will feed my definitions to an LLM and ask it to write 5 "borderline" reviews that blur the lines between Personal and Analytical. 
Verification: If I can't easily categorize them using my decision rules, I will refine the rules.  

**Intended use 3 —  **Annotation Assistance:**
Tool: Claude Opus 3
Expectation: I will use the LLM to do a "first pass" pre-labeling of my CSV based on my strict taxonomy. 
Verification: I will manually review the results and add my labels/reasoning. 

**Intended use 4 — Fine-tuning:**
Tool: Claude Opus 3
Expectation: Use an LLM to help determine how to adjust the hyperparameters to increase accuracy
Verification: Test the suggestions

**Intended use 5 — Failure pattern analysis (post-evaluation):**
Tool: Claude Opus 3
Expectation: Use an LLM to help analyze the specific wrong predictions. Feed it the misclassified reviews, the predicted label, and the true label, and ask it to identify patterns across the error set. 
Verification: Using 3 patterns, manually verify the patterns identified are correct. 

**Intended use 6 — planning and README review:**
Tool: Claude Opus 3
Expectation: Provide LLM my planning and README reviews. Have the LLM review these against the grading rubric. 
Verification: Read what the LLM has generated and make edits as needed. 

**Intended use 7 — Create Deployed Interface:**
Tool: Gemini Pro 3.1
Expectation: Provide the current Colab notebook to LLM and ask it to create a deployed app using Colab. Provide the required features. 
Verification: Read what the LLM has generated and make edits as needed. 