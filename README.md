# Text To Speech With Voice Cloning Feature (by neonsecret)
This repository is an implementation of [Transfer Learning from Speaker Verification to
Multispeaker Text-To-Speech Synthesis](https://arxiv.org/pdf/1806.04558.pdf) (SV2TTS), with custom tweaks. 
## Differences from parent
* Cleaned tensorflow requirement
* Added russian language support (+guide how to add other languages)
* Code reworks 
* Bilingual and single language model available

### Setup
### 0. delete everything from the directory, clone it from sratch
```
git clone https://github.com/neonsecret/TTS-With-Voice-Cloning-Multilang
```
or if you already cloned it
```
cd TTS-With-Voice-Cloning-Multilang
git pull
```

### 1. Install Requirements
1. Both Windows and Linux are supported. A GPU is not required on machine level, but without it it will take ages to train anything.
2. Install [ffmpeg](https://ffmpeg.org/download.html#get-packages). This is necessary for reading audio files.
3. Install [PyTorch](https://pytorch.org/get-started/locally/). Pick the latest stable version, your operating system, your package manager (pip by default) and finally pick any of the proposed CUDA versions if you have a GPU, otherwise pick CPU. Run the given command.
4. Install nvidia-pyindex by running `pip install nvidia-pyindex`
5. Install the remaining requirements with `pip install -r requirements.txt`

### 2. Train the models yourself. 

No pretrained models available, however if you succeed in training one, send it to my email (github profile).
### 3. Download Datasets (only if you plan on training the model)
I only recommend downloading [`LibriSpeech/train-clean-100`](https://www.openslr.org/resources/12/train-clean-100.tar.gz). Extract the contents as `<datasets_root>/LibriSpeech/train-clean-100` where `<datasets_root>` is a directory of your choosing. Other datasets are supported in the toolbox, see [here](https://github.com/CorentinJ/Real-Time-Voice-Cloning/wiki/Training#datasets). You're free not to download any dataset, but then you will need your own data as audio files or you will have to record it with the toolbox. For other language dataset construction, see below.
### 4. Other languages dataset
So to do a custom dataset thing, you must unpack all files into the <datasets_root> with the following structures:
```
datasets_root
    * LibriTTS
        * train-clean-100
            * speaker-001
                * book-001
                    * utterance-001.wav
                    * utterance-001.txt
                    * utterance-002.wav
                    * utterance-002.txt
                    * utterance-003.wav
                    * utterance-003.txt
```
Where each utterance-###.wav is a short utterance (2-10 sec) and the utterance-###.txt contains the corresponding transcript. 
Then you can process this dataset using:
```
python synthesizer_preprocess_audio.py datasets_root --datasets_name LibriTTS --subfolders train-clean-100 --no_alignments
```
More info [here](https://github.com/CorentinJ/Real-Time-Voice-Cloning/issues/437#issuecomment-666099538).
### 4.1 Using the open_stt datasets(russian lang)
If you use the datasets from [here](https://github.com/snakers4/open_stt), you can use the following command to preprocess it for the synthesizer training:
```
python rus_opus_preprocess.py -d <dataset_root>
```
and then go back to step 4.
This will just rename the folders and convert the opuses to wavs.
I also tweaked the original processor to detect the opus files so you can just pust the dataset to
LibriTTS/train-clean-100/
folder and skip converting to wavs.
### 4.2 Training
Not really necessary, unless you plan adding another language or finetuning pretrained models.
</br>Example synthesizer train command:
```
synthesizer_train.py rusmodel SV2TTS\synthesizer
```
### 5. Run the demo
```
python demo_cli.py 
```
if cpu only:
```
python demo_cli.py --cpu
```
