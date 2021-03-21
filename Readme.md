# Goal

Sync video playback with distance data

# POC

Distance data is pulled from a Performance Monitor 5 output on a Concept 2. The complete set of data is pulled before the application is run and stored in a text file.

POC is deployed to https://feddas.itch.io/swingpoc and https://github.com/swingmaster/swingmaster.github.io

# Video

Current sample video is https://www.youtube.com/watch?v=bkroMesEigI trimmed between timestamps 51.83 seconds and 2:51.83.

### Video format

Videos needs to be compressed for better web page speed
- Saved at 540p with Microsoft Video Editor
- Then compressed with https://www.freeconvert.com/video-compressor/download

### Web GL

Playing videos in WebGL requires loading from a url. This is done by leaveraging the StreamingAssets folder.

Some web browsers disable autoplay. If autoplay is disabled, a "Start" video button will be shown.

# Data

Data from Performance Monitor 5 is given in semi-colon delimited values.

ie `00:01:33.080;599.04;0;400.9;"00:01:49.200";0`

Column title | Data type | Description
--- | --- | ---
workTime | time string in ##:##:##.### | when that row of data is applied to the video since Unity scene start time. If multiple rows contain the same workTime, only the first one is used.
power | float | 
strokesPerMinute | int | 
distance | float | how far the participant has traveled
splitTime | double quote wrapped time string in ##:##:##.### | 
heartRate | NA |

# Data to video

The normal playback speed (x1) of the video is assumed to be 1 unit of data distance over 1 second. However, as this doesn't seem to match with the data the Performance Monitor 5 gives, time is scaled with distancePerSecond to 1/4. 

### Extrapolated values
1. `diffDistance` = distance - lastDistance
2. `diffTime` = workTime - lastWorkTime
3. `distancePerSecond` is set to 1/4. It maps distance in workout data to seconds of video
4. `playbackSpeed` = diffDistance / diffTime * distancePerSecond

### Examples
- speed=1 distancePerSecond=1
  - at 0s first distance is 0
  - at 1s second distance is 1
  - playback speed = 1 / 1 * 1 = 1

- speed=2 distancePerSecond=1
  - at 0s first distance is 0
  - at 1s second distance is 2
  - playback speed = 2 / 1 * 1 = 2

- speed=1 distancePerSecond=2
  - playback speed = 1 / 1 * 2 = 2

# Dev Notes

This projects source code is split into a .git repository and a Large Files zip. (a rudementary LFS system). To install the source code for this project do the following:

1. Clone or copy the zip from https://github.com/swingmaster/swingpoc
2. Download the latest LfsSwingPoc#.#.#.zip from https://github.com/swingmaster/swingpoc/releases
3. Ths zip should contain an Assets folder and Packages folder, just like the clone of source code contains. Copy these folders from the zip into the same place you cloned the project. Overwrite existing files.

# Repositories

- Code: https://github.com/swingmaster/swingpoc.git
- Web: https://github.com/swingmaster/swingmaster.github.io.git
