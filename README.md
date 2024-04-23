# Llama_Setup
Setting up llama on raspberry pi 4
## Introduction
This is a tutorial on how to run Llama 7b on raspberry Pi 4, here is a list of required items for this tutorial:
* Source PC with either Windows or a Linux distribution to retrieve and quantize the LLM(‘s)
* 8GB Raspberry Pi 5 to run the LLM on
* A memory card with at least 32GB containing a pre-installed OS such as Raspbian (I personally use Ubuntu 23.10 on my RPi for this)
* An USB stick with at least 34GB of space available to transport the LLM from your source PC to your RPi

## Preparing the source PC
We used Macbooks as our source PC, then downloaded UTM virtual machine to use Linux systems on our local mac. 
Here are the specs:
* Macbook 2023 M3 macbook, macOS: Sonoma 14.2.1
* UTM  https://mac.getutm.app/
* Ubuntu: 23.10 Ubuntu https://ubuntu.com/download/server/arm
To install Linux virtual machine, refer to this Youtube video, skip to the last part where he set up disk mounting / local ->vm file sharing
https://www.youtube.com/watch?v=O19mv1pe76M&t=212s

# Configuring Linux source PC
Once Ubuntu works, we start installing dependencies.
> sudo apt update

> sudo apt install git

> sudo apt install python3-pip

Note: if you run into python3 is externally managed error, do this, otherwise skip
> sudo mv /usr/lib/[python3.11]/EXTERNALLY-MANAGED /usr/lib/[python3.11]/EXTERNALLY-MANAGED.old

> pip install torch numpy sentencepiece

> sudo apt install g++ build-essential

> git clone https://github.com/ggerganov/llama.cpp

> cd llama.cpp

> make

> sudo apt install qbittorrent

> qbittorrent

Then, click on the link icon and put in the follwing magnet link:
> magnet:?xt=urn:btih:ZXXDAUWYLRUXXBHUYEMS6Q5CE5WA3LVA&dn=LLaMA

Then, once you enter that interface, only download 7B and tokenizer_checklist.chk and tokenizer.model
After the download finishes, lacate the 7B folder and the tokenizer files and copy them to the llama.cpp/models folder
Then, vim into the params.json file and change the '-1' to '32000'
![Screenshot 2024-04-22 at 9 12 14 PM](https://github.com/llelez/Llama_Setup/assets/167836348/8f4fba90-d2fa-4b8e-a222-8a22bed8fb25)
Then,
> cd llama.cpp
> python3 convert.py models/7B

> ./quantize ./models/7B/ggml-model-f16.gguf ./models/7B/ggml-model-q4_0.gguf q4_0

> mv models/7B models/llama-7b

> ./examples/chat.sh

Now you can talk to Bob:

![Screenshot 2024-04-22 at 7 20 22 PM](https://github.com/llelez/Llama_Setup/assets/167836348/70e78f90-1ff2-458e-adbd-0f789391bd26)
Finally, you move the llama-7b file to your USB stick. 

## Intalling the LLM on Raspberry Pi
First you flash your brand new ras pi and download os for it. Then, you can begin to install your quantized LLM on it

> sudo apt update
> sudo apt install git

> git clone https://github.com/ggerganov/llama.cpp
...
SInce the commands are similar to the ones I mentioned above, I will not repeat it again.
Copy up until the 'make' command.

Next, move the content from your external drive to the /models/ folder in your llama.cpp project. Now you got Bob on your raspi as well :)
