# Nosocomephobia
Category: OSINT
exiftool, Morse from audio, reverse image search, archive digging

### Challenge
There is this bottle I found washed on the shore yesterday.. Within it contained a cassette which sounded like simple beeping? ... In addition, there was this picture within the bottle as well ... Can you please give the recording a listen and find when and from where the recording is from?

Flag format will be: spooky{(Location Name).(Year of Recording)} Example: spooky{big_ben.1832}

Files: picture_in_bottle.jpg (a black-and-white photo of a hillside town with a ruined castle) and cassetterecording.mp3 (2:46, mono).

### Solution
1. The photo
Metadata. The JPEG carried an embedded caption: Denbigh, Wales, from the northwest, circa 1885 (Hulton Archive / Getty Images). The "circa 1885" is the date of the photograph, not the recording.
Reverse image search of the same image gives the same answer: Denbigh, with the castle ruins on the hill in the background.

3. The audio
The MP3 has no tags at all, so the information is in the sound. A spectrum showed a pure tone at about 550 Hz, which is on-off keyed Morse.

Decoding it gives a 138-word message:

```HELLO?? HELLO? I HAVE NO RECOLLECTION OF WHERE I AM CURRENTLY... THE ONLY THING THAT I HAVE WITH ME, IS THIS PICTURE OF MY CITY ... EVERYONE HERE IS REFERRED TO AS A "PATIENT" ... WEEKLY EVENTS ... A SCREENING OF AN EDUCATIONAL FILM, IN ROOM MALE 2 ...```

The message did not contain a year.

3. Location
"My city" is the photo (Denbigh), and patients, doctors, shocks and wards describe a psychiatric hospital.

5. Year
The Archives Wales article Entertaining Patients in the North Wales Hospital describes regular cinema shows and includes a weekly bulletin of patient activities from 1966.

```
Flag: spooky{north_wales_hospital.1966}
```

### Takeaways
- Always run exiftool or check IPTC/XMP on images. This one had the town name in the caption.
- A pure single-frequency tone that keeps switching on and off is Morse.
- When the decoded message does not hold the answer, the content of the message is the next clue.
