# ai201-project3-takemeter

### TakeMeter: a fine-tuned text classifier that evaluates discourse quality in an online community

---

### Community

I went with [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year).
This is an active, text-heavy subreddit that varies a lot in quality. Posts and comments include a variety of opinions,
arguments, and reactions.

### Labels

I decided to work with 3 different labels:

- **Opinion**: expressing a view on something without justification, e.g., Cats are better than dogs.
- **Argument**: expressing a view WITH reasoning aimed at convincing, even if the reasoning is personal or subjective, 
e.g., Working on-site is better because you get to socialize with your coworkers.
- **Reaction**: expressing how they feel or what they think about a post or comment, e.g., This is the best comment
  ever. Thank you.

I believe these distinctions matter because they let us understand the true intent of the post / comment. For example,
a post of someone sharing their opinion will lead to a discussion that's way different to a post of someone
trying to convince you of something, so this is why understanding the intent is important.

### Examples of Labels

#### Opinion

Posts / comments that clearly belong to this label:
> "Hollandaise sauce is a pain in the arse to make at home."

> "A shower in morning and one in evening. Everyday. That’s heaven"

Post / comment I'm not certain about:
> "You guys are delusional for thinking this is the last Rockstar game, or that GTA VI will not live to it's hype."

This comment feels very reactive, but at the same time I feel like the user is just expressing what they think about
something.

#### Argument

Posts / comments that clearly belong to this label:
> "Having fewer choices makes people happier. People act like more choices always means more freedom. I think having too
> many options just makes people overthink everything and be less satisfied with what they pick."

> "Going to university isn't worth it anymore. Especially in the US or other western countries, the job market for
> basically professional field is insanely oversaturated. Getting a degree isn't worth it, sure in the past it may have
> paid off but nowadays it doesn't. Good jobs are hard to come by and they only go to people who have experience. The ROI
> for college just isn't there."

Post / comment I'm not certain about:
> "Uncooked smoores are better than cooked smoores. It’s less messy, cooked marshmallows are usually burnt in the fire,
> a bite of solid chocolate is more satisfying then melted and overall I find it tastes better and is a more enjoyable
> experience. That is all."

This post feels to be opinion-heavy. As the user is just explaining why they think uncooked smores are better than
cooked
ones. At the same time, the post contains several reasons on why they think something is better, as if they were trying
to convince someone.

#### Reaction

Posts / comments that clearly belong to this label:
> "Terrible take"

> "I’m sorry but honestly you appear too unintelligent for me to even want to rebuke you. Just so many things wrong or
> misinformed. Reading your post gives me a headache."

Post / comment I'm not certain about:
> "This isn’t just an unpopular opinion but a poor one as well"

I feel like I could label this as an opinion, as the user is sharing what they think about a post. But at the end,
it feels too reactive to be an opinion.

### Hard edge cases

The type of posts I expect to be ambiguous between two labels are those where the user is sharing their opinion on one
topic, but at the same time they include reasoning on why they think this way. These kind of posts will sit in between
"opinion" and "argument".

Another case I expect to be ambiguous, are comments that sit in between "reaction" and "opinion". Reactions are easy to
distinguish: "I love this comment", "Thank you for this post", etc. But when they decide to add more, they can make
instantly make it go from a reaction to an opinion, e.g., "I love this comment. I think cats are better than dogs
because x, y, z."

When encountering ambiguous posts or comments, I'll analyze what is the user main goal. For example, "I love this
comment.
I think cats are better than dogs because x, y, z." starts with a reaction (I love this comment), is then followed
by an opinion (I think cats are better than dogs), but it also contains reasoning (because x, y, z), and in this case
I strongly believe the ultimate goal of the user is to try to convince others that cats are better than dogs, so that
makes it an argument.

TLDR; user intent will determine the label when handling ambiguous posts / comment.

### Data collection

All my data comes from posts and comments
from [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year).
Posts were sorted by top, then I went through each post. For each post, the actual body almost always fell in the 
argument category, as the user was expressing their view on something WITH reasoning behind it, so there was
an intent to convince others.

My final dataset was composed of exactly 201 examples. This allowed me to have a balanced dataset of 67 examples per label.

### Ambiguous cases
This is a list of 3 specific cases I found ambiguous:

1. > "Yellow is one of the best colors for a car. Yellow is such an underappreciated and underutilized color. Think
   about it. Yellow is a very visible color, it's associated with happiness. A nice, pale yellow looks awesome on a
   classic car and the bumblebee yellow looks awesome on a more modern sports car. I'm not trying to drive the typical
   cherry red sports car, or the cliche sleek black suburban. Nah, give me a yellow car over any other color any day."
    * This is a comment that feels personal and subjective, but it still provides reasoning to justify a conclusion.
      Therefore, I went with `argument` instead of `opinion`.

2. > "I like when the tables are sticky at restaurants. The sticky feeling of tables in general is very soothing to me,
   mainly in restaurants. It gives me the feeling that someone had such a good time before me, that they spilled their
   drink laughing. That's how I look at it."
    * This is another case of a comment that feels personal and subjective, but it still provides reasoning to justify a
      conclusion. Therefore, I went with `argument` instead of `opinion`.

3. > "You very well could be heat-sensitive. I am. 75 degrees is too hot to me, let alone 90. I prefer the upper 30s."
    * Ambiguous opinion/reaction. Chose opinion bc at the end they were just expressing what feels hot for them.

---

### Baseline

These are my baseline results:

🎯 Baseline accuracy: 0.806  (evaluated on 31/31 parseable responses)

Per-class metrics (baseline):

|              | precision | recall | f1-score | support |
|--------------|:---------:|:------:|:--------:|:-------:|
| opinion      |   0.73    |  0.80  |   0.76   |   10    |
| argument     |   1.00    |  0.90  |   0.95   |   10    |
| reaction     |   0.73    |  0.73  |   0.73   |   11    |
|              |           |        |          |         |
| accuracy     |           |        |   0.81   |   31    |
| macro avg    |   0.82    |  0.81  |   0.81   |   31    |
| weighted avg |   0.82    |  0.81  |   0.81   |   31    |

For my baseline I used `llama-3.3-70b-versatile` via Groq with the following prompt:

>You are classifying posts and comments from the r/unpopularopinion subreddit.
Assign each post/comment to exactly one of the following categories.
>* opinion: expressing a view without any justification
>  * Example: "Hollandaise sauce is a pain in the arse to make at home."
>
>* argument: expressing a view WITH reasoning aimed at convincing, even if the reasoning is personal or subjective.
>  * Example: "Going to university isn't worth it anymore. Especially in the US or other western countries, the job market for basically professional field is insanely oversaturated. Getting a degree isn't worth it, sure in the past it may have paid off but nowadays it doesn't. Good jobs are hard to come by and they only go to people who have experience. The ROI for college just isn't there."
>
>* reaction: expressing how they feel or what they think about a post or comment
>  * Example: "I’m sorry but honestly you appear too unintelligent for me to even want to rebuke you.  Just so many things wrong or misinformed.  Reading your post gives me a headache."
>
>Respond with ONLY the label name.
>Do not explain your reasoning.
>
>Valid labels:
>* opinion
>* argument
>* reaction

---
### Fine-tuning
For fine-tuning I went with the default `distilbert-base-uncased`.

#### Hyperparameters
These were my hyperparameters:
- `num_train_epochs`: 3 (the default value)
- `learning_rate`: 2e-5 (the default value)
- `per_device_train_batch_size`: 8 

I chose to use a value of 8 for `per_device_train_batch_size` to have a higher number of recalibrations given that my 
dataset is relatively small (201 records before splitting). 

---
### Evaluation Report
### Confusion Matrix

| True Label | Predicted Opinion | Predicted Argument | Predicted Reaction |  Total |
|------------|------------------:|-------------------:|-------------------:|-------:|
| Opinion    |                 4 |                  5 |                  1 |     10 |
| Argument   |                 0 |                 10 |                  0 |     10 |
| Reaction   |                 3 |                  0 |                  8 |     11 |
| **Total**  |             **7** |             **15** |              **9** | **31** |
The table above is based on the image below:
<img src="/data/confusion_matrix.png"/>

### Evaluation Results
```json
{
  "baseline_accuracy": 0.8065,
  "finetuned_accuracy": 0.7097,
  "improvement": -0.0968,
  "test_set_size": 31,
  "label_map": {
    "opinion": 0,
    "argument": 1,
    "reaction": 2
  },
  "model": "distilbert-base-uncased"
}
```
### Analysis on Wrong Predictions

Text: "A relationship won't last without love."

>True: **opinion**
> 
>Predicted: **reaction** (confidence: 0.34)

*Analysis:* this, to me, is a clear example of an opinion. The user is sharing their view on relationships without love.
I don't really have a theory on why the model guessed reaction, based on the low confidence score, I assume the model
simply guessed. To solve this, I'd probably add more examples for reaction.

---

Text: "Bike commuting and walking are peaceful and I love getting to be outside and about during those times. Driving, however, is hell."
>True: **opinion**
> 
>Predicted: **argument** (confidence: 0.36)

*Analysis:* this, to me, is a clear example of an opinion. The user is sharing their view on bike commuting, walking, 
and driving. However, since the user is comparing driving to bike / walking, this could be seen as an argument.
My hypothesis is that the simple comparison ("Driving, however, is hell") was seen by the model as reasoning. To solve 
this I'd probably add more diverse examples to train the model better on ambiguous situations.

---

Text: "I love posts like this because it shows how many people out there are absolutely butthurt about the stupidest stuff"
>True: **reaction**
> 
>Predicted: **opinion** (confidence: 0.37)

*Analysis:* this can be considered an ambiguous case. Yes, "I love posts like this" is a reaction from the user, but they
also added more text which can be seen as an opinion on people. My hypothesis is that the model simply ran into an 
ambiguous case, and defaulted to **opinion**. To solve this, I would do the same as with the first example, have more
examples for reaction.

---
### Sample Classifications
| # | Text                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | True     | Predicted | Confidence | Why Correctly Classified                                                                                                                                            |
|---|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-----------|-----------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | What did YOU say?! 😡                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | reaction | reaction  |     36.03% | this is a clear user reaction to another post / comment                                                                                                             |
| 2 | Rainy days are way better than sunny days. So many people associate rain with sadness yet all I associate it with is excitement and happiness. I love the way everything cools off, I love that I don't constantly have to squint cause the sun is in my eyes, I love the smell, and I love the colors, they're so much easier on the eyes. A rainy day is such a special day to me, seems like the perfect type of day for an adventure. I love to go out in the rain and walk around, maybe have lunch, go see a movie, or go hiking. | argument | argument  |     40.69% | the intent behind the comment is clear. the user has a preference for rainy days over sunny days, and they provide an explanation on WHY rainy is better than sunny |
| 3 | Cemeteries are a waste of space and everyone should be cremated or composted after death. It doesn't make sense to me why we have so many cemeteries, some of which are huge. We could be using that land for homes, businesses, agriculture, etc. Even if a person wants to be buried, there's no need to pour cement into the ground or to bury the body in a casket that won't decompose after a few years.                                                                                                                          | argument | argument  |     38.93% | again, the intent behind the comment is clear. the goal of the user is to convince people that cemeteries are a waste of space.                                     |

---
### What the model captured vs. What I intended it to capture
I will start with what I intended the model to capture: communicative intent. I framed this project around 
identifying user intent: opinion → expressing a view, argument → trying to convince w/ reasoning, reaction → responding to something.

The model failed at capturing this. When you think about the difference between opinions and arguments, sometimes they 
can look very similar. Arguments that have subjective / personal reasoning could be mistaken for opinions. Reactions
can be ambiguous if no context is provided. 

Based on the results, it looks like the model was acting in the following way:
- assumed argument if text contains keywords such as 'should', 'because', or 'however'.
- first-person language / expressions such as 'I love', 'I hate' were mistaken as opinion
- short, punchy examples → reaction

And sometimes it might be correct, a lot of reaction were short and punchy, but to correctly label something you
have to understand the communicative intent.

---
### Spec reflection
#### One way the spec helped guide your implementation
The spec helped me during the data collection process. At first, it wasn't really clear to me how I was going to 
approach the data collection process. But once I thought about and wrote it down it was clear: instead of sourcing 200 
comments from the same reddit post, I went through multiple comments to make sure my examples were diverse.

#### One way your implementation diverged from it 
My label implementation diverged slightly from my spec. Once I started gathering data, I quickly realized my label 
description was too loose and I was running in several ambiguous cases. After tightening the definition of my labels,
gathering examples got slightly easier. In my opinion, it also helped have a stronger classification prompt.

---
### AI Usage
#### Annotation assistance

- *What I gave the AI:*<br>I gave Claude (Sonnet 4.6) a set of unlabeled raw examples (exactly 100 records) and asked it
to: label them while keeping track of which cases were considered difficult.

- *What it produced:*<br>A batch of 100 labeled examples, including a marker for those considered difficult cases.

- *What I did with the output:*<br>I reviewed the examples to make sure they were labeled correctly. After reviewing
them I determined that the labels were applied correctly, and the explanation for cases considered difficult was clear.

#### Failure analysis

- *What I gave the AI:*<br>I gave Claude (Sonnet 4.6) a list of wrong predictions and asked it to identify
  patterns.

- *What it produced:*<br>A list of two clear examples: opinions that contain some sort of reasoning/evidence are being
mistaken for arguments, and reactions being mistaken for opinions because of a lack of context. 

- *What I did with the output:*<br>I used to elaborate on my fine-tuning evaluation report.