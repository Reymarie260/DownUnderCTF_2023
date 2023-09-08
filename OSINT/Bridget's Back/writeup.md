#  DownUnderCTF 2023 - Bridget's Back
## Problem Statement
Bridget's travelling the globe and found this marvellous bridge, can you uncover where she took the photo from?

NOTE: Flag is case-insensitive and requires placing inside DUCTF{} wrapper! e.g DUCTF{a_b_c_d_example}

## Information

**Point Value**: 100 points

**Category**: OSINT

**Difficulty**: Beginner

## Solution
Downloaded image provided 'bridgetsback.jpg', and searched for 'File Info' of the image in the gallery. Nothing appeared there, so proceeded by going to upload the photo in Google Images. We obtained that the bridge in the photo is called the Golden Gate Bridge in San Francisco, CA. By searching other photos taken similarly from the same places, we get keywords as 'bay' and 'Vista Point'. When looking in Google Maps for Vista Points in San Francisco we get H. Dana Bowers Memorial Vista Point. Looked at the photos provided by the location and effectively, was the place from where the Bridget's photo was taken.
## Flag
DUCTF{H._Dana_Bowers_Memorial_Vista_Point}
