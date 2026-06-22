# Project 3 Planning: TakeMeter

---

## Milestone 1: Choose Your Community and Define Your Labels

I went with [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year). 
This is an active, text-heavy subreddit that varies a lot in quality. Posts and comments include a variety of opinions,
arguments, and reactions. 

#### Labels
I decided to work with 3 different labels:
- Opinion: someone's opinion on a specific topic, e.g., Cats are better than dogs.
- Argument: someone trying to convince you of something, e.g., Working on-site is better because you get to socialize with your coworkers.
- Reaction: someone expressing how they feel or what they think about a post or comment, e.g., This is the best comment ever. Thank you.

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
> "Having fewer choices makes people happier. People act like more choices always means more freedom. I think having too many options just makes people overthink everything and be less satisfied with what they pick."

> "Going to university isn't worth it anymore. Especially in the US or other western countries, the job market for basically professional field is insanely oversaturated. Getting a degree isn't worth it, sure in the past it may have paid off but nowadays it doesn't. Good jobs are hard to come by and they only go to people who have experience. The ROI for college just isn't there."

Post / comment I'm not certain about:
> "Uncooked smoores are better than cooked smoores. It’s less messy, cooked marshmallows are usually burnt in the fire, a bite of solid chocolate is more satisfying then melted and overall I find it tastes better and is a more enjoyable experience. That is all."

This post feels to be opinion-heavy. As the user is just explaining why they think uncooked smores are better than cooked
ones. At the same time, the post contains several reasons on why they think something is better, as if they were trying
to convince someone.

#### Reaction
Posts / comments that clearly belong to this label:
> "Terrible take"

> "I’m sorry but honestly you appear too unintelligent for me to even want to rebuke you.  Just so many things wrong or misinformed.  Reading your post gives me a headache."

Post / comment I'm not certain about:
> "This isn’t just an unpopular opinion but a poor one as well"

I feel like I could label this as an opinion, as the user is sharing what they think about a post. But at the end, 
it feels too reactive to be an opinion.

---

## Milestone 2: Project Spec

### Reflection
I believe [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year) is a good 
community for a classification as it contains a wide variety of discussions. The main idea of their community is for
people to share unpopular opinions. This alone is enough to generate different reactions from people. Different reactions
equals to interesting and varied comments. Some choose to reply with only a simple reaction, others share their opinion,
and others like to offer a strong counterargument. 

#### Hard edge cases
The type of posts I expect to be ambiguous between two labels are those where the user is sharing their opinion on one
topic, but at the same time they include reasoning on why they think this way. These kind of posts will sit in between
"opinion" and "argument".

Another case I expect to be ambiguous, are comments that sit in between "reaction" and "opinion". Reactions are easy to
distinguish: "I love this comment", "Thank you for this post", etc. But when they decide to add more, they can make 
instantly make it go from a reaction to an opinion, e.g., "I love this comment. I think cats are better than dogs 
because x, y, z."

When encountering ambiguous posts or comments, I'll analyze what is the user main goal. For example, "I love this comment. 
I think cats are better than dogs because x, y, z." starts with a reaction (I love this comment), is then followed
by an opinion (I think cats are better than dogs), but it also contains reasoning (because x, y, z), and in this case 
I strongly believe the ultimate goal of the user is to try to convince others that cats are better than dogs, so that 
makes it an argument. 

TLDR; user intent will determine the label when handling ambiguous posts / comment.

#### Data collection
All my data will come from posts and comments from [r/unpopularopinion](https://www.reddit.com/r/unpopularopinion/top/?screen_view_count=2&t=year).
I will sort by the top posts, then will go through each post and for each post try to get 3 to 5 comments, at least one 
comment per label.

To avoid underrepresented labels, I will save 201 posts / comments, which will allow me to have exactly 67 posts / comments
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
  1. >"Honestly, remote work just feels more productive to me. You avoid all the office distractions and save hours commuting — I don't get why companies are still pushing everyone back in."
      >  * **Opinion / Argument** 

  2. >"This is such a good point. Streaming services really have killed the experience of going to the movies together as a culture."
     >  * **Reaction / Opinion**

  3. >"I've never understood the hype around Marvel films. They're formulaic, the stakes never feel real, and the humor undercuts every dramatic moment."
     >  * **Opinion / Argument**

  4. >"Great write-up. Though I'd push back a bit — open-source models are catching up fast and the gap with proprietary ones is shrinking every quarter."
     >  * **Reaction / Argument**

  5. >"That actually makes me sad. Social media has genuinely rewired how young people communicate, and we just kind of accepted it without questioning it."
     >  * **Reaction / Opinion**

- *What I did the data:*<br>
These are the labels I used for each post:
  1. **Opinion**
  2. **Opinion**
  3. **Argument**
  4. **Argument**
  5. **Reaction**

#### Annotation assistance
- *What I will give the AI:*<br>I will give Claude (Sonnet 4.6) 20 examples per label, this is around 30% of the records
I plan to have per label, and ask it to label them.

- *What I expect it to produce:*<br>A batch of accurately labeled examples.

- *What will I do with the output:*<br>I will review the examples myself, and then I will keep track of them by
adding a column in my examples dataset that identifies them as labeled using Claude. This column will be ignored
when running the classification task to avoid any issues.

#### Failure analysis
- *What I will give the AI:*<br>I will give Claude (Sonnet 4.6) a list of wrong predictions and ask it to identify
patterns.

- *What will I do with the output:*<br>This will depend on the patterns that are brought up. For example, if the 
analysis detects that a specific label is being confused, lets say arguments are often mislabeled as reactions, I will 
analyze the mistakes and look for ambiguous comments. Maybe the error happens when a comment starts with a simple reaction
and the AI instantly labels it without reading the rest of the comment.
---
