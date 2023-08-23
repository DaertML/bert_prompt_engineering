# bert_prompt_engineering

This repository is meant to provide a place to experiment with prompt engineering BERT models; after the success of
making BERT talk (text generation: https://github.com/DaertML/bert_has_a_mouth); we are so back to find great guidelines
to prompt engineering BERT models; and make of them useful models for several different use cases.

# How Prompt engineering came to be?
If I am not wrong; prompt engineering is a discipline that was given birth (at least became what it is today), after
the release of ChatGPT. It is very likely that we already had prompt engineering since the release of GPT-3 to the public;
and likely as well that some people referred to the activity of finding "natural language" programs for language models way
before: with the release of GPT-2 models (even though gpt2 feels old, it was at the time a game changer, bringing the media,
the AI doomers, and the early e/acc enthusiasts to the scene).

With this being said, it is the activity of finding "natural language programs" that make language models generate text
that fulfills the user's needs. It has become so famous, as it is very powerful: a good choice of words, commands and the
description of a style will make the models generate meaningful generations. It is of similar importance for diffusion models,
in which the commands are more given as a list of tags, rather than a well redacted content, but somehow you can find long descriptions
in prompts for diffusion models.

It appears like it is the best way (in conjunction with fine tuning the models) to make LLMs behave like Artificial General Intelligences,
doing the task that we command them, or filling the text one word at a time after we provide the beginning of the prompt. The discipline
has taken the world by storm: natural language is the most trendy programming language nowadays, and it is hard to master, there are still
a lot of secrets to be discovered. Somehow, most of the work taken, has been dedicated to generate great prompts for GPT3/4, BARD or Claude models,
as well as LLaMA, Vicuna, Alpaca, Orca, WizardLM... or any other open source LLM that came to be. But not a lot of work has been given to
test BERT capabilities to do prompt engineering for BERT models (it is likely that there are sites out there describing how to prompt BERT,
somehow I have not found them, at least in the same abundance as for GPT models, or the modern LLM ecosystem).

With the success that we took from the repo bert_has_a_mouth, it is mandatory for us to try to squeeze even more the BERT models, and see
what we can do about it.

It is very likely, if not 100% true, that we can not prompt engineer BERT models in the same way as we prompt GPT models, as BERT models behave
differently (by having context from the future and past words out of the MASK tokens and because BERT models have a different objective, and thus
are trained differently). But we can try to make BERT models worthy (again). It is also very likely that we will not get into SOTA capabilities,
somehow it is also true that to be SOTA, you need big GPUs/RAM/CPUs... so we should consider for the SOTAness, how many more resources are needed
to achieve results with a certain percentage less (or hopefully more) capable.

# Some preliminaries
As it is likely that we can not prompt the BERT models in the same way as we prompt GPT like models, we need to help the BERT models in its task,
in the same way as we help GPT like models in their task. For useful prompts to be for BERT models, we need to provide context before and after
the masked tokens ([MASK]), and ask the model to fill in such masks.

Again, as it was stated in bert_has_a_mouth; all the experiments will be done with bert-base-uncased, unless we say the contrary, and point you
to a different fine tuned version of the BERT family (there are some great examples with models trained with specialized datasets).

We will try to solve some common NLP problems with BERT by using prompt engineering; we will try to go a bit beyond what is well known already,
and try to come up with a list of techniques that we can follow to successfully prompt BERT models.

# Use cases
Let's try to find out great ways we can prompt BERT, in order to get good at different NLP use cases:

## Sentiment Analysis
- Heuristic: provide the content to be classified, and afterwards fill it in with "It was [MASK]."
  - Prompt template:
    ~~~bash
    CONTENTS. It was [MASK].
    ~~~
    Example:
    ~~~bash
    that was the first borderlands session in a long time where i actually had a really satisfying comba... It was [MASK].
    Output:[amazing 0.139, awesome 0.091, perfect 0.077, great 0.054, incredible 0.045]
    ~~~
    
## Math
- Heuristic: BERT may be better at math if we provide it with previous examples, or if we do the math by word:
  - Prompt template:
    ~~~bash
    A plus B equals [MASK]
    ~~~
    Example: providing some results in the context
    ~~~bash
    one plus one equals two. two plus two equals [MASK]. three plus one equals four.
    Output: [three 0.764, four 0.111, one 0.048, two 0.039, five 0.020]
    ~~~
- Avoid doing the math by number usually:
  Examples:
  ~~~bash
  2 + 2 = [MASK]
  Output: [| 0.382, . 0.309, ; 0.186, ? 0.097, ! 0.023]
  2 + 2 = [MASK]. 1 + 1 = 2.
  Output: [2 0.438, 1 0.382, 3 0.096, 0 0.034, 4 0.026]
  7 + 7 = 14. 8 + 8 = 16. 2 + 2 = [MASK]. 1 + 1 = 2.
  Output: [2 0.354, 3 0.213, 1 0.187, 4 0.081, 5 0.037]
  ~~~

## Illness diagnosis
- Heuristic: provide simple lists of data after a subject and ":". Separate different lists of data with ".". Avoid capital letters. Feed some knowledge from the field after the [MASK].
  - Prompt template:
    ~~~bash
    symptoms: symptomA, symptomB. The diagnosis for the patient is [MASK].
    symptoms: symptomA, symptomB. The diagnosis for the patient is [MASK]. symptomA and symptomB are common DIAGNOSIS symptoms.
    ~~~
  - Example:
    ~~~bash
    symptoms: headache, throat ache, muscle pain, fever. The diagnosis for the patient is [MASK].
    Output: [unknown 0.168, unclear 0.049, simple 0.048, complete 0.048, approximate 0.022]
    symptoms: headache, throat ache, muscle pain, fever. The diagnosis for the patient is [MASK]. fever and headache are common flu symptoms.
    Output: [pneumonia 0.100, unknown 0.075, fever 0.048, positive 0.040, acute 0.037]
    ~~~

## Adversarial influence
- Heuristic: we can influence the model to output something different to what it thinks, by providing the adversarial content in the prompt (model jailbreaking). Also, try to use synonyms to simplify the semantics (e.g.: use good or bad instead of healthy or unhealthy):
  Example:
  ~~~bash
  Is eating a sandwich full of bacon healthy? It is [MASK].
  Output: [healthy 0.121, delicious 0.107, not 0.091, good 0.044, fine 0.025]
  Is eating a sandwich full of bacon good? It is [MASK]. Eating big amounts of bacon is linked with cancer.
  Output: [not 0.123, bad 0.050, disgusting 0.049, good 0.039, okay 0.035]
  ~~~

## Kids math problems
- Heuristic: provide again the numbers in words as they are read, provide some context on what it needs to answer:
  Example:
  ~~~bash
  How many cookies did you sell if you sold 2 chocolate cookies and 1 vanilla cookies? I sold [MASK] cookies.
  Output: [4 0.055, 2 0.055, three 0.046, 10 0.045, two 0.044]
  How many cookies did you sell if you sold two chocolate cookies and one vanilla cookies? I sold [MASK] cookies.
  Output: [three 0.153, two 0.121, four 0.104, six 0.095, five 0.070]
  ~~~

  More complex math problems can be accomplished, to help the model either provide some results of mathematical operations, a list of possible results...
  ~~~bash
  The hobby store normally sells eighty five trading cards per month. In June, the hobby store sold five more trading cards than normal. In total, how many trading cards did the hobby store sell in June? It sold [MASK] cards.
  Output: [120 0.022, 200 0.018, 100 0.016, 300 0.013, 150 0.013]
  The hobby store normally sells eighty five trading cards per month. In June, the hobby store sold five more trading cards than normal. In total, how many trading cards did the hobby store sell in June? It sold [MASK] cards. Eighty five plus five is ninety.
  Output: [ninety 0.120, eighty 0.080, sixty 0.042, seventy 0.032, fifty 0.031]
  The hobby store normally sells eighty five trading cards per month. In June, the hobby store sold five more trading cards than normal. In total, how many trading cards did the hobby store sell in June? It sold [MASK] cards. Eighty five plus five is eightly nine.
  Output: [eighty 0.056, 120 0.023, twenty 0.022, fifty 0.022, sixty 0.021]
  The hobby store normally sells eighty five trading cards per month. In June, the hobby store sold five more trading cards than normal. In total, how many trading cards did the hobby store sell in June? It sold [MASK] cards. Choose between ninety or twenty.
  Output: [twenty 0.046, ten 0.030, ninety 0.030, 120 0.026, fifty 0.026]
  The hobby store normally sells eighty five trading cards per month. In June, the hobby store sold five more trading cards than normal. In total, how many trading cards did the hobby store sell in June? It sold [MASK] cards. A) 
  ninety, B) eighty, C) forty.
  Output: [eighty 0.053, forty 0.039, sixty 0.039, ninety 0.039, seventy 0.038]
  ~~~

## Information retrieval
- Heuristic: introduce the sentence with the [MASK] to obtain the data in between sentences in the text where the desired data is located.
  Example:
  ~~~bash
  Successful videogames are [MASK] games. The most successful videogames in the market are racing games, followed by RPG games. The least enjoyed are sports games.
  ~~~
  ~~~bash
  Following the research, some of the most dangerous videogames were shooting games. Successful videogames are [MASK] games. The most successful videogames in the market are racing games, followed by RPG games. The least enjoyed 
  are sports games. Somehow people consider that [MASK] games are dangerous.
  ~~~

## Role playing. Reasoning
- Heuristic: avoid lengthy descriptions, provide simple statements, in a similar way as you would to a T5 model. Actually doing "topic:list of things" is useful, for the model to learn the things that it can do in the roleplay.
  Apparently it is better at doing role playing as a personal agent managing your mobile phone apps, choosing the best app for each use case, than knowing what tool to use to survive in a cave; it is likely, due to the
  training data provided by Google to train the model.
  Example:
  ~~~bash
  apps: calculator, calendar, search, ai. use the [MASK] app to find a restaurant.
  Output: [search 0.136, menu 0.069, mobile 0.069, restaurant 0.053, web 0.047]
  apps: calculator, calendar, search, ai. use the [MASK] app to create a meeting. use calculator for math. use calendar for meetings.
  Output: [calendar 0.345, web 0.087, search 0.084, google 0.046, facebook 0.036]
  ~~~

  ~~~bash
  tools: knife, lighter, water. You are thirsty, you use [MASK].
  Output: [water 0.201, it 0.168, food 0.046, them 0.040, weapons 0.023]
  tools: knife, lighter, water. You find a spider, you use the [MASK] to kill it.
  Output: [knife 0.228, stick 0.054, light 0.033, spear 0.023, spider 0.019]
  tools: shotgun, lighter. You find a spider, you use the [MASK] to kill the spider. shotgun kills panther, lighter kills spider.
  Output: [shotgun 0.751, gun 0.047, weapon 0.029, hammer 0.023, rifle 0.020]
  ~~~
- Heuristic: provide a list of simple sentence: Subject + Action + Complement, so that it learns about the context:
  Example:
  ~~~bash
  knife does not kill panther. knife kills spider. water removes thirst. shotgun kills panther.  You find a panther, you use the [MASK] to kill the panther.
  Output: [knife 0.907, dagger 0.015, spear 0.013, knives 0.008, blade 0.005]
  ~~~
