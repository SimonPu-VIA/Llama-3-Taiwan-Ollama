# Llama-3-Taiwan-Ollama
This repro notes converting a readily available LLM model from Hugging Face into a format compatible with Ollama.  
</br>
Here, the example utilizes the [Llama-3-Taiwan-8B-Instruct-DPO model](https://huggingface.co/yentinglin/Llama-3-Taiwan-8B-Instruct-DPO), readily accessible on HuggingFace and the NVIDIA PyTorch 23.09-py3 [docker container](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch) environment, as a demonstration.
</br>
</br>
## 1. Clone the model from Hugging Face repository
```
# Make sure you have git-lfs installed (https://git-lfs.com)
git lfs install
# Make sure your SSH key is properly setup in your user settings.
# https://huggingface.co/settings/keys
git clone git@hf.co:yentinglin/Llama-3-Taiwan-8B-Instruct-DPO
```

## 2. Clone lama.cpp and setting the environment
```
git clone https://github.com/ggerganov/llama.cpp.git
pip install -r requirements.txt
```

## 3. Download and installation Ollama
```
# Confirm that your system has the lspci or lshw utility installed. If not, 
# please run sudo apt-get install pciutils for example.
# https://github.com/ollama/ollama/blob/main/docs/linux.md
# installation Ollama

curl -fsSL https://ollama.com/install.sh | sh

# If you are utilizing a Docker environment and your docker system has not been
# initialized with systemd, you can deploy the Ollama setp within a separate 
# Docker container. For example:
sudo docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

## 4. Convert the model to GGUF format
```
python llama.cpp/convert_hf_to_gguf.py Llama-3-Taiwan-8B-Instruct-DPO --outfile Llama-3-Taiwan-8B-Instruct-DPO.gguf --outtype f16
```

## 5. Import GGUF model into Ollama 
```
# Create the model locally with your username as the namespace
ollama create <ollama-username>/<model-name> -f /path/to/Modelfile
# Show the model import successful or not
ollama list
# Run the model 
ollama run <ollama-username>/<model-name>

# If you are using the Ollama setp within a separate Docker container. Please
# run the following command to import the model to the Ollama.
docker exec -it ollama create <ollama-username>/<model-name> -f /path/to/Modelfile
docker exec -it ollama list
docker exec -it ollama run <ollama-username>/<model-name>
```

## 6. Push and share the model to Ollama(Optional)
```
# Ensure that you possess both an Ollama account and the requisite Ollama keys.
# The key must reside in the /usr/$USENAME/.ollama directory(for example: 
# /root/.ollama) and be set up as passphrase-free.
# 
# 6.1 Create a new model https://www.ollama.ai/new
# 6.2 Push the model
ollama push <ollama-username>/<model-name>

# If you are using the Ollama setp within a separate Docker container. Please 
# run the following command to push the model to the Ollama server.
docker exec -it ollama ollama push <ollama-username>/<model-name>
```
