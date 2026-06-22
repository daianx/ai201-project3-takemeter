# Book Review Discourse Quality Classifier

This project trains a classifier to label the discourse quality of book reviews from GoodReads, StoryGraph, and LibraryThing. The goal is to distinguish between four modes of engagement: Analytical, Personal, Summary-heavy, and Reactive.

## Motivation

The online book reviewing community is a space where professional literary critique sits side-by-side with pure emotional venting. What makes a "good take" here is highly debated: some readers want objective analysis of prose and pacing, while others purely want to know if the book will make them cry. This discourse is incredibly varied in quality, length, and intent, making it a rich target for classification.  

Using three platforms deliberately introduces cross-platform variation. GoodReads skews toward casual readers and hype-driven reviews; StoryGraph attracts a more analytically inclined readership; LibraryThing has an older, catalog-oriented user base that tends toward descriptive reviews. This cross-platform variation should produce a naturally balanced label distribution.

## Data

The training data consists of ~270 book reviews collected from the top 3 books in each of 10 categories from the GoodReads Choice Awards 2025. For each book, 3 reviews were sampled from each of the three platforms.

The 10 categories are:
1. Fiction
2. Historical Fiction
3. Mystery & Thriller
4. Romance
5. Fantasy
6. Science Fiction
7. Horror
8. Nonfiction
9. Memoir
10. History & Biography

The data contains the following columns: 

- `book_title`: The title of the book being reviewed
- `source`: The platform the review was collected from (GoodReads, StoryGraph, LibraryThing) 
- `category`: The book's GoodReads Choice Awards category
- `url`: The URL of the original review
- `rating`: The star rating given by the reviewer (1-5)
- `text`: The text content of the review
- `ai_generated_label`: AI pre-batched label done before the human review
* `ai_reasoning`: reasoning given for the ai_generated_label decision
* ``label``: human given label
* `difficult`: Boolean to identify reviews that were difficult to give a label to
* `reasoning`: reasoning for the label given
* `label_2`: second reviewer for 36 of the reviews

## Labels

Each review is labeled with one of four categories:

| Label             | Definition                                                                                                                                                                                             |
| :---------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Analytical**    | The review discusses craft elements — pacing, prose style, narrative structure, theme, or comparative context. The argument is about the book as a made thing, not primarily the reader's feelings.    |
| **Personal**      | The review centers the reader's emotional or biographical response: how the book affected them, what it reminded them of, how it connects to their life. Specific to the reader, not a craft argument. |
| **Summary-heavy** | The review is primarily plot retelling with minimal evaluative content. Often "here's what happens" with a brief verdict tacked on at the end.                                                         |
| **Reactive**      | The review is a polarized, uncritical take — strong positive or negative without elaboration. Star ratings narrated as prose, hype reactions, emojis and rage-quit posts fall here.                    |


Labeling was performed in two stages:

1. Pre-labeling by an AI model (Claude Opus 3) based on the defined taxonomy. The AI-generated labels and reasoning are stored in the `ai_generated_label` and `ai_reasoning` columns.
2. Manual review and correction by human annotators, recorded in the `label` and `reasoning` columns. A `difficult` boolean column flags reviews that were hard to categorize.

A subset of 36 reviews was also independently labeled by a second annotator for inter-annotator agreement evaluation. Their labels are stored in the `label_2` column.

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
## Inter-Annotator Reliability

I compared the labels with the 36 values in label_2. These are the results:

- Number of dually-annotated examples: 36
- Percentage Agreement: 69.4%
- Cohen's Kappa: 0.588

**DISAGREEMENTS FOR ERROR ANALYSIS** 
Text: (1.5 stars) In this book, a girl named Louisa runs into her idol, the world famous artist who created her favorite painting. The artist dies and sends...
**I labeled: Analytical  |  They labeled: Reactive**

Text: I get the hype. But this one was just too broken for me and I didn’t need to feel that many sad things right now. ...
**I labeled: Reactive  |  They labeled: Personal**

Text: I have a like/love relationship with Fredrick Backman’s books. Some I like, some I like a lot, some I love, and some just blow me out of the water. “M...
**I labeled: Analytical  |  They labeled: Personal**

Text: OMGGGGGG TJR is just a treasure 🥹🤍🚀📖
this kinda wasn’t what i expected at ALL but of course i’m in awe. the last 20 pages had me so teary eyed!!!! he...
**I labeled: Reactive  |  They labeled: Personal**


Text: Was a fine read - easy writing style, but shocker to noone, this genre isn't exactly my bag.
What worked for me:
I did enjoy the structure of the bo...
**I labeled: Personal  |  They labeled: Analytical**

Text: Okay, I'm an straight, old, white guy. I'm SO not TJR's target audience. Like...not even a little bit.
But, when everyone was raving about DAISY JONE...
**I labeled: Reactive  |  They labeled: Personal**


Text: My first Taylor Jenkins Reid experience was Maybe in Another Life in 2015. She hasn't let me down over the past ten years. There have been ugly cries ...
**I labeled: Reactive  |  They labeled: Personal**


Text: Bury Our Bones in the Midnight Soil by V.E. Schwab
a stunning aesthetic drenched in emptiness
I am so f*cking disappointed.
This was my first V.E. Sc...
**I labeled: Personal  |  They labeled: Reactive**


Text: It’s rare to find a decently done vampire novel lately that isn’t mired in cliches and repeated storylines, but Schwab does the genre proud with her l...
**I labeled: Analytical  |  They labeled: Summary-heavy**


Text: This book is exactly what the blurb says it's going to be, with Zoe and Claire discussing the various Scottish witch trials and their forgotten victim...
**I labeled: Summary-heavy  |  They labeled: Analytical**


Text: This book was absolutely brilliant- so well researched, informative and also funny. It’s been on my list since I saw Zoe and Claire speak at the Wigto...
**I labeled: Summary-heavy  |  They labeled: Analytical**

---
## Model

The labeled data will be used to fine-tune a DistilBERT model using the Hugging Face Transformers library.

Target performance is a macro F1 score of >0.70 across all four classes.

Tech Stack: 

| Component          | Tool                                   | Notes                                                               |
| ------------------ | -------------------------------------- | ------------------------------------------------------------------- |
| Base model         | distilbert-base-uncased                | HuggingFace — free to download, no account needed                   |
| Fine-tuning        | Google Colab (free GPU)                | Free T4 GPU; fine-tuning DistilBERT on 200 examples takes ~5–15 min |
| Training libraries | transformers + datasets + scikit-learn | Pre-installed on Colab — no setup needed                            |
| Baseline LLM       | Groq (llama-3.3-70b-versatile)         | Free tier                                                           |

I faced a lot of problems with the Groq API key rate limits. 

---
## Evaluation

The trained model will be evaluated on a held-out test set. Key metrics:

- Macro F1 score across all classes
- Per-class F1 scores 
- Confusion matrix

Note: Using the default script gave worse metrics than the baseline (ex. 0.366 accuracy). I ended up making the following changes to the hyperparameters which helped increase the accuracy of the model:
- Increased the training epochs to 10 to give it more time to learn
- Lowered warmup_steps to 15 so the model will warm up quicker
## Baseline Comparison
### Baseline metrics:

🎯 Baseline accuracy: 0.390  (evaluated on 41/41 parseable responses)

#### Per-class metrics (baseline):

|               | precision | recall | f1-score | support |
| ------------- | --------- | ------ | -------- | ------- |
| analytical    | 0.26      | 0.75   | 0.39     | 8       |
| personal      | 0.43      | 0.25   | 0.32     | 12      |
| summary_heavy | 0.00      | 0.00   | 0.00     | 10      |
| reactive      | 0.88      | 0.64   | 0.74     | 11      |
| accuracy      |           |        | 0.39     | 41      |
| macro avg     | 0.39      | 0.41   | 0.36     | 41      |
| weighted avg  | 0.41      | 0.39   | 0.37     | 41      |

### After fine-tuning: 

🎯 Fine-tuned model accuracy: 0.366

#### Per-class metrics (fine-tuned model):
|               | precision | recall | f1-score | support |
| ------------- | --------- | ------ | -------- | ------- |
| analytical    | 0.00      | 0.00   | 0.00     | 8       |
| personal      | 0.32      | 1.00   | 0.48     | 12      |
| summary_heavy | 1.00      | 0.20   | 0.33     | 10      |
| reactive      | 1.00      | 0.09   | 0.17     | 11      |
| accuracy      |           |        | 0.37     | 41      |
| macro avg     | 0.58      | 0.32   | 0.24     | 41      |
| weighted avg  | 0.60      | 0.37   | 0.27     | 41      |


The fine-tuned model using initial settings was less accurate than the baseline. After updating the hyperparameters, the model is now much more accurate. 

🎯 Fine-tuned model accuracy: 0.585

#### Per-class metrics (fine-tuned model):
|               | precision | recall | f1-score | support |
| ------------- | --------- | ------ | -------- | ------- |
| analytical    | 0.25      | 0.12   | 0.17     | 8       |
| personal      | 0.50      | 0.58   | 0.54     | 12      |
| summary_heavy | 0.62      | 0.80   | 0.70     | 10      |
| reactive      | 0.80      | 0.73   | 0.76     | 11      |
| accuracy      |           |        | 0.59     | 41      |
| macro avg     | 0.53      | 0.56   | 0.54     | 41      |
| weighted avg  | 0.56      | 0.59   | 0.56     | 41      |
##### Fine-Tuned Model -- Confusion Matrix (Test Set)

|             | Predicted Label: | Analytical | Personal | Summary-heavy | Reactive |
| ----------- | ---------------- | ---------- | -------- | ------------- | -------- |
| True Label: | Analytical       | 1          | 3        | 4             | 0        |
|             | Personal         | 2          | 7        | 1             | 2        |
|             | Summary-heavy    | 0          | 2        | 8             | 0        |
|             | Reactive         | 1          | 2        | 0             | 8        |


![[confusion_matrix.png]]




### Sample Classifications:

| Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Predicted Label (with Confidence Score) | Actual Label     | Why Prediction is Reasonable                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| This was everything my bookish heart could have wished for!  <br>  <br>Rebecca Romney structured this book brilliantly! It has the perfect amount of literary history, informative insights, interesting commentary, and personal reflections.  <br>She beautifully wove together the life stories of these trailblazing women, their impact on the literary landscape (and the world at large), how they inspired Jane Austen, and how reading their work changed both Austen’s and Romney’s lives.  <br>As a result, I’m now extremely eager to read each book she mentioned and see how they might change my life as well.  <br>  <br>It was also fascinating to hear about Rebecca Romney’s work as a rare book dealer. I have spent countless hours researching and looking at antiquarian editions of my favorite classics, as well as learning about the history of book binding. Like Rebecca Romney, and I’m sure many others, I not only consider myself a reader but also a book *collector.* Being on an endless quest to discover rare and special editions is one of my life’s greatest joys!  <br>  <br>Thank you Rebecca Romney for giving me an unforgettable tour of Jane Austen’s bookshelf!                                                                                                                                                                                                                                                                                                                                                   | Personal (40%)                          | Analytical (31%) | This is mostly about the author, her job and how she structured the book.                                                                                            |
| Always enjoy Freida McFadden’s books, and The Tenant was no exception! She has a way of pulling you in with suspense, unreliable characters, and just the right pacing to keep you flipping pages late into the night. This one had her signature twist—just when I thought I had things figured out, the story shifted and caught me off guard.  <br>  <br>The setting adds to the tension, and as the layers unfold, you start questioning everyone’s motives. It’s fast, engaging, and the kind of thriller that makes you second-guess what you think you know. Not my absolute favorite of hers, but still a solid, twisty ride. Another fun and thrilling read from McFadden!                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Personal (46%)                          | Analytical (21%) | This is mostly about the author's style and setting. There is a mention of her other books.                                                                          |
| Okay, this book? Chef’s kiss. Carley Fortune just gets it, those messy, beautiful, heart-squeezing feelings that come with love, friendship, and figuring yourself out. One Golden Summer felt like sinking into a warm lake at sunset. It’s nostalgic, romantic, and full of that bittersweet magic that makes you want to hug the book when you’re done.  <br>  <br>The story gave me all the summer vibes: sun-soaked days, late-night talks, and that one person who makes everything feel electric. I loved how real the characters felt. They’re not perfect, and that’s what made me root for them even harder. Fortune’s writing is so easy to fall into. It’s like she’s telling you a story over iced coffee, and you’re hanging on every word.  <br>I laughed, I got a little misty-eyed, and I didn’t want it to end. It’s the kind of book that makes you want to text your best friend and say, “You have to read this.”                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Personal (48%)                          | Reactive (25%)   | I don't think the opinion is strong enough to be reactive.                                                                                                           |
| This beautifully written book is why I love to read. Sybil Van Antwerp's past and present, encompassing her 78 years, are revealed in letters sent from and to her. She writes to family members, friends, authors of books she loves, a boy experiencing difficult times and anywhere she sees an injustice, including one to her garden club. She is compassionate, opinionated and, beneath her at-times prickly exterior, vulnerable.  <br>  <br>Sybil is a retired attorney with an impressive educational background. Divorced for many years with her ex-husband living in Belgium, she has a daughter and a son. Another son died tragically at the age of 8, which left her unmoored and led in part to the divorce. She and her brother were both adopted by loving parents, but much was missing until a random DNA search reveals secrets from the past. Her letters also lead to a romance she didn't know was possible.  <br>  <br>This is a book that touched my heart and Sybil is a character I will fondly remember. Her references to books, notably Crossing to Safety, resonated with me. I was sometimes reminded of another noteworthy character, Olive Kitteridge. It is more than worthy of a rare 5-star rating.                                                                                                                                                                                                                                                                                                                        | Summary_heavy (43%)                     | Personal (25%)   | I can see why it was put as summary_heavy. I labeled it personal since the main point seems to be that the book touched the reviewer's heart and resonated with her. |
| I have a like/love relationship with Fredrick Backman’s books. Some I like, some I like a lot, some I love, and some just blow me out of the water. “My Friends” has me catapulting up and out of the sea. It is rare that I drop everything to review a book shortly after finishing it but this book is that good that everyone should know and be able to get a copy.<br><br>His characters are amazing, damaged, big of heart, genius, sensitive, creative, intelligent beyond their years and often hilarious. Each is perfectly drawn. They are capable of plotting perfect plans that unravel because the plotting comes from a brain that is in equal measures genius and non-genius. They are so fortunate that they can run really fast. Their adventures defy everything, absolutely everything. They are carried by their belief in each other. They are everything to each other. They became everything to me. How did Backman create this extraordinary story that grabs at you from every direction?<br><br>I laughed really hard but I hurt equally because that is the perfect balance that Backman has created. He takes so much with anger, violence and deprivation but gives so much back with insight and humor and each share space on so many pages. Excellent, brilliant, extraordinary writing with dialog that captures emotions that can rip you to shreds one minute and have you choking with laughter a second later. This is not just a 2025 favorite but an all time favorite deserving of all the stars, accolades and bravos. | Analytical (43%)                        | Analytical       | Focuses mainly on the author's characters and style.                                                                                                                 |
| This could’ve been an email                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Reactive (49%)                          | Reactive         | Strong opinion. Does not provide any supporting details or personal experience.                                                                                      |
| Stephen King delivers another sharp, character driven thriller with Never Flinch, and I really enjoyed it. It’s a slower burn than some of his earlier work, but it’s packed with tension, moral questions, and that unmistakable King knack for creeping under your skin without resorting to ghosts or gore.  <br>  <br>This time, Holly Gibney is pulled into two unsettling cases, a chilling letter promising a series of murders and a high profile feminist speaker who’s being stalked. The storylines run side by side and gradually intertwine in a way that’s both satisfying and a little unsettling. Holly continues to be one of King’s most relatable and endearing characters, quirky, determined, and deeply human.  <br>  <br>It’s definitely more crime fiction than horror, but it still has that signature King unease simmering in the background. A few sections felt a bit meandering, but the writing is confident, and the emotional payoff is worth it. King’s take on guilt, revenge, and justice feels timely and surprisingly introspective.  <br>  <br>Overall, a solid 4 star read for me. Smart, suspenseful, and full of heart. Not King at his absolute peak, but proof that even when he’s in a quieter mode, he’s still one of the best storytellers around.  <br>  <br>I Highly Recommend                                                                                                                                                                                                                                   | Analytical (37%)                        | Analytical       | Accurate. The review compares the work to previous works of the author. Additionally, the reviewer critiques the author's style, plot and character.                 |

### Analysis:

Wrong predictions: 17 / 41

- **Personal vs Analytical confusion:**  Several reviews that mix personal reactions with analytical observations are misclassified in both directions. 
	- "Look, no one is more surprised than me at this rating. I am an avid fan of Fredrik Backman's work. I even read his Instagram captions and chuckle occasionally. All the same, I have ambivalent feelings..."
		- Labeled Personal, predicted Analytical. The review starts with a personal stance ("no one is more surprised than me") but then shifts into an evaluative mode ("ambivalent feelings"). The model seems to be overly sensitive to any analytical language, even when it's framed by personal perspective.
	- "Always enjoy Freida McFadden's books, and The Tenant was no exception! She has a way of pulling you in with suspense, unreliable characters, and just the right pacing to keep you flipping pages late i..."
		- Labeled Analytical, predicted Personal. The opposite problem: a primarily analytical review with a brief personal opener ("Always enjoy...") is misclassified as Personal. To improve this, the model needs a better understanding of the _overall focus_ of a review, not just the presence of certain phrases. If the majority of the content is personal reflection, it should be labeled Personal even if it contains some analytical observations, and vice versa.
- **Summary-heavy vs Analytical confusion:**  Reviews that make analytical claims supported by plot summary are often misclassified as purely Summary-heavy. 
	- "Horror hits differently when it's built on the bones of real, genocidal history. The blood, viscera, and supernatural vampire horror is plentiful in The Buffalo Hunter Hunter but it pales in compariso..."
		- Labeled Analytical, predicted Summary-heavy. There's a clear analytical claim here (that the real history is more horrific than the fictional elements) but because it's intertwined with plot details, the model seems to overweight the summary aspect. The model needs to be better at identifying argumentative structure even when it's scaffolded by narrative recap. One heuristic to try: if removing the plot details leaves behind a coherent analytical claim, the review is likely Analytical even if it contains substantial summary.
- **Reactive vs Personal confusion for emotionally intense reviews:**  Highly emotional reviews are sometimes misclassified as Reactive even when they contain Personal grounding. For example:
    - "This is very detailed at points, excruciatingly so. If you are a survivor, this could trigger you. It didn't me…I found comfort in recognizing how own my trauma has had a lasting effect on my marriage..."
	    - Labeled Personal, predicted Reactive. The intense emotional language ("excruciatingly", "trigger") seems to be overpowering the personal details ("my own trauma", "my marriage") in the model's weighting. To fix this, the model needs to place more importance on _specific_ personal anchors, even if they're mentioned briefly. A single reference to "my marriage" should be enough to make a review Personal, no matter how emotionally charged the rest of the language is. The presence of biographical specifics should trump intensity in the Personal vs Reactive decision.
### Results Comparison

| Model                     | Accuracy |
| ------------------------- | -------- |
| Zero-shot baseline (Groq) | 0.390    |
| Fine-tuned DistilBERT     | 0.585    |

Fine-tuning improvement: 0.195


### Confidence Calibration:
Confidence 30-40%: Model is accurate 33.3% of the time (12 examples)
Confidence 40-50%: Model is accurate 61.9% of the time (21 examples)
Confidence 50-60%: Model is accurate 83.3% of the time (6 examples)
Confidence 60-70%: Model is accurate 100.0% of the time (2 examples)


## Deployed Interface

The deployed interface can be run directly out of the Google Colab file `ai201_project3_takemeter_starter_clean.ipynb` by running the cells in the Interface section after running the rest of the notebook. 

First, run the !pip install cell. Next, update the MODEL_DIR value to where the tokenizer exists. In my demo, it is in the `./takemeter-model/checkpoint-72` folder. Next, run the last cell. This will not only show the interface in the cell, but will also give a public URL (gradio.live URL). 


## AI Tool Usage

This project uses AI assistance at several stages:

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