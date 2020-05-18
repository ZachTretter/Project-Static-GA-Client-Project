## Problem Statement

Prompt : Using live police radio reports for real time identification of people needing assistance.

“Currently, FEMA identifies areas that require immediate attention (for search and rescue efforts) either by responding to reports and requests put directly by the public or, recently, using social media posts. This tool will utilize live police radio reports to identify hot spots representing locations of people who need immediate attention. The tool will flag neighborhoods or specific streets where the police and first-respondents were called to provide assistance related to the event.”

------------

## Data Science Questions

Is it feasible to build a tool that pulls in live audio data from a police scanner and outputs locations of emergencies on a map?
* What geographic and situational information is provided by (live) police radio?
* How can this information be displayed on a map?

-------------

## Conclusions

#### Limitations in Raw Data Impact Utility of an Automated Speech-to-Text-to-Location Tool

Police radio provides some geographic information
* Translating radio to discrete information inhibited by
  * Poor audio quality of raw feed
  * Inherent limitations of speech-to-text tools
* Mapping of geographic information in radio transcript obfuscated by
  * Unintelligibility of police chatter to layman
  * Ambiguity of street names in an urban area
  * Uncertainty of precise location even with a clear address

-------------

## Table of Contents

#### Code Folder

|File Name|Role|Scope|
|:-------------------------|:-------------------------|:-------------------------|
|00_Scrape_Streets.ipynb|Data Collection|Scrape greater Boston area street names|
|01_Broadcastify_Audio_Download_FINAL.ipynb|Data Collection|Pull historic police scanner audio files from the 'Broadcastify' archives|
|02_audio_cleaning_functions_FINAL.ipynb|Audio Processing|Run the Broadcastify audio files through the Dolby sound cleaning API|
|03_split_audio_FINAL.ipynb|Audio Processing|Split the cleaned Broadcatify audio files on silence leaving only portions with an audible voice|
|04_Speech_to_Text_via_googleAPI_FINAL.ipynb|Audio Processing|Transcribe the cleaned and split Broadcastify audio files into text using Google's speech to text API|
|05_Identify_Addresses_in_Transcript_FINAL.ipynb|Natural Language Processing|Identify possible addresses from the transcribed text|
|06_Geocode_Mapping_FINAL.ipynb|Geocoding & Mapping|Geocode addresses and map them|


--------------

#### Datasets Folder

|File Name|Type|Content|Product of Workbook #|Used in Workbook #|
|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|
|ancillary_csv|Folder|Scraped street names and police codes|0|5|
|enchanced_audio|Folder|Enhanced audio files|2|3|
|geocodes|Folder|Geocodes Boston and Watertown transcript addresses|6|6|
|raw_audio|Folder|Raw audio files from Broadcastify archives|1|2|
|split_audio|Folder|Processed audio files|3|4|
|transcripts|Folder|Voice to text transcripts|4|5|
|dataframe_final.csv|csv|Dataframe with transcript and addresses|5|6|

--------------

## Data and Process Overview

Data used in this analysis was aquired from two sources: 1) Broadcastify audio archive and 2) geographic.org.

36 hours of police scanner audio files were acquired from Broadcstify via the 'BroadcastifyArchive API'. All street names for the greater Boston area were scraped from 'geographic.org' using 'bs4' 'BeautifulSoup' library.

We found that the raw audio files were rough had a lot of static noise required cleaning. The raw audio files were cleaned by running them through 'Dolby's' audio cleaning API.  The cleaned audio files were then split on silence leaving only portions with an audible voice using the 'pydub' 'split_on_silence' audio processing library. We then ran the cleaned and split audio files through 'Google Cloud' 'speech_v1p1beta1' speech to text function which outputs transcribed text.

The transcribed text was then processed using 'NLTK' 'RegexpTokenizer' to break the text up into individual words. These words were then cross referenced with the list of greater Boston area street names using 'spacy' 'Matcher' function to determine if any of the words from the transcript matched street names. The transcript was then processed again to pull out possible street numbers using the 'usaddress' library. The list of possible street names ('Hanover') and the list of possible street numbers ('213') were concatenated to create a list of possible full addresses (213 Hanover, etc.).

The full street names pulled from the transcribed audio were then transformed into longitudinal and latitudinal coordinates and mapped using the 'folium' library.

--------------

## Required Software

- requests
- bs4: BeautifulSoup
- time
- numpy
- pandas
- broadcastify_archtk: BroadcastifyArchive
- datetime: datetime
- os
- boto3
- AWS
- time
- pydub: AudioSegment, split_on_silence
- seaborn
- google.cloud: speech_v1p1betal, enums
- io
- import spacy
- re
- spacy: displacy, LOWER, Matcher
- collections: Counter
- usaddress
- nltk: RegexpTokenizer
- en_core_web_sm
- folium
