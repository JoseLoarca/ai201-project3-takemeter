# Project 3 Planning: TakeMeter

---

## Milestone 1: Choose Your Community and Define Your Labels

I went with [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year).
This is an active, text-heavy subreddit that varies a lot in quality. Posts and comments include a variety of opinions,
arguments, and reactions.

#### Labels

I decided to work with 3 different labels:

- Opinion: expressing a view on something without justification, e.g., Cats are better than dogs.
- Argument: expressing a view WITH reasoning aimed at convincing, even if the reasoning is personal or subjective, 
e.g., Working on-site is better because you get to socialize with your coworkers.
- Reaction: expressing how they feel or what they think about a post or comment, e.g., This is the best comment
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

---

## Milestone 2: Project Spec

### Reflection

I believe [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year) is a good
community for a classification as it contains a wide variety of discussions. The main idea of their community is for
people to share unpopular opinions. This alone is enough to generate different reactions from people. Different
reactions equals to interesting and varied comments. Some choose to reply with only a simple reaction, others share their opinion,
and others like to offer a strong counterargument.

#### Hard edge cases

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

#### Data collection

All my data will come from posts and comments
from [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year).
I will sort by the top posts, then will go through each post and for each post try to get 3 to 5 comments, at least one
comment per label.

To avoid underrepresented labels, I will save 201 posts / comments, which will allow me to have exactly 67 posts /
comments
for each label.

#### Evaluation metrics

I will use overall accuracy and per class accuracy. I think these metrics are ideal because this is a simple task
with just 3 labels, and a relatively small data set. Also, given that my dataset will be balanced, overall accuracy will
act as good feedback, but the per class accuracy is what's going to help me identify if any of my classes have
weaknesses.

#### Definition of success

My current definition of success is an 80% per class accuracy score. Realistically, there are no real consequences if
a post / comment is mislabeled. And given Reddit's nature, a lot of content could potentially be ambiguous, so I
can't expect a score of 100%.

### AI Tool Plan

#### Label stress-testing

- *What I gave the AI:*<br>I gave Claude (Sonnet 4.6) my label descriptions and my hard edge cases section from my
  planning.md, and asked it to generate 5 comments that sit at the boundary of two labels.

- *What it produced:*<br>
    1. > "Honestly, remote work just feels more productive to me. You avoid all the office distractions and save hours
       commuting — I don't get why companies are still pushing everyone back in."
       >  * **Opinion / Argument**

    2. > "This is such a good point. Streaming services really have killed the experience of going to the movies
       together as a culture."
       >  * **Reaction / Opinion**

    3. > "I've never understood the hype around Marvel films. They're formulaic, the stakes never feel real, and the
       humor undercuts every dramatic moment."
       >  * **Opinion / Argument**

    4. > "Great write-up. Though I'd push back a bit — open-source models are catching up fast and the gap with
       proprietary ones is shrinking every quarter."
       >  * **Reaction / Argument**

    5. > "That actually makes me sad. Social media has genuinely rewired how young people communicate, and we just kind
       of accepted it without questioning it."
       >  * **Reaction / Opinion**

- *What I did the data:*<br>
  These are the labels I used for each post:
    1. **Opinion**
    2. **Opinion**
    3. **Argument**
    4. **Argument**
    5. **Reaction**

#### Annotation assistance

- *What I will give the AI:*<br>I will give Claude (Sonnet 4.6) a set of unlabeled raw examples and ask it to label them
  based on the label descriptions and examples found in planning.md.

- *What I expect it to produce:*<br>A batch of accurately labeled examples.

- *What will I do with the output:*<br>I will review the examples myself, and then I will keep track of them by
  adding a column in my examples dataset that identifies them as labeled using Claude.
  This is an interesting experiment. Since I am the one who is collecting the example, I somehow have control over the
  content I choose to include. So when gathering the first 60 examples that will be labeled using AI, I will have a
  rough
  idea of how many examples per label I am gathering. This means that by the end, if the AI chooses a different label,
  I might have to adjust the definition of my labels.

#### Failure analysis

- *What I will give the AI:*<br>I will give Claude (Sonnet 4.6) a list of wrong predictions and ask it to identify
  patterns.

- *What will I do with the output:*<br>This will depend on the patterns that are brought up. For example, if the
  analysis detects that a specific label is being confused, lets say arguments are often mislabeled as reactions, I will
  analyze the mistakes and look for ambiguous comments. Maybe the error happens when a comment starts with a simple
  reaction
  and the AI instantly labels it without reading the rest of the comment.

---

## Milestone 3: Collect and Annotate Your Dataset

This was a painful process, I think I will quit Reddit after this. I was able to gather exactly 201 comments or posts
from r/unpopularopinion. I have a balanced dataset: exactly 67 examples per label.

There were several cases that felt ambiguous, most of the time they were cases that could be either opinion or argument,
here are 3 examples of ambiguous cases:

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

## Milestone 4: Run Your Baseline

**Baseline Results**

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

### Reflection
The label the baseline struggled the most with was "reaction". The model seems to be using "opinion" as the default
label when uncertain.

This could because without context, a comment that says "this made me irrationally angry" can be seen as someone's
view on something. Hypothesis: reactions are harder to identify without context.
---
## Milestone 5: Fine-Tune Your Model

### Wrong Predictions

Text: "A relationship won't last without love."

>True: **opinion**
> 
>Predicted: **reaction** (confidence: 0.34)

*Analysis:* this, to me, is a clear example of an opinion. The user is sharing their view on relationships without love.
I don't really have a theory on why the model guessed reaction, based on the low confidence score, I assume the model
simply guessed.

---

Text: "Bike commuting and walking are peaceful and I love getting to be outside and about during those times. Driving, however, is hell."
>True: **opinion**
> 
>Predicted: **argument** (confidence: 0.36)

*Analysis:* this, to me, is a clear example of an opinion. The user is sharing their view on bike commuting, walking, 
and driving. However, since the user is comparing driving to bike / walking, this could be seen as an argument.
My hypothesis is that the simple comparison ("Driving, however, is hell") was seen by the model as reasoning.

---

Text: "I love posts like this because it shows how many people out there are absolutely butthurt about the stupidest stuff"
>True: **reaction**
> 
>Predicted: **opinion** (confidence: 0.37)

*Analysis:* this can be considered an ambiguous case. Yes, "I love posts like this" is a reaction from the user, but they
also added more text which can be seen as an opinion on people. My hypothesis is that the model simply ran into an 
ambiguous case, and defaulted to **opinion**.

---
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

### Confusion Matrix

| True Label | Predicted Opinion | Predicted Argument | Predicted Reaction |  Total |
|------------|------------------:|-------------------:|-------------------:|-------:|
| Opinion    |                 4 |                  5 |                  1 |     10 |
| Argument   |                 0 |                 10 |                  0 |     10 |
| Reaction   |                 3 |                  0 |                  8 |     11 |
| **Total**  |             **7** |             **15** |              **9** | **31** |
The table above is based on the image below:
<img src="/data/confusion_matrix.png"/>
