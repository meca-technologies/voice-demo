<!-- version tag -->

 
RUTH TTS Benchmark, accuracy and voice quality test.
---
Objective metrics such as the Signal-to-Noise Ratio (SNR), the Mean Opinion Score (MOS), and the Perceptual Evaluation of Speech Quality (PESQ) are used to measure the quality of the audio.


# <div align="center">Full documentation</div>


# Metrics

To compute these metrics, we use Python libraries such as pydub and pesq. pydub is a simple and easy-to-use audio manipulation library that can be used to extract and process audio data. pesq is a Python implementation of the ITU-T P.862 PESQ algorithm, which is a widely used objective metric for evaluating speech quality.

we used the MUSAN dataset, which provides a diverse set of audio signals for evaluation, including music, speech, and noise. 

<p>Other ethods, libraries & software used to compare voice quality: <a href="https://pypi.org/project/tts-scores/"> tts-scores</a> by James B. <br> https://www.nist.gov/itl/iad/mig/tools by National Institute of Standards and Technology.</p>

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

   
   
   

2- Save the code to a Python file, e.g. tts_benchmark.py

3- Place the input audio files (from various TTS Vendors) (vendor1_audio.wav, vendor2_audio.wav, and vendor3_audio.wav) and the noise file (noise.wav) in the same directory as the Python file.

4- Open a terminal or command prompt in the directory where the Python file and audio files are located.

5- Run the Python file using the following command:
 
 
 
   
    python tts_benchmark.py


The script will output the results of the benchmark tests for each TTS engine, including text-to-speech latency, word error rate (WER), voice activity detection (VAD), SNR, MOS, and PESQ. The final output will be the pairwise cosine similarity matrix between the audio files generated by each vendor.

Note that the script assumes that the audio files are in WAV format and have a sample rate of 16 kHz for VAD and SNR testing, and a sample rate of 16 kHz for MOS and PESQ computation. If your audio files have different formats or sample rates, you will need to modify the script accordingly.

Additionally, the script assumes that the noise file has a sample rate of 44.1 kHz. If your noise file has a different sample rate, you will need to modify the script to resample the noise file to 16 kHz before computing SNR.

*To use pre-processed audio - Please look for the folder raw.githubusercontent.com/meca/meca.github.io/main
/voice_clips/


* replace audio file name in audio_files


NB: In general, the fundamental frequency of the complex speech tone – also known as the pitch or f0 – lies in the range of 100-120 Hz for men, but variations outside this range can occur. The f0 for women is found approximately one octave higher. For children, f0 is around 300 Hz. (The spectrum of speech - DPA Microphone)



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
