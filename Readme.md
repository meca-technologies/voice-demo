<!-- version tag -->

 
RUTH TTS Benchmark, accuracy and voice quality test.
---
Objective metrics such as the Signal-to-Noise Ratio (SNR), the Mean Opinion Score (MOS), and the Perceptual Evaluation of Speech Quality (PESQ) are used to measure the quality of the audio.

RUTH is cli based tool that can be used to train and test models. 


# <div align="center">Full documentation</div>


# Metrics

To compute these metrics, we use Python libraries such as pydub and pesq. pydub is a simple and easy-to-use audio manipulation library that can be used to extract and process audio data. pesq is a Python implementation of the ITU-T P.862 PESQ algorithm, which is a widely used objective metric for evaluating speech quality.

we used the MUSAN dataset, which provides a diverse set of audio signals for evaluation, including music, speech, and noise. 

# <div align="center">See installation bellow</div>

    $ pip install pyttsx3 librosa numpy python-Levenshtein webrtcvad pydub pesq

1- Create a file, e.g tts_benchmark.py
   
    import os
    import time
    import librosa
    import numpy as np
    import pyttsx3
    import Levenshtein
    import webrtcvad
    from pydub import AudioSegment
    from pesq import pesq

    # Define parameters
          texts = [
           "The quick brown fox jumps over the lazy dog.",
            "The rain in Spain stays mainly in the plain.",
            "To be or not to be, that is the question.",
            "In Xanadu did Kubla Khan a stately pleasure-dome decree.",
            "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife."
                  ]
            audio_files = [
            "vendor1_audio.wav",
            "vendor2_audio.wav",
            "vendor3_audio.wav"
                  ]
    noise_file = "noise.wav"

    # Initialize TTS engines
    engines = []
    for name in ["sapi5", "espeak", "google"]:
    engine = pyttsx3.init(driverName=name)
    engines.append(engine)

    # Load VAD model
vad = webrtcvad.Vad()

# Generate speech output and evaluate performance
for engine in engines:
    print(f"Testing {engine.getProperty('name')} TTS engine...")
    for i, text in enumerate(texts):
        # Measure latency
        start_time = time.time()
        engine.say(text)
        engine.runAndWait()
        end_time = time.time()
        elapsed_time = end_time - start_time
        print(f"Text-to-speech latency for '{text}': {elapsed_time:.2f} seconds")

        # Evaluate WER
        generated_text = engine.getProperty('utterance_finished_string')
        wer = Levenshtein.distance(text.lower(), generated_text.lower()) / len(text.split())
        print(f"WER for '{text}': {wer:.2f}")

        # Evaluate VAD
        y, sr = librosa.load(audio_files[i], sr=16000)  # load audio file at 16 kHz sample rate
        frame_duration = 30  # duration of VAD frames in milliseconds
        frame_samples = int(frame_duration * sr / 1000)  # number of samples per VAD frame
        frame_step = int(frame_samples / 2)  # number of samples between VAD frames
        for j in range(0, len(y), frame_step):
            frame = y[j:j+frame_samples]
            if len(frame) < frame_samples:
                break
            is_speech = vad.is_speech(frame.tobytes(), sr)
            if is_speech:
                print(f"Speech detected in frame {j // frame_step}")
                break

        # Compute SNR
        clean_audio = AudioSegment.from_wav(audio_files[i])
        noisy_audio = clean_audio.overlay(AudioSegment.from_wav(noise_file))
        snr = 10 * np.log10(np.sum(np.square(clean_audio.get_array_of_samples())) / np.sum(np.square(noisy_audio.get_array_of_samples())))
        print(f"SNR for '{text}': {snr:.2f} dB")

        # Compute MOS and PESQ
        clean_audio_data, _ = librosa.load(audio_files[i], sr=16000)
        noisy_audio_data, _ = librosa.load(noisy_audio, sr=16000)
        pesq_score = pesq(16000, clean_audio_data, noisy_audio_data, 'wb')
        mos_score = (1 + pesq_score) / 2 * 5
        print(f"MOS for '{text}':

   
   
   
1- Save the code to a Python file, e.g. tts_benchmark.py
2- Place the input audio files (vendor1_audio.wav, vendor2_audio.wav, and vendor3_audio.wav) and the noise file (noise.wav) in the same directory as the Python file.
3- Open a terminal or command prompt in the directory where the Python file and audio files are located.
4- 


    
    $ git clone https://github.com/prakashr7d/Research-implementation-NLU-engine.git
    $ cd Research-implementation-NLU-engine
    $ python setup.py install
  
### Using makefile (for linux & mac users)

 [Makefile](https://www.gnu.org/software/make/manual/make.html#toc-Overview-of-make) is a file that contains a set of directives used by make build automation tool to generate executables and other non-source files of a program from the program's source files.


    $ git clone https://github.com/prakashr7d/Research-implementation-NLU-engine.git
    $ cd Research-implementation-NLU-engine

for ubuntu, 

    $ make bootstrap
   <!--create's poetry  -->

for mac,

    $ make bootstrap-mac

then finally to install package run the following bash command

    $ make install


### Pytorch installation with GPU support


    $ pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116



# <div align="center">Documentation </div>


 Getting Started
----
The main objective of this lib performs to extract information by parsing the sentence written in natural language. To getting started with RUTH let's follow the below steps.
Run the following command to build an initial project with data and a default pipeline file.

    $ mkdir project_name
    $ ruth init 

Output

Project will be initialized with below structure 
```bash
.
├── data
│    └── example.yml
└── pipeline.yml
```
Project will be created with example data and pipeline   

## <div >CLI </div>

RUTH has a CLI interface to train and test the model, to get started with the CLI interface, run the following command

    $ ruth --help
    
for example, to train the model, run the following command

    usage: $ ruth [-h] [-v] {train,test} ...

## <div >Training </div>


To train the model, run the following command

    $ ruth train -p path/to/pipeline.yaml 
      -d path/to/dataset.json

Parameters


    -p, --pipeline  Pipeline file 
    -d, --data dataset path 
 Saving Trained models
---

Once the training is finished the model will be saved in a directory named `models` in the current working directory.

Dataset format 
---
RUTH uses a yaml file to store the training data, the yaml file should have the following syntax

example 

```yml
version: "0.1"
nlu:
- intent: ham
  examples: |
    - WHO ARE YOU SEEING?
    - Great! I hope you like your man well endowed. I am  &lt;#&gt;  inches
    - Didn't you get hep b immunisation in nigeria.
    - Fair enough, anything going on?
    - Yeah hopefully, if tyler can't do it I could maybe ask around a bit
- intent: spam
  examples: |
    - Did you hear about the new Divorce Barbie? It comes with all of Ken's stuff!
    - I plane to give on this month end.
    - Wah lucky man Then can save money Hee
    - Finished class where are you.
    - K..k:)where are you?how did you performed?
```

<div >Pipeline</div>
---
RUTH is a pipeline based NLU engine, it has 3 basic main components
- Tokenizer
- Featurizer
- Intent Classifier

In [pipeline-data.yml](https://github.com/prakashr7d/Research-implementation-NLU-engine/blob/main/data/test/pipelines/pipeline-basic.yml) file is used to define the pipeline and its components
Example of pipeline-basic.yml file for Support Vector Machine (SVM) based intent classifier and CountVectorizer based featurizer.

```yaml
task:
pipeline:
  - name: 'WhiteSpaceTokenizer'
  - name: 'CountVectorFeaturizer'
  - name: 'SVMClassifier'

```

```yaml
task:
pipeline:
  - name: 'HFTokenizer'
    model_name: 'bert-base-uncased'
  - name: 'HFClassifier'
    model_name: 'bert-base-uncased'

``` 
## <div >Parsing </div>

To parse the text, run the following command

    $ ruth parse -m path/to/model_dir 
      -t "I want to book a flight from delhi to mumbai"

Parameters

    -m, --model_path  model file (optional)
    -t, --text  text message (required)


If model path is not provided, Parse function will use the latest model in the model directory as a default model.

## <div >Testing </div>

To test the model performance, run the following command

    $ ruth evaluate -p path/to/pipeline-basic.yml 
      -d path/to/dataset

Parameters 

    -p, --pipeline  pipeline file 
    -d, --data  dataset file
    -o, --output_folder  to save result as PNG file (optional)
    -m, --model_path (optionol)

If model path is not provided, `Evaluate function` will use the latest model in the model directory as a default model. If output folder is not provided, the result will be saved in `results` folder in the current working directory.

### <div >Deployment </div>
RUTH uses FastAPI to deploy the model as a REST API, to deploy the model, run the following command

    $ ruth deploy -m path/to/model_dir
Parameters

    -m, --model_path  model file (required)
    -p, --port port number (optional)
    -h, --host host name (optional)

Output

```bash
INFO:     Started server process [1]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://localhost:5500 (Press CTRL+C to quit)
```

## <div >API </div>

Once the model is deployed, you can use the following API to parse the text

    POST /parse
    {
        "text": "I want to book a flight from delhi to mumbai"
    }

Output

    {
        "text": "hello ruth!",
        "intent_ranking": [
            {
                "name": "greet",
                "accuracy": 0.9843385815620422
            },
            {
                "name": "how_are_you",
                "accuracy": 0.0017248070798814297
            },
            {
                "name": "voice_mail",
                "accuracy": 0.0008955258526839316
            },
        ],
        "intent": {
            "name": "greet",
            "accuracy": 0.9843385815620422
        }
    }

<!-- for social connects -->
##### <p>Connect us with </p>


[<img align="left" alt="Puretalk | LinkedIn" width="30px" src="https://img.icons8.com/color/48/000000/linkedin.png" />][linkedin][<img align="left" alt="Puretalk | Twitter" width="30px" src="https://img.icons8.com/fluent/48/000000/twitter.png" />][twitter][<img align ="left" alt="Sanjaypranav" width="30px" src="https://img.icons8.com/color/48/000000/gmail.png" />][gmail]


[linkedin]: https://www.linkedin.com/company/puretalkai/
[twitter]: https://twitter.com/puretalka
[gmail]: mailto:info@puretalk.ai
<br>
<br>
Devoloped by Puretalk@2022 
<br>
<!-- <img src="data/img/ruth_g.png" width=50px height='50px'> -->
