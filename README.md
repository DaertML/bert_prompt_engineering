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
  - Prompt template: CONTENTS. It was [MASK].


