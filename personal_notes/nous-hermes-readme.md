Convert the huge LLM from float16 to int4. Note, the `convert-pth-to-ggml.py` in the original model has [been deprecated](https://github.com/ggerganov/llama.cpp/pull/1641/files). Use `convert.py` instead. This will produce a `.gguf` file in the vicuna model directory.

```
python convert.py models/Nous-Hermes-13b/
```

Quantize it, this is the step that actually converts it from a float16 to int4. Using the f16 gguf file, this will produce a new bin file. This will result in a loss in quality, but makes it possible to run on low-spec hardware.

```
./quantize ./models/Nous-Hermes-13b/ggml-model-f16.gguf ./models/Nous-Hermes-13b/ggml-model-q4_0.bin 2
```

Run a quick test

```
./main -m ./models/Nous-Hermes-13b/ggml-model-q4_0.bin -n 128
```

Run the model in interactive mode

```
./main -m ./models/Nous-Hermes-13b/ggml-model-q4_0.bin -t 4 -c 2048 -n 2048 --color -i --reverse-prompt '### Instruction: ' -p '### Instruction: '
```
