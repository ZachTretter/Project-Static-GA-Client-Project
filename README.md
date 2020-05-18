## Problem Statement

In an emergency event like a hurricane or a terrorist attack having the ability to identify hot spots where people are most affected could help first responders best allocate their resources. A visual tool that maps these affected areas could be very useful.


## Data Science Question

Is it feasible to build a tool that pulls in live audio data from a police scanner and outputs locations of emergencies on a map?

## Table of Contents

#### Code Folder

- **00_Scrape_Streets.ipynb** - Data collection. Code to scrape greater Boston area street names from 'geographic.org'
- **01_Broadcastify_Audio_Download_FINAL.ipynb** - Data Collection. Code to pull historic police scanner audio files from the 'Broadcastify' archives.
- **02_audio_cleaning_functions_FINAL.ipynb** - Audio Processing. Code to run the Broadcastify audio files through the Dolby sound cleaning API.
- **03_split_audio_FINAL.ipynb** - Audio Processing. Code to split the cleaned Broadcatify audio files on silence leaving only portions with an audible voice.
- **04_Speech_to_Text_via_googleAPI_FINAL.ipynb** - Audio Processing. Code to transcribe the cleaned and split Broadcastify audio files into text using Google's speech to text API.
- **05_Identify_Addresses_in_Transcript_FINAL.ipynb** - NLP. Code to pull possible addresses from the transcribed text.
- **06_Geocode_Mapping_FINAL.ipynb** - Mapping. Code to take addresses from NLP and map them using longitude and latitude.

#### Datasets Folder

- **ancillary_csv** - Scraped street names and police codes
- **enhanced_audio** - Enhanced audio files
- **geocodes** - Latitude and Longitude data for Boston and Watertown
- **raw_audio** - Raw audio files from Broadcastify archives
- **split_audio** - Processed audio files
- **transcripts** - Voice to text transcripts

## Data and Process Overview

Data used in this analysis was aquired from two sources: 1) Broadcastify audio archive and 2) geographic.org.

36 hours of police scanner audio files were acquired from Broadcstify via the 'BroadcastifyArchive API'. All street names for the greater Boston area were scraped from 'geographic.org' using 'bs4' 'BeautifulSoup' library.

We found that the raw audio files were rough had a lot of static noise required cleaning. The raw audio files were cleaned by running them through 'Dolby's' audio cleaning API.  The cleaned audio files were then split on silence leaving only portions with an audible voice using the 'pydub' 'split_on_silence' audio processing library. We then ran the cleaned and split audio files through 'Google Cloud' 'speech_v1p1beta1' speech to text function which outputs transcribed text.

The transcribed text was then processed using 'NLTK' 'RegexpTokenizer' to break the text up into individual words. These words were then cross referenced with the list of greater Boston area street names using 'spacy' 'Matcher' function to determine if any of the words from the transcript matched street names. The transcript was then processed again to pull out possible street numbers using the 'usaddress' library. The list of possible street names ('Hanover') and the list of possible street numbers ('213') were concatenated to create a list of possible full addresses (213 Hanover, etc.).

The full street names pulled from the transcribed audio were then transformed into longitudinal and latitudinal coordinates and mapped using the 'folium' library.


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
