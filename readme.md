

```
ooooo   ooooo   .oooooo.   oooooo   oooooo     oooo         ooooooooooooo   .oooooo.
`888'   `888'  d8P'  `Y8b   `888.    `888.     .8'          8'   888   `8  d8P'  `Y8b
 888     888  888      888   `888.   .8888.   .8'                888      888      888
 888ooooo888  888      888    `888  .8'`888. .8'                 888      888      888
 888     888  888      888     `888.8'  `888.8'     8888888      888      888      888
 888     888  `88b    d88'      `888'    `888'                   888      `88b    d88'
o888o   o888o  `Y8bood8P'        `8'      `8'                   o888o      `Y8bood8P'
```
A cheat sheet for common data journalism stuff. For details on installing these tools, [see how I work](http://mtdukes.com/work.html).

### Jump to:
**Command line tools** [grep](https://github.com/mtdukes/how-to#grep) | [head/tail](https://github.com/mtdukes/how-to#headtail) | [ffmpeg](https://github.com/mtdukes/how-to#ffmpeg) | [pdftk](https://github.com/mtdukes/how-to#pdftk) | [esridump](https://github.com/mtdukes/how-to#esridump) | [wget](https://github.com/mtdukes/how-to#wget) | [file](https://github.com/mtdukes/how-to#file) | [sed](https://github.com/mtdukes/how-to#sed)

**Convenience files** [U.S. states](https://github.com/mtdukes/how-to#us-states) | [N.C. counties](https://github.com/mtdukes/how-to#us-states)

# Command line tools
A collection of tips and tricks for working with tools executed using bash terminals.

## grep
Search text files for specific character sequences.

### Basic search

```bash
grep "DUKES" absentee.csv
```
Return lines containing a string from a specified file and print to the command line.

### Print to a file
```bash
grep "\"DUKES\",\"MICHAEL\",\"TYLER\"" absentee.csv > dukes.csv
```
Search for a string with quotes and output all lines to a file.

## head/tail
Get a preview of a file.

### See the top
```bash
head absentee.csv
```
Print the first 10 lines of a file to the command line.

### Specify the number of lines

```bash
head -1 absentee.csv > absentee_header.csv
```
Get the first line of a file and save it to a CSV file.

## ffmpeg
A power tool for processing and converting video and audio files.

### Make a GIF
```bash
ffmpeg -ss 5 -t 1.7 -i video.MOV -vf "fps=24,scale=640:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 video.gif
```
Create a high quality GIF 640 pixels wide at 24 frames per second using the specified video file, skipping 5 seconds and lasting 1.7 seconds and save the output.

### MOV to mp4
```bash
ffmpeg -i apple.mov -vcodec h264 -acodec aac apple.mp4
```
Use a codec flag to convert a video file from QuickTime to the more universal mp4 format.

### AVI to mp4
```bash
ffmpeg -i full_video.avi full_video.mp4
```
Convert an AVI file to the more universal mp4 format.

### Combine video clips
```bash
ffmpeg -f concat -safe 0 -i vidlist.txt -c copy full_video.avi
```
Combine all of the files recorded in a text file called `vidlist.txt`, which looks like this:
```
file '/Users/username/directory/vid_seq001.avi'
file '/Users/username/directory/vid_seq002.avi'
```
## pdftk
A power tool for processing and converting PDF files.

### Combine files
```bash
pdftk *.pdf cat output all_documents.pdf
```
Combine all the PDF files in the present directory into a single file. *Note: check to make sure the capitalization of the filetype matches.*

## esridump
[A targeted utility](https://github.com/openaddresses/pyesridump) to pull down geographic data from ESRI maps.

### Download a geojson
```bash
esri2geojson https://services.arcgis.com/iFBq2AW9XO0jYYF7/arcgis/rest/services/Covid19byZIPnew/FeatureServer/0 nc_zipDATE.geojson
```
Download data from the ESRI REST endpoint that powers the N.C. DHHS COVID map of cases by zip code and save it as a geojson file.

## wget
A power tool for recursively downloading files, for example from the Web.

### Download a single file
```bash
wget http://mtdukes.com/work.html
```
Saves the file from the specified URL.

### Download a list of files
```bash
wget -i file_list.txt
```
Saves individual files from URLs specified in a TXT file, one URL on each line.

### Download a directory
```bash
wget --recursive --no-parent http://mtdukes.com
```
Recursively download an entire site's contents.

## file
Tool to do various file formatting things I think.

### Detect encoding of a file
```bash
file -I input.csv
```
Detect encoding of a file.

## sed
Tool to make substitutions in a text file.

### Replace all instances of a word and output result to new file
```bash
sed "s/dook/duke/g" ./old.csv > new.csv
```
[Syntax](https://www.tutorialspoint.com/unix/unix-regular-expressions.htm) uses `/` as a delimiter to separate patterns you want to substitute.

### Replace all instance of a word within the file
```bash
sed -i "" "s/dook/duke/g" ./data.csv
```
Substitute directly in the file by passing an empty string after the `-i` flag.

### Pass a file name to make a backup.

```bash
sed -i "./data-backup.csv" "s/dook/duke/g" ./data.csv
```

# Browser tricks
Plugins, URL parameters and other neat stuff.

## PDFs
Tips and tricks for handling PDFs in a Web browser (like Chrome)

### Jump to a page
```
https://assets.avigilon.com/file_library/pdf/acc7/avigilon-player7-en.pdf#page=21
```
Pass the `page` number as a URL parameter to link directly to a page.

## Video
Shortcuts and other cool things that help navigate various online video players.

### Jump to a time in YouTube
```
https://youtu.be/UwVNkfCov1k?t=30
```
Add a `t` parameter to specify the jump point in seconds.

# Convenience files
Collections of commonly used lists and references in various data structures.

## U.S. states
-   [State names and postal code abbreviations, comma- and line-separated and text-qualified by single quotes](https://gist.github.com/mtdukes/e6636bb7e7423fbf0faf52134257d675)

## N.C. counties

- [NC counties, comma- and line-separated and text-qualified by single quotes](https://gist.github.com/mtdukes/ed03837107a7e173a56b71cf7c785424)
- [NC counties and election/voter data codes, tab-delimited](https://gist.github.com/mtdukes/c3abc68866e884a7b0fa418e712b40c8)
- [NC counties and court codes, as tuples](https://gist.github.com/mtdukes/88e089e6dd08b12e57667dd7fe3b4305)
- [NC counties and court codes, tab-delimited](https://gist.github.com/mtdukes/a1689d5c678cc52b3efd6ac1a3409e70)
