

```
ooooo   ooooo   .oooooo.   oooooo   oooooo     oooo         ooooooooooooo   .oooooo.
`888'   `888'  d8P'  `Y8b   `888.    `888.     .8'          8'   888   `8  d8P'  `Y8b
 888     888  888      888   `888.   .8888.   .8'                888      888      888
 888ooooo888  888      888    `888  .8'`888. .8'                 888      888      888
 888     888  888      888     `888.8'  `888.8'     8888888      888      888      888
 888     888  `88b    d88'      `888'    `888'                   888      `88b    d88'
o888o   o888o  `Y8bood8P'        `8'      `8'                   o888o      `Y8bood8P'
```
A cheat sheet for common data journalism stuff. For details on installing these tools, [see how I work](http://mtdukes.com/work.html). Use `CMD` + `F` to search the page, or the jump menu below if you know what you're looking for.

### Jump to:
**Command line tools** [grep](https://github.com/mtdukes/how-to#grep) | [head/tail](https://github.com/mtdukes/how-to#headtail) | [ffmpeg](https://github.com/mtdukes/how-to#ffmpeg) | [pdftk](https://github.com/mtdukes/how-to#pdftk) | [esridump](https://github.com/mtdukes/how-to#esridump) | [wget](https://github.com/mtdukes/how-to#wget) | [file](https://github.com/mtdukes/how-to#file) | [sed](https://github.com/mtdukes/how-to#sed) | [wc](https://github.com/mtdukes/how-to#wc)

**R packages** [base]([scales](https://github.com/mtdukes/how-to#base) | [scales](https://github.com/mtdukes/how-to#scales) | [ggpmisc](https://github.com/mtdukes/how-to#ggpmisc) | [dplyr](https://github.com/mtdukes/how-to#dplyr)

**Math for journalists** [Rate comparisons](https://github.com/mtdukes/how-to#rate-comparisons) | [Odds ratios](https://github.com/mtdukes/how-to#odds-ratios)

**Browser tricks** [PDFs](https://github.com/mtdukes/how-to#pdfs) | [Video](https://github.com/mtdukes/how-to#video)

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

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

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

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

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

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## pdftk
A power tool for processing and converting PDF files.

### Combine files
```bash
pdftk *.pdf cat output all_documents.pdf
```
Combine all the PDF files in the present directory into a single file. *Note: check to make sure the capitalization of the filetype matches.*

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## esridump
[A targeted utility](https://github.com/openaddresses/pyesridump) to pull down geographic data from ESRI maps.

### Download a geojson
```bash
esri2geojson https://services.arcgis.com/iFBq2AW9XO0jYYF7/arcgis/rest/services/Covid19byZIPnew/FeatureServer/0 nc_zipDATE.geojson
```
Download data from the ESRI REST endpoint that powers the N.C. DHHS COVID map of cases by zip code and save it as a geojson file.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

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

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## file
Tool to do various file formatting things I think.

### Detect encoding of a file
```bash
file -I input.csv
```
Detect encoding of a file.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## sed
Tool to make substitutions in a text file (submitted by [Chris Alcantara](https://github.com/chrisalcantara)).

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

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## wc
A basic character counting utility for the command line.

### Count the number of lines in a file
```bash
wc -l < data_file.txt
```
The less-than flag excludes the file name from the results.

### Pipe the results of some data and count the lines
```bash
curl mtdukes.com --silent | wc -l
```

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)


# R packages

## base
The stripped down version of R has lots of built-in stuff worth using.

### Get unique values
```R
property_sales %>% 
  unique()
```
Generates a dataframe of unique rows across all fields.

### Get duplicate values
```R
wake_sales %>% 
  .[duplicated(.), ]
```
Generates a dataframe of duplicated rows, comparing all fields.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## scales
[A library](https://scales.r-lib.org/) to make scaling and labeling easier.

### Show figures as dollars
```R
vax_income %>%
  ggplot(aes(x = median_income, y=PctTotal)) +
  geom_point() +
  scale_x_continuous(labels = scales::dollar_format())
```
The `dollar_format` function shortcuts the annoying parsing issues.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## ggpmisc
[Miscellaneous extensions](https://exts.ggplot2.tidyverse.org/ggpmisc.html) to the ggplot package.

### Include a regression equation on your scatterplot
```R
vax_income %>%
  ggplot(aes(x = median_income, y = pct_total)) +
  geom_point() +
  geom_smooth(method = "lm", formula = y ~ x, show.legend = FALSE) +
  stat_poly_eq(aes(label = paste(..eq.label.., ..rr.label.., sep = "~~~")), 
               label.x.npc = "right", label.y.npc = 0.15,
               formula = y ~ x, parse = TRUE, size = 3)
```
The `stat_poly_eq` function lets you annotate the graph with a regression formula. *BONUS: [What's a good value for R-squared?](https://people.duke.edu/~rnau/rsquared.htm)*

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## dplyr
A ["grammar of data manipulation"](https://dplyr.tidyverse.org/) and part of the [tidyverse](https://www.tidyverse.org/) package.

### Get a random sample of rows
```R
nc_voters %>% 
  sample_n(10)
```
Specify the number of rows from the dataframe to return.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# Math for journalism
Formulas and concepts I always have to look up. For more, read [Numbers in the Newsroom](https://www.ire.org/product/numbers-in-the-newsroom-using-math-and-statistics-in-news-second-edition-e-version/) by Sarah Cohen.

## Rate comparisons
Calculating the rate of something happening in a subgroup and comparing it to another can help suss out disproportionate impact, especially when the groups are different sizes.

### Comparing two groups

> A school has 300 white and 120 Black students. Last year, 30 white
> students were suspended, and 20 Black students were suspended.

It may be enough to say that Black students made up 60% of suspensions while only making up 29% of the school. But you may want to put a finer point on the disparity. First, calculate the suspension rate for Black students
```R
20 / 120 = 0.17
```

Next, do the same for white students.
```R
30 / 300 = 0.1
```

Then you compare the two rates of suspension.
```R
0.167 / 0.1 = 1.7
``` 

So black students are suspended at about 1.7 times the rate of white students.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## Odds ratios
Compare the likelihood of something happening for one group compared to another ([h/t to Arianna Giorgi](https://twitter.com/ArianaNGiorgi/status/1068208108596588544)). Often used in medical contexts. For more, see [this worksheet from the University of Delaware](https://github.com/mtdukes/how-to/blob/main/documents/odds_ratios.pdf).

### Comparing two groups
Instead of calculating and comparing the *rate* of something happening in a subgroup, calculating an odds ratio means you have to look at how much more likely something is to happen *than not happen* within that subgroup. So:

> A class has 21 boys and 16 girls. On a recent test, 11 boys and 14
> girls passed.

First calculate the likelihood that boys passed the test vs. not passing the tests
```R
11 / 10 = 1.1
```

Do the same for the girls.
```R
14 / 2 = 7
```

Now you can compare the ratios.
```R
7 / 1.1 = 6.4
```
So girls are 6.4 times as likely to pass the test than boys.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# Browser tricks
Plugins, URL parameters and other neat stuff.

## PDFs
Tips and tricks for handling PDFs in a Web browser (like Chrome)

### Jump to a page
```
https://assets.avigilon.com/file_library/pdf/acc7/avigilon-player7-en.pdf#page=21
```
Pass the `page` number as a URL parameter to link directly to a page.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## Video
Shortcuts and other cool things that help navigate various online video players.

### Jump to a time in YouTube
```
https://youtu.be/UwVNkfCov1k?t=30
```
Add a `t` parameter to specify the jump point in seconds.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# Convenience files
Collections of commonly used lists and references in various data structures.

## U.S. states
-   [State names and postal code abbreviations, comma- and line-separated and text-qualified by single quotes](https://gist.github.com/mtdukes/e6636bb7e7423fbf0faf52134257d675)

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## N.C. counties

- [NC counties, comma- and line-separated and text-qualified by single quotes](https://gist.github.com/mtdukes/ed03837107a7e173a56b71cf7c785424)
- [NC counties and election/voter data codes, tab-delimited](https://gist.github.com/mtdukes/c3abc68866e884a7b0fa418e712b40c8)
- [NC counties and court codes, as tuples](https://gist.github.com/mtdukes/88e089e6dd08b12e57667dd7fe3b4305)
- [NC counties and court codes, tab-delimited](https://gist.github.com/mtdukes/a1689d5c678cc52b3efd6ac1a3409e70)

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)