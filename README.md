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
