---
layout: default
title:  "raid-I-VIII"
date:   2022-04-13
---

## Raid I - VIII

**Description**: A ping came through on slack - one of our sock puppet accounts monitoring Raidforums has discovered a unique dataset being shared. It appears to be the removable SD card of a drone. Our drone expert in Helsinki is tied up with other work but said it’s important to note it does not appear to be the on-board storage SD. Investigators believe this may be tied to a counter-terrorism case, but need to confirm the details, can you help them fill in the gaps? 

**attachment**: "seized drone contents" in [Google Drive](https://drive.google.com/drive/folders/1_f0PLreUcap2YW33Yfmi_Hj1cVyd9u6w?usp=sharing) with following files:

```
seized drone contents
└── MISC
    ├── THM
    ├── XCODE
    ├── IDX
    │   ├── idx10
    │   ├── idx11
    │   ├── idx00
    │   └── idx01
    └── LOG
        ├── linux_log.log
        ├── camera_log.log
        └── flylog
            └── fc_log.log
```

The critical part to solve this series question was to find the right way to interpret the attachments. The following were my failed attempts:
- open .log in NotePad++ with different encodings, but still can't read them
- open in hex editor but can't find any hex signatures to tell the type of file
- try to use professional log viewer but still could not work

At last,I finally found some tips in [Mavic forum](https://mavicpilots.com/threads/logs-inside-the-sd-card-i-want-to-know.91756/) and figured out: change the file extension .log to .dat and dump them into an app called DatCon, convert them into a butch of files, among which there would be a  human-readable .csv file. Unfortunately for myself though, I only found this out in the last 2 hours of the competition and I made another mistake of focusing too much on the linux_log and fc_log files, while actually camera_log has the most important information, so I didn't get all the flags on time.

Later I find another useful website: [airdata.com](https://airdata.com). By uploading the .dat files to the website, then almost all the answers could be found in there.

And some useless information about those idx files in here: https://forum.dji.com/thread-155447-1-1.html


### Raid I - - They said data it couldn't be done

**Description**: First off, what is the serial number of the drone (not the Machine ID)? Flag format is FLAG{serial}

##### Solution:

Serial number can be found in the camera_log.log.txt, one of the output file of DatCon. The "Ac SN" is the one we should be looking for.
![airdata-sn](/assets/airdata-sn.png)


### Raid II - Moar Powah

**Description**: Smart batteries aren't 'dumb' like traditional LiPo's - there is a lot of data involved. It can also be used to track down criminals who've purchased batteries with the package - or separately. Can you please help us find the serial number of the battery? Flag format is FLAG{serial}

#### Solution:

Same method from question above, or from airdata, as shown in the "Internal Serial" in image below:

![airdata-battery](/assets/Airdata-battery.png)


### Raid III - Groundhog day

**Description**: "This is like the fourth time we've done this today..." "Yeahp, that's forensics for you. Lock in those numbers, the whole court case depends on it." Can you you provide the make, model and manufacture date of the drone? Flag format is FLAG{make-model-submodel-DD-MM-YYYY}

#### Solution:

By searching the serial number found in Raid I in [DJI Serial Decoder](http://tools.retroroms.info/), the model and manufacture data can be found.


### Raid IV - Geoguesser

**Description**: Okay great, got the details of the drone, now we need to know where these multiple flights occurred. There's a number of ways of doing this - but whatever works for you. What city and country was the drone flown in? Flag format is FLAG{city, country}

#### Solution:

From airdata, check the location that it can be seen it's from Melbourne, Australia.

![airdata-location](/assets/airdata-location.png)

### Raid V - Snap n Send it

**Description**: What is the date of the oldest recorded flight log? This can be useful to cross-reference with our drone detection systems, see if anything was registered in the air at that time. Flag format is FLAG{DD/MM/YYYY}

#### Solution:

From the log of the file, I though the date of oldest flight would be 31/10/2020, but somehow the accepted date was 30/10/2020.

![airdata-history](/assets/airdata-history.png)


### Raid VI - Send n Snap it

**Description**: What is the date of the most recent recorded flight log? This can be useful to cross-reference with recent CCTV, or even social media posts that may have caught them in the pic - or even released photos of the drone! Flag format is FLAG{DD/MM/YYYY}

#### Solution:

Similar to the flag above, the final answer was 15/05/2021.


### Raid VII - Ruffle My Rotors

**Description**: Got it - so we have the list of days the flights were involved. To check what the weather was that day, we tried pulling the data from the meteorologists...unfortunately, their expert told us "wind can travel in any direction depending on environmental objects". Damn it. Can you please read the data recorded to the drone about the wind direction of the most recent flight, and what the average direction was? Flag format is FLAG{direction}

#### Solution:

Clearly tell from the image below that the average wind direction was north:
![airdata-wind](/assets/Airdata-wind.png)

### Raid VIII - Mayday

**Description**: Thanks for all the information so far - however, we still need to know what happened in that final flight. Keep in mind, this might pop up in court, we need to be completely right here. We've come up with a few options, but obviously the only real way to know is from the data. A little OSINT wouldn't hurt either! Options: ran out of battery, entered no-fly-zone, caught in a tree, caught in powerlines, landed with no issue, crashed into the ground, downed with jammer, downed by hacking, drifted away by wind, critical system failure. Flag format is FLAG{the full text answer}

