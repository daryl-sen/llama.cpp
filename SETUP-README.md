# Setting up Llama cpp locally

Clone the Llama.cpp repo (ideally, fork it first)

```
git clone https://github.com/ggerganov/llama.cpp
```

In the root directory of the repo, run `make`.

Clone the appropriate model into the `./models` directory. Using vicuna here as an example:

```
cd models
git clone git clone https://huggingface.co/lmsys/vicuna-7b-v1.3
```

Go back to the root directory, create a python virtual environment and install the dependencies

```
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
```

Convert the huge LLM from float16 to int4. Note, the `convert-pth-to-ggml.py` in the original model has [been deprecated](https://github.com/ggerganov/llama.cpp/pull/1641/files). Use `convert.py` instead. This will produce a `.gguf` file in the vicuna model directory.

```
python convert.py models/vicuna-7b-v1.3/
```

Quantize it, whatever that means. Using the f16 gguf file, this will produce a new bin file.

```
./quantize ./models/vicuna-7b-v1.3/ggml-model-f16.gguf ./models/vicuna-7b-v1.3/ggml-model-q4_0.bin 2
```

Run a quick test

```
./main -m ./models/vicuna-7b-v1.3/ggml-model-qa_0.bin -n 128
```

Run the model in interactive mode

```
./main -m ./models/vicuna-7b-v1.3/ggml-model-q4_0.bin -t 4 -c 2048 -n 2048 --color -i --reverse-prompt '### Human:' -p '### Human:'
```

Loosely based on this broken tutorial on medium: https://medium.com/@enricdomingo/chatgpt-is-already-the-past-run-llama-2-in-your-laptop-for-free-b27aa6f1cb3e, and the accompanying video that shows different instructions: https://www.youtube.com/watch?v=T4mJcz7dRvE&t=372s&ab_channel=EnricDomingo

Version of Llama cpp: https://github.com/ggerganov/llama.cpp/tree/afefa319f1f59b002dfa0d1ef407a2c74bd9770b
Commit hash: `afefa319f1f59b002dfa0d1ef407a2c74bd9770b`

Forked repo: https://github.com/daryl-sen/llama.cpp
