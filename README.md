
# Quantization of Hugging Face models using llama.cpp
**0. Install make, gcc and git-lfs in the system**

1. Create a directory named os_model and move to that directory.
```bash
mkdir os_model
cd os_model
```

2. Create a virtual environment named osenv(do it according to your OS).
```zsh
  python -m venv osenv
  source osenv/bin/activate
```

3. Install Git Large File System(lfs)
and clone the sqlcoder-34b repo from hugging face. This takes a lot of time because the remote repo is nearly 140GB. **Be patient and do not cancel the download.**
```bash
git lfs install
git clone https://huggingface.co/defog/sqlcoder-34b-alpha

```

4. Download only tokenizer.model file from the following repo https://huggingface.co/defog/sqlcoder-7b/tree/main and place in the sqlcoder-34b-alpha folder.

5. Clone llama.cpp and setup python conversion stuff
``` bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
pip install -r requirements.txt
```

6. Create a directory called sqlcoder-34b-alpha in the models directory located in llama.cpp folder

7. Run the following commands
```bash
make
python3 convert.py <model_folder../os_model/sqlcoder-34b-alpha> --outfile ./models/sqlcoder-34b-alpha/ggml-sqlcoder-34b-f16.gguf --outtype f16
```

8. Now let's quantize the above generated model to q4_k. You can qunatize it to q8_0 for better performance(to do it replace q4_k with q8_0 in the below code). 
```bash
./quantize ./models/sqlcoder-34b-alpha/ggml-sqlcoder-34b-f16.gguf ./models/sqlcoder-34b-alpha/ggml-sqlcoder-34b-q4_k.gguf.bin q4_k
```

9. We can use this model for various text-generation tasks. The model downloaded performs well for the task of natural language to SQL query generation.

