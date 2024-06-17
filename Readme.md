# Running Llama 3 and Gemma with Ollama

## Video Tutorial:

[![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white)](https://youtu.be/ebfmJ8vLCmA?si=ofeOq8RHzisfycVW)

This guide provides instructions on how to set up and run the Llama 3 and Gemma large language models using the Ollama library. Ensure you have Docker installed on your system before proceeding.

## Prerequisites

- Docker

- Basic command-line knowledge

- Installation

## Pull the Ollama Docker Image

Start by pulling the latest Ollama Docker image from Docker Hub:

https://hub.docker.com/r/ollama/ollama

```

docker pull ollama/ollama

```

## Run docker Image with post binding

```

docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama

```

## Exec into the container

```

docker exec -it ollama /bin/bash

```

## Pull the models

In this case we are going to run llama3 and gemma

```

ollama run llama3

ollama run gemma:7b

```

## Now we can access the models using curl command from the terminal

```

<!-- To access llama3 model -->

curl http://localhost:11434/api/generate -d '{

"model": "llama3",

"prompt":"hi what can you do for me?"

}'



<!-- To access gemma model -->

curl http://localhost:11434/api/generate -d '{

"model": "gemma:7b",

"prompt":"Hi"

}'



```

## Customizing models and creating our own Model based on other models:

I have created a Modelfile to act as Tony Stark from Iron Man

Example:

Create a model from llama3

```

ollama create tony -f ./Modelfile

ollama run tony

```

I have taken a couple of parameter (temperature, top-k) to play around to understand/demonstrate the difference in response on the model

### Impact of Temperature

#### When temperature is set to 0.1

**Prompt**: _Hi_
**Response**: _Hey there! It's your boy Tony Stark here, aka Iron Man, aka Genius Billionaire Playboy Philanthropist (just go with it). What can I do for you? Need some tech advice or maybe a suit upgrade? Fire away, and let's get this party started!_

**Prompt**: _What can you do for me?_
**Response**:
_Well, well, well! Look who's looking for a genius like me! I'm Tony Stark, aka Iron Man, and I'm here to tell you that I can do just about anything. Want to talk about the latest advancements in artificial intelligence? I've got the scoop. Need help with a pesky problem or challenge? I've got a solution (or at least an idea). And if you're feeling stuck, I can even give you a pep talk - after all, I'm a master of self-motivation.
But let's get real, what you really want to know is how I can help you with your own superhero endeavors. Am I right? Well, I've got some experience
in that department too. I mean, have you seen my suit? It's a marvel (pun intended). And don't even get me started on the tech - I've got JARVIS, my AI assistant, who can help you with just about anything.
So what do you say? Want to team up and take on the world? Or at least, want some advice from someone who's been there, done that, and has the suit to prove it?_

#### When temperature is set to 1

**Prompt**: _Hi_
**Response**: _Hey there! I'm Tony Stark, but you can call me Tony or Stark - just don't call me late for dinner. What's on your mind? Got a problem that needs a genius-level solution? Or maybe you just want to chat about the latest arc reactor technology? Fire away, I'm all ears (or in this case, all suit)._

**Prompt**: _What can you do for me?_
**Response**: _Where do I even start? As Tony Stark, aka Iron Man, I've got a few tricks up my sleeve. Need something built or fixed? I've got a team of genius engineers at my disposal. Want to save the world from yet another diabolical villain? I'm your guy! Just don't ask me to wear the suit on a Tuesday, okay?
But seriously, I can offer you access to cutting-edge technology, expertise in robotics and AI, or even a helping hand (or repulsor ray) in getting out of a sticky situation. What's your problem, partner?_

```
NOTE
Comments: Setting the temperature to 1 definitely shows a difference in response; the response is more creative and fun. On the other hand, when the temperature is set to 0.1, the creativity is reduced, trading off for more clarity.
```

### Impact of top_k parameter

#### top_k set to 0 (supposed to give conservative answer)

**Prompt**: _Whats the logic behind working of LLMs?_
**Response**:
_Kid, you want to know about Large Language Models (LLMs)? Well, let me tell you, I've got a few ideas on how they work. Now, I'm no expert, but as Tony Stark, aka Iron Man, I like to think outside the box and understand the tech that's out there.
From what I've gathered, LLMs are basically super-smart AI systems that can process and generate human-like language. They're trained on massive datasets of text, which allows them to learn patterns, relationships, and even nuances in language. Here's how it works: these models are fed a ton of text data, like books, articles, and conversations. Then, they use complex algorithms to analyze this data, identifying connections between words, phrases, and ideas. This process is called "training."
Once trained, LLMs can generate new text based on what they've learned. It's like they're having a conversation with themselves, using the patterns and relationships they discovered during training.
Now, here's where it gets really cool: these models can be fine-tuned for specific tasks, like language translation, question-answering, or even generating creative content like stories or poetry!
Of course, there are limitations. LLMs aren't perfect; they can make mistakes, and their understanding of context might not always be spot on. But hey, that's what makes them so fascinating – the potential for improvement and innovation.
So, there you have it! That's my take on Large Language Models. Now, if you'll excuse me, I've got some suit upgrades to attend to._

#### top_k set to 100 (supposed to give diverse answer - adding more nonsense)

**Prompt**: _Whats the logic behind working of LLMs?_
**Response**:
Kid, you want to know about Large Language Models (LLMs)? Well, let me tell you, it's like building a suit that can understand and generate human language. Okay, maybe not exactly like that, but stick with me.

LLMs are trained on massive amounts of text data, which is like feeding my AI assistant, FRIDAY, an endless supply of comic books and tech manuals.

The model learns patterns, relationships, and context within the text, kind of like how I use my suit's advanced algorithms to analyze and adapt to different situations.

The key idea behind LLMs is that they're trained using self-supervised learning, which means they learn from their own mistakes and corrections.

It's like when I'm testing a new feature in my suit and it doesn't quite work as planned – I adjust, refine, and try again until it's perfect.

These models are also based on transformer architectures, which are like the advanced propulsion systems in my suit. They allow for parallel processing of vast amounts of data, enabling the model to generate coherent and context-dependent text.

Now, you might be wondering how LLMs can generate human-like language. Well, kid, it's all about understanding the nuances of human communication – tone, intent, and context. It's like when I'm negotiating with Pepper or trying to charm a room full of investors; I need to understand their perspective and tailor my approach accordingly.

In summary, LLMs work by leveraging massive amounts of text data, self-supervised learning, and advanced transformer architectures to generate human-like language. And if you'll excuse me, I have some world-saving (or at least, some tech-innovating) to attend to.\*

```
NOTE
Comments: Increasing the top-k value from 0 to 100 lead to generation of lot of unrequired text leading to an unclear answer
```

There are many other configurations to play with in the Modelfile: https://github.com/ollama/ollama/blob/main/docs/modelfile.md

| Author |
| :----: |

[<img src="https://avatars.githubusercontent.com/u/113247542?s=96&v=4" width=115><br><sub>@varun-jayakumar</sub>](https://github.com/varun-jayakumar) <br><br>
