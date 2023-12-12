---
title: "Hands-on experience with `llama2` with Python: Building a simple Q&A app"
author: "Pankaj Chejara"
date: "2023-12-12"
categories: [python,llm,llama2,nlp,gen-ai]
image: "./images/llama2/llama2.jpg"
code-block-background: true
highlight-style: "arrow"
toc: true
---

# First hands-on experience with running LLAMA2

This post shares the step-by-step process of running the LLAMA2 model locally on MAC. The post is a reflection of my learning of Large Language Models and their practical applications.

![](/Users/htk/Documents/Personal/pankajchejara23.github.io/posts/post-with-code/images/llama2/llama2.jpg)
Image by [Freepik]([Free Vector | Cute alpaca background with quote](https://www.freepik.com/free-vector/cute-alpaca-background-with-quote_2658409.htm#query=llama%202&position=21&from_view=search&track=ais&uuid=9b522a7a-d5d7-4b4b-a99a-8143e7e48b0a))



This post assumes **your system already has Python installed**.

## Installing required libraries

#### Clone llama C++ repo

```bash
git clone https://github.com/ggerganov/llama.cpp.git
```

You can create a virtual environment for setting up llama. I have used here `conda` commant to create a new environment for my setup.

```bash
conda create -n llama
conda activate llama
```

Next, go to the repository and run the `make` command.

```bash
cd llama.cpp
make
```

Install python packages

```bash
pip3 install llama-cpp-python
```

#### Accesing llama model files

We need llama2 model files. To access these files, a request has to be made by filling out the form given [here](https://ai.meta.com/resources/models-and-libraries/llama-downloads/). On submission, you will get an email providing instructions to download llama2 models.

:::{.callout-tip}

The email will have an URL which is asked while downloading the models.

:::

#### Converting model files

Now, we have llama2 models and installed llama CPP binding for Python. We will now convert the downloaded model files.


The first step in converting the model file is to run the following command while being in the llama.cpp directory. To run this command, we need the path of the directory containing the downloaded models.

```bash
python3 convert.py <directory_containing_llama_model>
```

This command will generate a file with the name ggml-model-f16.gguf and save it in the repository of llama_model which has the downloaded models.


The second step will run the quantize command.

```bash
./quantize <directory_containing_llama_model>/ggml-model-f16.gguf <directory_containing_llama_model>/ggml-model-q4_0.gguf q4_0
```



:::{.callout-important}

You can also use the already converted file available [here](https://drive.google.com/file/d/1afPv3HOy73BE2MoYCgYJvBDeQNa9rZbj/view?pli=1) if you get any error while running above two steps.

:::

#### Installing LangChain library to build applications

LangChain makes it easier to develop applications using language models. You can learn about it more [here](https://python.langchain.com/docs/get_started/introduction)

```bash
pip3 install langchain 
```

## Building applications on the top of a language model

Now, we will see the **fun part** of asking a question to llama2 and getting its answer. 

The following python script has been used from the tutorial available [here](https://github.com/facebookresearch/llama-recipes/blob/main/demo_apps/HelloLlamaLocal.ipynb).

```python
from langchain.llms import LlamaCpp
from langchain.callbacks.manager import CallbackManager
from langchain.callbacks.streaming_stdout import StreamingStdOutCallbackHandler

callback_manager = CallbackManager([StreamingStdOutCallbackHandler()])

llm = LlamaCpp(model_path='../ggml-model-q4_0.gguf',
    temperature=0.0,
    top_p=1,
    n_ctx=6000,
    callback_manager=callback_manager, 
    verbose=True,
)

question = "who is mr. narendra modi?"
answer = llm(question)

print('Q:',question)
print('A:',answer)
```

Output:

```bash
Mr. Narendra Modi is the current Prime Minister of India, serving since May 2014. He is known ...
```



## References

1. https://medium.com/@karankakwani/build-and-run-llama2-llm-locally-a3b393c1570e

2. https://github.com/facebookresearch/llama-recipes/blob/main/demo_apps/HelloLlamaLocal.ipynb
