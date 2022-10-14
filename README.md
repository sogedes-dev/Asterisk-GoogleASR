# Asterisk-GoogleASR
Real-Time Google Speech Recognition on Asterisk -- with python EAGI script

### How to use

Install dependencies on your Asterisk machine

```
- sudo apt install python3-pip
- pip install pyst2
- pip install google-cloud-speech
- pip install numpy
- copy streaming.eagi under /var/lib/asterisk/agi-bin/ 
- chmod +x streaming.eagi
- put the credentials.json file from Google Cloud under /var/lib/asterisk/agi-bin/google-cloud-credentials.json
```

These are the argv expected by the EAGI script in this particular order:

```
- model ("default", "enhanced" or "phone_call"). Mandatory field
- mainLanguage (e.g. "en-US"). Mandatory field
- alternativeLanguages (e.g. "de-DE, pt-BR"). Optional array field. Does not work with "phone_call" model.
```

Example using Asterisk dialplan, where the default model is used, en-US is the main language and de-DE and pt-BR are alternative languages:

```
exten => 1234,1,Answer()   
exten => 1234,n,eagi(streaming.eagi,default,en-US,de-DE,pt-BR)   
exten => 1234,n,Verbose(1,The text you just said is: ${TRANSCRIPT})   
exten => 1234,n,Hangup()   

```

ASR Streaming example: https://cloud.google.com/speech-to-text/docs/streaming-recognize 


### EXTRA - Google Text to Speech -- with python AGI script


Install dependencies on your Asterisk machine

```
- sudo apt install python3-pip
- pip install pyst2
- pip install soundfile
- pip install numpy
- copy tts-google.agi under /var/lib/asterisk/agi-bin/ 
- chmod +x tts-google.agi 
- Insert your Google API Key in the source file
```

TTS example: https://cloud.google.com/text-to-speech/docs/reference/rest/v1beta1/text/synthesize

```
These are the argv expected by the TTS AGI script in this particular order:

- languageCode
- gender (can be MALE, FEMALE or NEUTRAL)
- text
```

Example using Asterisk dialplan, where en-US is the language code, MALE is the gender and "If you liked, give a star to this repo!" is the text to be spoken:

```
exten => 1234,1,Answer()   
exten => 1234,n,agi(tts-google.agi,en-US,MALE,"If you liked, give a star to this repo!")   
exten => 1234,n,Hangup()   

```

Improvements: implement voice variations, pitch, speed etc...