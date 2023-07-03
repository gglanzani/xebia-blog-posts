---
author: Giovanni Lanzani
brand: xebia.com
categories:
- data-science-and-ai
- technology
date: 2023-06-27 00:00:00+02:00
email: giovanni.lanzani@xebia.com
focus-keywords: llms chatgpt bing llm test testing
guid: https://xebia.com/wp-json/wp/v2/posts/65904
image: images/banner.jpg
og:
  image: images/og-banner.jpg
slug: through-the-looking-glass-exploring-the-wonderland-of-testing-ai-systems
status: draft
subtitle: Testing AI systems is more necessary than ever. So why are few doing it
  right?
title: 'Through the Looking Glass: Exploring the Wonderland of Testing AI Systems'
---

<p style="font-size: 1.2em">Artificial Intelligence (AI) systems are becoming ubiquitous: from cars
to risk models to large language models (LLMs). As we depend more on
these systems, testing should be a top priority during deployment.</p>

<div style="font-size: 1.1em; font-style: italic;">Leaving software 
untested is like writing a cheque without verifying
the amount or recipient; you can do it right the first time, but you'd
rather be sure to avoid nasty surprises or even catastrophic results.
Take, for example, the code bug that bankrupted Knight Capital Group's
by losing USD 440M in less than an hour. AI systems are vulnerable in
the same way since they also contain code, but not only that: they need
data and algorithms. Testing one without the other is dangerous.

AI is the domain of data scientists who are not renowned for their
test-writing abilities—boring details compared to science. They should
be helped to do so in multidisciplinary teams, as AI systems are built
and maintained across departments.

To avoid slowdowns, testers need end-to-end ownership, focusing on
where the business need and risk is more acute. This approach ensures
precious buy-in. This article explains where to start and what is
required to avoid overdosing on chamomile to sleep at night.</div>

Artificial Intelligence (AI) systems are becoming ubiquitous: from cars
to risk models to large language models (LLMs). As we grow more
dependent on these systems, testing should be a top priority for
organizations and governments using them. Leaving them unchecked is akin
to writing a cheque without validating the amount or recipient. Checking
can be annoying, but making a mistake can be fatal.

Dutch parents with dual nationality know this well. An AI system trained
by the government to spot childcare benefits fraud ended up
discriminating against this parental group. The outcome for the families
discriminated against was tragic. Some had to sell their homes and
belongings. The Dutch cabinet ended up stepping down amidst the fallout
(although many Dutch citizens didn't learn: the same prime minister was
re-elected).

## Tests avoid surprises

To avoid surprises, AI systems should be tested by feeding them
real-world-like data. When a new system version is ready, the tests
ensure it still functions correctly. And testing software is essential
for governments and businesses alike. In 2012, Knight Capital Group lost
USD 460M (EUR 360M at the time) for a mistake in its trading software.
With no one able to spot the mistake, chaos quickly ensued: within one
hour, the company went bankrupt. To this day, the only thing left is a
Wikipedia entry. Hardly the legacy a group with an income of hundreds of
millions of dollars would aspire.

Why, then, isn't the practice of testing widespread? AI systems are
knottier than software systems; they are made of data, code, and
algorithms. The algorithm is a mathematical procedure that describes how
to go from data to output. For example, to know how much taxes we owe,
the tax office applies an algorithm to convert our gross income into a
lesser version of itself (the net income). The gap, how much we owe, is
the algorithm's output. The code is the algorithm's translation into
orders that a computer can understand and perform. In AI, there is
another ingredient: algorithms are tweaked based on the patterns in the
data. For example, if the patterns are biased, the outcome will also be
biased — as in the Dutch childcare benefits fraud case.

## Test the data, code, and algorithm.

To properly test an AI system, all three components — data, code, and
algorithm — need tests.

Let's start with data. Among the things that could go wrong, we have:

- Bias in the data — as a lack of representation for a minority
  group.
- We might interpret variables wrongly — is the "Amount" in USD or
  EUR?
- The underlying data might drift -- maybe a competitor enters the
  market, destroying the sales forecast accuracy. Catching these
  performance drifts early is underrated.

On the code side, we could have bugs that could be as simple as
switching revenue and costs when computing the profit.

And on the algorithm side, common issues include:

- Using an algorithm unfit for the data. Statistical properties of the
  data, such as average and median, might exclude some algorithm
  classes.
- Forgetting exploring if different parameters increase the model
  performance.

## A daunting prospect

Testing everything seems daunting, especially when considering the main
force behind AI systems: data scientists. Their role recently emerged.
As the name suggests, most come from academia, where software tests
aren't exactly fashionable, unlike publishing papers. And to
successfully publish, the work needs to be better than previous papers.
All attention goes, therefore, to the logic and methodology and not to
the correctness of the code. Testing becomes an irrelevant nuisance. In
the industry, however, the main goal is not performance but the value
the model generates. An AI system 5% less accurate than the competition
is still useful; companies make up for the difference elsewhere. But to
avoid surprises, as what happened to Knight Capital Group, tests are
needed.

And it's not just data scientists that should test. As AI systems span
across departments, multiple roles contribute to proper testing:
Software and platform engineers for the code and the platform it runs on
and data engineers (a particular type of engineer specialized in
data-intensive applications) for the data. Unfortunately, these profiles
don't always understand each other. If data scientists struggle to grasp
why you should test, engineers often fail to realize how AI systems
differ from pure software systems and how to deal with them. And
everyone should be aware that testing data is a challenge that is only
becoming more important, especially considering just how much data we
produce (see Figure 1). Companies are starting to invest in data
literacy programs to alleviate the ensuing babel and ensure mutual
understanding when talking *data & AI*.

<figure>
  <img src="https://xebia.com/wp-content/uploads/2023/07/graph_data_worldwide_usage.png">
  <figcaption>Figure 1</figcaption>
</figure>

## Testing end-to-end

An AI system usually touches multiple components: Data resides in
databases, the code and algorithm in central repositories (specialized
databases), and the knowledge of the data and processes around it with
business stakeholders. And not from a single department: sales data
might be used in an AI system used by the finance department to allocate
marketing spending better.

These octopus-like systems, though, are more efficient when a single
team tests them end-to-end without external blockers. To achieve this,
internal barriers and the silo mentality should be broken. Cynics will
argue that keeping a snowman in the desert from melting is easier, but
ignoring the challenge means burying our heads in those desert sands. A
top-down approach might prove more practical: if the executives sketch
the plan, the organization will break those rigid barriers and be more
fluid— hopefully unlike our snowman.

## Win the skeptics

Changing the organization to ensure AI systems are safe seems daunting.
But just like an apprentice is slowly initiated into their craft,
companies do not need to change everything at once. They can start where
the impact is higher and the risk smaller and iterate. Some components
might initially be tested manually or with other departments — at the
cost of speed. As the organization's trust increases, take an extra
step. Automate a manual process away or increase the ownership of the
team. If the AI system is vital for a department, make them your
partner. Being open and explaining why testing is necessary (you're
free to refer them to this article) will win many skeptics over. Tell
them what could go wrong if things go south. And if all else fails, ask
them whether they'll be the ones to bankrupt the company because of
their stubbornness.

## What about LLMs?

If reading the above, you thought testing AI systems was complex, we
have bad news. Systems built on top of LLMs are even more challenging
as:

- Natural Language Processing is ambiguous, and since LLMs do not
  understand what we tell them, reproducibility doesn't come out of
  the box.
- Backward compatibility is not guaranteed; you might need extra work
  to ensure things don't break in production. For example, if the
  developers behind the LLM you're leveraging add some checks to
  refuse to answer a class of questions, your system might break if
  you don't catch the change early.

Even Microsoft, today the uncontested leader in the space thanks to the
partnership with OpenAI, was
[bitten](https://answers.microsoft.com/en-us/bing/forum/all/this-ai-chatbot-sidney-is-misbehaving/e3d6a29f-06c9-441c-bc7d-51a68e856761)
[by](https://www.theverge.com/2023/2/15/23599072/microsoft-ai-bing-personality-conversations-spy-employees-webcams)
[unwanted](https://www.businessinsider.com/bing-chatgpt-ai-chatbot-argues-angry-responses-falls-in-love-2023-2)
[behavior](https://arstechnica.com/information-technology/2023/02/microsoft-lobotomized-ai-powered-bing-chat-and-its-fans-arent-happy/)
in their Bing AI chatbot shortly after its launch. They learned their
lessons, though, and introduced the proper checks and
balances—tests!---to ensure new versions wouldn't exhibit the same
quirks.

Some good news? A [company](https://xebia.ai/data) out there seems to
have the right skills to help you, skills acquired through building
LLM-based products to enhance protein engineering, to automate the
process of extracting ESG data from company quarterly reports, to
respond to health queries by reading thousands of medication package
inserts, and more.
