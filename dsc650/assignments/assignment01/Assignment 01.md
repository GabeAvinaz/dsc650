---
title: Assignment 1
subtitle: Computer performance, reliability, and scalability calculation
author: Gabriel Avinaz
---

## 1.2 

#### a. Data Sizes

| Data Item                                  | Size per Item | 
|--------------------------------------------|--------------:|
| 128 character message.                     | 150 Bytes     |
| 1024x768 PNG image                         | 454.66 KB     |
| 1024x768 RAW image                         | 1.37 MB       | 
| HD (1080p) HEVC Video (15 minutes)         | 380 MB        |
| HD (1080p) Uncompressed Video (15 minutes) | 209.95 GB     |
| 4K UHD HEVC Video (15 minutes)             | 1507.5 MB     |
| 4k UHD Uncompressed Video (15 minutes)     | 671.85 GB     |
| Human Genome (Uncompressed)                | 200 GB        |

For the character message I created a JSON file to measure the size if it contained a 128 character string, to account for any overhead the file creation would add.

The sizes for the various image and video file types vary greatly depending on a number of factors, so I couldn't really calculate them using a standard laid out by W3.  I ended up using an estimator, found through google to ett a rough estimate of how the file sizes would look.

https://toolstud.io/photo/filesize.php

The PNG image was an estimate for a compressed 24bit lossless image, and the RAW was a uncompressed Nikon 14 bit NEF file.

Video had a similar issue where bitrates and compression play a huge factor on the video's file size, so I tried to use the same method to get a rough estimate.  I used the same site for the uncompressed video formats, but had to find an alternative for HEVC estimates as it seams the size varies so much, it's difficutl to get an answer.

https://toolstud.io/video/filesize.php
https://www.seagate.com/video-storage-calculator/

For the human genome, I found an article on medium discusssing this.  While a perfect genome would only take up abou 700 MB, all the sequencing required to work with it brings the storage information up to about 200 GB.

https://medium.com/precision-medicine/how-big-is-the-human-genome-e90caa3409b0

#### b. Scaling

|                                           |  Size      |   # HD  | 
|-------------------------------------------|-----------:|--------:| 
| Daily Twitter Tweets (Uncompressed)       |  75 GB     |       3 |
| Daily Twitter Tweets (Snappy Compressed)  |  11.25 GB  |       3 |
| Daily Instagram Photos                    |  34.1 TB   |      12 |
| Daily YouTube Videos                      |  1.09 PB   |     327 |
| Yearly Twitter Tweets (Uncompressed)      |  27.39 TB  |       9 |
| Yearly Twitter Tweets (Snappy Compressed) |  4.109 TB  |       3 |
| Yearly Instagram Photos                   |  12.5 PB   |    3750 |
| Yearly YouTube Videos                     |  398.12 PB | 119,436 |

Daily Twitter tweets would be 150 bytes times 500,000,000: 75 billion bytes or 75 GB.
Snappy's compression ration of JSON file seems to be about 85% according to linked site, so we'll be using that for our estimate.
15% of 75 GB is 11.25 GB. then both are multiplied by 365.25 to get the yearly rate.

https://www.adaltas.com/en/2021/03/22/performance-comparison-of-file-formats/

firstly 75% of 100 million is 75 million, we can just multiply that by our estimate for PNG images and get 34,099,500,000 KB of storage, or 34.1 TB. doing the same for our yearly rate, we get 12,455.025 TB simplified to 12.5 PB

to get our daily YouTube size, we'll need to multiply our 15 minute estimate by 4 to get an hourly size. then calculate how many hours of video are uploaded daily.

380 MB * 4 is 1,520 MB
500 * 60 minutes * 24 hours is 720,000 hours daily
1,520 MB * 720,000 gets us 1,094,400,000 MB reduced to 1.09 PB

again multiplying by 365.25 gets us 398.12 PB

For the drives required, we can divide everythign by 10 TB, round up to the nearest drive and then triple the required number to compensate for HDFS redundancy. 

#### c. Reliability
|                                    |  # HD  | # Failures  |
|------------------------------------|-------:|------------:|
| Twitter Tweets (Uncompressed)      |      9 | 33.57% of 1 |
| Twitter Tweets (Snappy Compressed) |      3 | 11.19% of 1 |
| Instagram Photos                   |   3750 |         140 |
| YouTube Videos                     | 119436 |        4455 |


According to Backblaze, the average failure rate of a 10TB drive is 3.73% meaning that we can expect that many of the drives to fail during the year.
our tweets have a 33.57% chance of a single drive failing uncompressed and 11.19% compressed so I uncluded these percentages instead of the number of drives, even though we should expect at least a single failure for good practice. The rest will also be rounded up the nearest drive.



#### d. Latency

|                           | One Way Latency      |
|---------------------------|---------------------:|
| Los Angeles to Amsterdam  | 29.82 ms             |
| Low Earth Orbit Satellite | 3.34 ms              |
| Geostationary Satellite   | 120.1 ms             |
| Earth to the Moon         | 1209.82 ms           |
| Earth to Mars             | 10.84 minutes        | 


The calculation for these will be done assuming radio transmission as the media for delivery.  in a vaccum it travels at the speed of light, which is 299,792 km/s
The Distance between Amsterdam and Los Angeles is 8,938.60 km.
Low Earth Orbit is about 1,000 km
Geostationary orbit is about 36,000 km
The moon is currently about 362,696 km
Mars is currently about 195,124,782 km

for each we find the number of seconds it takes for light to travel that distance.

https://www.omniaccess.com/leo/
https://www.timeanddate.com/astronomy/moon/distance.html
https://theskylive.com/how-far-is-mars