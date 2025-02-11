# video link will be added once the video published

None of the things in this tutorial won't affect your other installations such as Stable Diffusion

Join our discord channel and ask any questions : https://discord.com/servers/software-engineering-courses-secourses-772774097734074388

## prerequisites

To follow this video you need to have Python 3.10.x and Git to be installed on your computer and have basic knowladge about how to use

Git link : https://git-scm.com/download/win

Python 3.10.9 : https://www.python.org/ftp/python/3.10.9/python-3.10.9-amd64.exe 

I have excellent tutorial for Python and Git installation here : https://youtu.be/B5U7LJOvH6g 

### Part 1 - Preparing training speech files
We will begin with pre-processing the speech training file

First accept the license terms on https://huggingface.co/pyannote/segmentation 

So you must have a free hugging face account to accept terms. Register hugging face : https://huggingface.co/join

To do that we will git clone and install ozen toolkit : https://github.com/devilismyfriend/ozen-toolkit

commit id : f5ea2ce4f95bfa54635ec9deac56cb546aa7d516

```git clone https://github.com/devilismyfriend/ozen-toolkit```

To be able to use this, you need to have installed Anaconda or Miniconda with Python 3.10

Anaconda download and install link : https://www.anaconda.com/download

Miniconda download and install link : https://docs.conda.io/en/latest/miniconda.html

If you fail to install Anaconda here my tutorial where I installed and shown : https://www.youtube.com/watch?v=YJebdQ30UZQ

Then double click and run "Set Up Ozen.bat" file inside directory 

Lets say something went wrong and you want to reinstall or, you had installed this repo before and you want to reinstall, then Anaconda will give you this message

```CondaValueError: prefix already exists: C:\Users\King\miniconda3\envs\ozen```

So open folder ```C:\Users\King\miniconda3\envs``` and delete the ```ozen``` directory 

Then after installation drag and drop the training speech file into Drag_Here.cmd

It will ask you hugging face token get it here : https://huggingface.co/settings/tokens

Merge multiple audio files into a single file : ```ffmpeg -i "concat:input1.mp3|input2.mp3|input3.mp3" -acodec copy output.mp3```


There are several key issues. It uses by default large model. If your GPU is not 12 GB you can either use CPU to transcribe or you can use medium or small model

Whisper tutorial : https://www.youtube.com/watch?v=msj3wuYf3d8

```
tiny	39 M	tiny.en	tiny	~1 GB	~32x
base	74 M	base.en	base	~1 GB	~16x
small	244 M	small.en	small	~2 GB	~6x
medium	769 M	medium.en	medium	~5 GB	~2x
large	1550 M	N/A	large	~10 GB	1x
```

Moreover, its transcribed txt file has encoding problem. So copy a all change encoding and paste again and save as I have shown in the tutorial video. I had to spend quite a bit time to find out this.

If you ever needed to install a missing package manually, to activate anaconda or miniconda venvs you need to open a anaconda cmd and type the generated venv name like

```conda activate ozen```

```pip uninstall torch```

https://pytorch.org/

```pip3 install torch==1.13.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117```

Notepad++ to change transcription file encoding : https://notepad-plus-plus.org/downloads/ 

### Part 2 - Doing training and training settings

We will use DL-Art-School for training. It has a very nice GUI and makes training piece of cake

commit id : 5ab4d9ed415e0a5c0b7ce9aae02aa2e76fe0eccd

https://github.com/152334H/DL-Art-School

This also requires Anaconda or Miniconda with Python 3.10

Since we already installed it in the previous step you don't have to install again

If you want to reinstall this then you need to delete the venv generated in miniconda venvs

```C:\Users\King\miniconda3\envs```

Do git clone

```git clone https://github.com/152334H/DL-Art-School```

Double click Setup DLAS.bat 

It will make a venv in anaconda or miniconda and install everything. 

However you may encounter an error like i did related to torch. So we will reinstall torch manually

If you ever needed to install a missing package manually, to activate anaconda or miniconda venvs you need to open a anaconda cmd and type the generated venv name like

```conda activate DLAS```

```pip uninstall torch```

```pip3 install torch==1.13.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117```

After installation double click "Start DLAS.cmd" file and it will open the GUI interface

Follow the steps I have shown in the video to start training.

I have explaine

* What does each step work
* Batch size effect on 1 epoch
* How total number of epochs calculated based on your batch size and number of training elements and total number of your set steps
* How to analyze and see loss value over time with a custom script and many more

I have even shown how you can continue training by using your last checkpoint.

### Part 3, generating cloned voice via tortoise-tts-fast

commit id : f4820a8eabdd559eb369a07d60cbe03960ddd54d

 https://github.com/152334H/tortoise-tts-fast

```git clone  https://github.com/152334H/tortoise-tts-fast```

```python -m venv venv```

activate venv

```
git clone https://github.com/152334H/tortoise-tts-fast
cd tortoise-tts-fast
pip3 install torch==1.13.1 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```

Then while venv is activated move into your main installation folder e.g.
F:\voice clone tutorial\tortoise-tts-fast

```
python -m pip install -e .
pip3 install git+https://github.com/152334H/BigVGAN.git
```

This is the folder where you need to run base command
(venv) your_install_path\tortoise-tts-fast\scripts>
e.g. (venv) F:\voice clone tutorial\tortoise-tts-fast\scripts>


```python tortoise_tts.py --preset fast --ar_checkpoint "F:\voice clone tutorial\DL-Art-School\experiments\voiceclone\models\40_gpt.pth" "Welcome to the software engineering courses channel."```

For the script I shared and shown to work first open **inference.py** under **\tortoise-tts-fast\tortoise folder**

copy paste this code and change function split_text

```
def split_text(text: str, text_split: str):
    if text_split:
        texts = text.split(";")
    else:
        texts = split_and_recombine_text(text)
    #
    if not texts:
        raise ValueError("no text provided")
    return texts
```

**decent quality (low VRAM) command below with additional output directory and iterations. Dont forget to change --ar_checkpoint path and output_dir path

```python tortoise_tts.py --seed 1 --candidates 3 --voicefixer True --no_cache False --low_vram True --text_split "gg" --diffusion_iterations 100 --output_dir "F:\voice clone tutorial\multi_sentences" --preset fast --ar_checkpoint "F:\DL-Art-School\experiments\test2\models\1480_gpt.pth" "Welcome to the software engineering courses channel.;This channel is the best source for learning technology and artificial intelligence.;Please subscribe."```

**best quality (high VRAM) command below with additional output directory and iterations. Dont forget to change --ar_checkpoint path and output_dir path

```python tortoise_tts.py --seed 1 --candidates 3 --voicefixer True --no_cache False --low_vram False --text_split "gg" --diffusion_iterations 1000 --output_dir "F:\voice clone tutorial\multi_sentences" --preset high_quality --ar_checkpoint "F:\DL-Art-School\experiments\test2\models\1480_gpt.pth" "Welcome to the software engineering courses channel.;This channel is the best source for learning technology and artificial intelligence.;Please subscribe."```

* ```--seed``` May improve consistency. Remove to get more randomness or try different seeds to get a better generation. Ask chatGPT to learn more about seeds in generative AI  
* ```--candidates``` Allows you to generate multiple variations of same speech in 1 run. Can be very useful to get better options
* ```--voicefixer``` I prefer voicefixer False and using adobe podcast enchance : https://podcast.adobe.com/enhance 
* ```--low_vram```Enable low_vram to reduce VRAM usage if you get out of memory exception
* ```--no_cache``` Enable no_cache to reduce VRAM usage if you get out of memory exception 
* ```--half```  Enable half to reduce VRAM usage if you get out of memory exception - this reduces quality
* ```--diffusion_iterations``` 250 and above said to generate good quality. This just increases processing time
* ```--output_dir``` Set your desired output folder where you want images to be saved
* ```--preset``` Significantly affects the quality. Choose according to your GPU among : ultra_fast - fast - standard - high_quality
* ```--ar_checkpoint``` Give the path of your trained voice model checkpoint file path
* The last input given inside "" is the text you want to be transcribed. If you apply the split_text function modification as show above, seperate each sentences with a semicolon ; 

adobe enchance speech
https://podcast.adobe.com/enhance

automatic processing script files shown in the video
https://www.patreon.com/posts/voice-clone-82712205

## the parameters you need to modify are as below

### in loss_graph.py 

install this pip package in your main python installation. start a cmd window and type below code

```pip install matplotlib```

Then modify the path variable in script. Change path in below part of script to your training log file

```
with open(r"F:\voice clone tutorial\DL-Art-School\experiments\bigtom\train_bigtom_230509-021906.log", 'r') as file:
    text = file.read()
```

Then start a cmd whereever this file is located and execute below code to display loss graphs. This script is safe to use during training to see values.

```python loss_graph.py ```

### in process_given_speech_text.bat

```call "F:\voice clone tutorial\tortoise-tts-fast\venv\Scripts\activate.bat"``` change with your venv installed path

```cd "F:\voice clone tutorial\tortoise-tts-fast\scripts"``` change with your tortoise-tts-fast installed path

```--voicefixer``` I suggest you to set this False and use adobe voice enhancer : https://podcast.adobe.com/enhance 

```set outputdir=``` : where you want your output speech files to be saved a folder path

```set voicesdir=``` : give your training voices directory path - optional i am not sure if makes difference atm

```set training_checkpoint_file=``` : where your trained checkpoint file located

```--diffusion_iterations``` - best suggested value is 250 and above but you can use either 50 20 1000 up to you. test each one depending on your GPU and speed you are expecting

```--seed``` if you don't provide a seed i think everytime it will be random thus you may get different output as expected in generative AI models

```--preset``` this really makes huge difference. it is flow worst to best like this. as you use better one it will be much more slower and will use more vram ultra_fast - fast - standard - high_quality

```--low_vram``` - you can make it true like this if you have low vram ```--low_vram True```

```--no_cache``` - you can make it true like this if you have low vram ```--no_cache True``` - use only if low_vram fails

```--half``` - you can make it true like this if you have low vram ```--half True``` - use only when all others fails

### in epoch_comparison.bat

```set CHECKPOINTS_DIR=``` : directory of your checkpoints

```set OUTPUT_BASE_DIR=``` : directory of where checkpoint results will be saved parent directory

other settings are as above

### in pre_process_given_text_file.py

```split_length``` : set your split length. by default maximum suggested is 200. if you go over it, it will start to hallucinate. too small sentences are also somewhat problematic. therefore, the script will automatically merge sentences when the merged sentence length is smaller than split_length

### in move_check_point_sound_files.py

This file will iterate your check point comparison generated sound files and put them into a new folder with the parent name so that it will be easier to test each checkpoint sound file easier

It will copy the files don't move

Just change the paths of the below lines

```
root_folder = r'F:\voice clone tutorial\check_point_comparison_v3'
target_folder = r'F:\voice clone tutorial\check_point_comparison_v3_all_files'
```

To run the file open cmd in that folder and execute as below

```python pre_process_given_text_file.py ```
