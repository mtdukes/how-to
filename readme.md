
```
ooooo   ooooo   .oooooo.   oooooo   oooooo     oooo         ooooooooooooo   .oooooo.
`888'   `888'  d8P'  `Y8b   `888.    `888.     .8'          8'   888   `8  d8P'  `Y8b
 888     888  888      888   `888.   .8888.   .8'                888      888      888
 888ooooo888  888      888    `888  .8'`888. .8'                 888      888      888
 888     888  888      888     `888.8'  `888.8'     8888888      888      888      888
 888     888  `88b    d88'      `888'    `888'                   888      `88b    d88'
o888o   o888o  `Y8bood8P'        `8'      `8'                   o888o      `Y8bood8P'
```
A cheat sheet for common data journalism stuff. For details on installing these tools, [see how I work](http://mtdukes.com/work.html). Use <kbd>CMD</kbd> + <kbd>F</kbd> to search the page, or the jump menu below if you know what you're looking for.

### Jump to:
**Command line tools** [grep](https://github.com/mtdukes/how-to#grep) | [head/tail](https://github.com/mtdukes/how-to#headtail) | [ffmpeg](https://github.com/mtdukes/how-to#ffmpeg) | [pdftk](https://github.com/mtdukes/how-to#pdftk) | [esridump](https://github.com/mtdukes/how-to#esridump) | [wget](https://github.com/mtdukes/how-to#wget) | [file](https://github.com/mtdukes/how-to#file) | [sed](https://github.com/mtdukes/how-to#sed) | [wc](https://github.com/mtdukes/how-to#wc) | [imagemagick](https://github.com/mtdukes/how-to#imagemagick) | [libpst](https://github.com/mtdukes/how-to#libpst) | [gunzip](https://github.com/mtdukes/how-to#gunzip) | [sysctl](https://github.com/mtdukes/how-to#sysctl)

**R packages** [shortcut keys](https://github.com/mtdukes/how-to#shortcut-keys) | [base](https://github.com/mtdukes/how-to#base) | [readr](https://github.com/mtdukes/how-to#readr) | [scales](https://github.com/mtdukes/how-to#scales) | [ggpmisc](https://github.com/mtdukes/how-to#ggpmisc) | [dplyr](https://github.com/mtdukes/how-to#dplyr) | [plyr](https://github.com/mtdukes/how-to#plyr) | [clipr](https://github.com/mtdukes/how-to#clipr) | [googlesheets4](https://github.com/mtdukes/how-to#googlesheets4)

**Math for journalists** [Standard deviation](https://github.com/mtdukes/how-to#standard-deviation) | [Rate comparisons](https://github.com/mtdukes/how-to#rate-comparisons) | [Odds ratios](https://github.com/mtdukes/how-to#odds-ratios) | [Making sense of symbols](https://github.com/mtdukes/how-to#making-sense-of-symbols)

**GIS tips** [Latitude and longitude](https://github.com/mtdukes/how-to#latitude-and-longitude) | [NC bounding box](https://github.com/mtdukes/how-to#nc-bounding-box)

**Browser tricks** [PDFs](https://github.com/mtdukes/how-to#pdfs) | [Video](https://github.com/mtdukes/how-to#video)

**Troubleshooting** [Location errors](https://github.com/mtdukes/how-to#location-errors)

**Convenience files** [U.S. states](https://github.com/mtdukes/how-to#us-states) | [N.C. places](https://github.com/mtdukes/how-to#nc-places)

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

### Recursively search a directory of files, first line only
```bash
head -1 ./*/*|grep -B1 'Hospital overall rating' > variable.txt
```
Combining `head` and `grep` with a pipe allows you to chain commands, and the `-B1` flag allows you to output the file name.

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

### Caption the GIF
```bash
ffmpeg -ss 278.8 -t 3.3 -i wings.mp4 -filter_complex "fps=24,scale=640:-1:flags=lanczos,drawtext=box=1:boxcolor=black@0.4:boxborderw=5:fontfile=/System/Library/Fonts/Supplemental/Impact.ttf:text='CONTRARY TO MY APPEARANCE,':fontsize=48:fontcolor=white:x=(w-tw)/2:y=(h/PHI)+th,drawtext=box=1:boxcolor=black@0.4:boxborderw=5:fontfile=/System/Library/Fonts/Supplemental/Impact.ttf:text='I AM ENJOYING THIS.':fontsize=48:fontcolor=white:x=(w-tw)/2:y=(h/PHI)+th+50,split[x1][x2];[x1]palettegen[p];[x2][p]paletteuse" wings.gif
```
Tweak the parameters (or delete the second line) to adjust the font, text etc. of the caption.

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

### Create a video from a sequence of images
```bash
ffmpeg -r 1/5 -i img%03d.jpg -c:v libx264 -vf "fps=25,format=yuv420p" out.mp4
```
Read in a sequence of images from a folder and write to an mp4 file. The `-r` flag is the framerate, where the duration of each image is the inverse of the provided value (e.g. 1/5 is 5 seconds, 60 is 1/60 of a second). The `-i` flag specifies the filename structure, with 0 padding specified (e.g.  img%03d.jpg will iterate through img001.jpg, img002.jpg, img003.jpg etc.). [More details here](https://hamelot.io/visualization/using-ffmpeg-to-convert-a-set-of-images-into-a-video/).

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## pdftk
A power tool for processing and converting PDF files.

### Combine files
```bash
pdftk *.pdf cat output all_documents.pdf
pdftk doc01.pdf doc02.pdf cat output all_documents.pdf
```
Combine all the PDF files in the present directory into a single file. Or specify individual files *Note: check to make sure the capitalization of the filetype matches.*

### Split files by page number
```bash
pdftk blue_docs.pdf cat 1-700 output blue_docs01.pdf
pdftk blue_docs.pdf cat 701-end output blue_docs02.pdf
````
Specify the page number or use the `end` keyword to slice up a document.

### Split a PDF portfolio
```bash
pdftk doj_emails_portfolio.pdf unpack_files output doj_emails
```
PDF portfolios contain a bunch of individual files bound up in a filetype that needs a native PDF reader. Get around this by unpacking each file into a specific directory.

### Split a PDF portfolio with attachments
```bash
pdftk doj_emails_portfolio.pdf unpack_files output doj_emails;
IFS=$'\n'; set -f
for f in $(find ./doj_emails/ -name '*.pdf'); do pdftk "$f" unpack_files output ./doj_emails/; done
unset IFS; set +f
```
If your PDF portfolio has attachements within the individual PDF, you can use your terminal to unpack the portfolio into a directory, then set up a loop to unpack all of the PDFs in that directory on by one (Thanks to this [Stackoverflow thread](https://stackoverflow.com/questions/4638874/how-to-loop-through-a-directory-recursively-to-delete-files-with-certain-extensi) for tips on bash recursion and dealing with spaces in filenames.)

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
The `<` flag excludes the file name from the results.

### Pipe the results of some data and count the lines
```bash
curl mtdukes.com --silent | wc -l
```

## imagemagick
[A power tool](https://imagemagick.org/index.php) for quickly editing images.

### Batch crop a folder of images
```bash
mogrify -crop 800x450+0+40 -path ./cropped *.jpg
```
In a folder of images, crop every jpg image at size 800x450, with a 0px offset from the left (x) and a 40px offset from the top (y).

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## libpst
A [collection of tools](https://www.five-ten-sg.com/libpst/) to work with and convert PST files (Outlook folders containing email). On Macs, [install with homebrew](https://formulae.brew.sh/formula/libpst).

### Convert PST to MBOX
```bash
readpst public_records.pst
```
Outputs a single file in mbox format, which is a more open format you can import into a number of email clients.

### Convert PST to individual email files
```bash
readpst -e -D public_records.pst
```
Separates the PST into individual eml files. Attachments are embedded in the file. The `-D` flag preserves deleted items. Can be read by services like Google's Pinpoint.

### Convert PST to rich text files and export attachments
```bash
readpst -S -D public_records.pst
```
Separates the PST into individual eml files, each emails rich text body and individual attachment. The numbered files are in eml format, with no extension. The `-D` flag preserves deleted items.

### Convert PST to eml and msg files
```bash
readpst -m -D public_records.pst
```
Produces both msg and eml files for each message.  The `-D` flag preserves deleted items.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## gunzip
Quickly and efficiently unzip files (or a folder full of files). Can also unzip some files where a normal unzipping GUI throws errors. Useful for `.gz` file extensions.

### Unzip a single file
```bash
gunzip map_file.gz
```
Unzips a specific file and replaces it with the unzipped version.

### Keep the original zipped file
```bash
gunzip -c map_file.gz > map_file.shp
```
Uses the `stdout` flag to read to the console, but pipe to a new file to keep the original.

### Unzip a folder full of files
```bash
gunzip -r /map_files
```
Uses the recursive flag to iterate through every zipped file in a folder and replace it with the unzipped version.

## sysctl
A suite of tools to anaylze your system (for Mac).

### Examine CPU threads
```bash
sysctl hw.physicalcpu hw.logicalcpu
```
Provides an output of physical and logical cores [your CPU has](https://superuser.com/questions/1101311/how-many-cores-does-my-mac-have).

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# R packages

## shortcut keys
A few common shortcuts save you from typing in RStudio.

### Start a new section
<kbd>Command</kbd> + <kbd>Shift</kbd> + <kbd>R</kbd>

Prompt for a new label used in the document outline for an R script

### Execute a command
<kbd>Command</kbd> + <kbd>Enter</kbd>

Run a section of code in your R script.

### Use a pipe
<kbd>Command</kbd> + <kbd>Shift</kbd> + <kbd>M</kbd>

Input a `%>%` at your cursor to pipe output to the next line with the `dplyr` library.

### Use an assignment
<kbd>Option</kbd> + <kbd>-</kbd>

Input a `<-` at your cursor to assign output to a variable.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## base
The stripped down version of R has lots of built-in stuff worth using.

### Clear all environment variables
```R
rm(list = ls())
```
Start with a clean slate using the `rm` command.

### Set your working directory
```R
setwd('~/projects/newsobserver/big_investigation/data')
```
Save time otherwise spent typing out long path names.

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

### Get a list of files in a folder
```R
county_data <- list.files(path = './data/counties', full.names = TRUE)
```
The `full.names` flag prepends the directory path if `TRUE`, and file name only if `false`.

### Remove a package
```R
detach("package:plyr", unload=TRUE)
```
Removing a package can help when you have conflicts between functions with the same name.

### Format a date
```R
as.Date('01/01/2001', format = '%m/%d/%Y' )
```
Specify the format explicitly [using the syntax](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/strptime) from `strptime`.

### Find and replace characters in a string
```R
gsub(',', '', 'womp,womp')
gsub('\\(', ',for real', 'Replace the literal parenthesis (' )
```
Enter a [pattern](http://uc-r.github.io/regex), replacement and data value to search.

### Get rid of non-ASCII characters
```R
gsub('[^ -~]', '', '日本人GALATIA')
```
This [pattern](https://boyter.org/2014/02/explain-regex-matches-ascii-characters/), translates to "not any ASCII character". Useful when cleaning a malformed file. Can also use the `[^ -~]` in other contexts.

### Load a file with an annoying encoding
```R
vax_data <- read.delim('cnty20210731.csv', fileEncoding = 'UTF-16LE', sep = '\t',
                              col.names = c('index', 'county', 'week_of', 'age12_17',
                              'age18_24', 'age25_49', 'age50_64')
                              )
```
The [readr package](https://github.com/mtdukes/how-to#readr) is my preferred method for importing data, sometimes [it chokes](https://github.com/tidyverse/readr/issues/306) on strange file encodings even with the `locale` parameter set. So this method is worth trying.

### Turn off scientific notation
```R
options(scipen = 999)
```
Prints out the full numeral in your current workspace.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## readr
A ["fast and friendly way"](https://www.rdocumentation.org/packages/readr/versions/1.3.1) to read in data. Part of the tidyverse suite of packages.

### Specify a default column type
```R
my_data <- read_csv('../my_data.csv', col_types = cols(.default = 'c', date = 'D'))
```
Tell `read_csv` to import all columns as a character by default, except for the date field, which should be a date (you can remove the date part if you want to read everything in as a character).

### Get the file encoding
```R
guess_encoding(file = 'annoying_file.csv')
```
Useful for errors reading in the file (like embedded nulls).

### Pull data from a GitHub gist
```R
county_fips <- read_tsv( url(
    'https://gist.githubusercontent.com/mtdukes/e0c6563927fb4f3e48f4e092b84b7023/raw/56e5abc1daf2277b5d901cbc25b9f9e64ab8c073/nc_fips_tab.tsv'
    ))
```
Use the `url` function from base R to pull formatted data posted in public gists, [like these convenience files](https://github.com/mtdukes/how-to#convenience-files). Swap out the delimiter function (read_csv, read_delim, etc) as needed. Get the URL by clicking "Raw" on the Gist page.

### Specify a file encoding when loading data
```R
my_dataframe <- read_tsv('annoying_file.csv', locale = locale(encoding = "UTF-16LE") )
```
Specify non-UTF encodings you get from the `guess_encoding` output.

### Write and timestamp a csv
```R
my_spreadsheet %>% 
  write_csv(paste0('my_data/my_spreadsheet',
                   format(Sys.time(),'%Y%m%d%H%M'),
                   '.csv'))
```
Using `Sys.time()`, you can quickly save a dataset with an automatic timestamp for easy organization. No more `my_spreadsheet_final_Final_FINAL_FINALFORREAL.csv`!

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

### Convert table to uppercase
```R
clean_table <- dirty_table %>% 
  mutate(across(where(is.character), toupper))
```
Tranforms all columns containing characters to uppercase all at once. Incredibly useful for cleaning data!

## Fix multibyte strings and bad character encodings

```R
df_clean <- df %>% mutate(across(everything(), ~ iconv(.x, sub = '') ))
```
Works across an entire dataframe, removing all malformed characters, multibyte strings or bad, non-UTF8 encodings that can't be converted.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## plyr
[Tools to solve common problems](https://www.rdocumentation.org/packages/plyr/versions/1.8.6), like performing the same task over and over. *NOTE: This package conflicts with some dplyr commands, so if you're getting weird errors, this might be why.*

### Repeat a function using a list as input
```R
precinct_sort <- ldply(county_files, read_tsv, na='', col_types = cols(
  county_id = col_double(),
  election_dt = col_date(format = '%m/%d/%Y'),
  contest_id = col_double(),
  contest_title = col_character(),
  contest_vote_for = col_double(),
  precinct_code = col_character(),
  candidate_id = col_double(),
  candidate_name = col_character(),
  candidate_party_lbl = col_character(),
  voting_method_lbl = col_character(),
  voting_method_rslt_desc = col_character(),
  vote_ct = col_double()
))
```
The first parameter is the list and the second is the function you want to repeat. Everything that follows are parameters specific to your function. You can use your own functions too.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## clipr
[A set of simple commands](https://github.com/mdlincoln/clipr) for writing to and reading from the clipboard.

### Write to the clipboard
```R
age_group_populations %>%
  mutate(lookup_helper = paste0(fips,age_group), .after = 'age_group') %>% 
  write_clip()
```
Copies dataframes in a tab-delimited format for easy pasting into spreadsheets.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## googlesheets4
[Read and write data](https://googlesheets4.tidyverse.org/) from Google Sheets. Part of the tidyverse.

### Read in data from a Google Sheet
```R
test <- read_sheet('https://docs.google.com/spreadsheets/d/<<SPREADSHEET_ID>>/edit#gid=<<SHEET_ID>>')
```
Authentication may be required depending on permissions. Accepts the URL of the sheet and writes to a dataframe.

### Overwrite data in an existing sheet
```R
write_sheet(census_data,
            as_sheets_id('https://docs.google.com/spreadsheets/d/<<SPREADSHEET_ID>>/edit#gid=<<SHEET_ID>>'),
            sheet = "census_data"
            )
```
Accepts a dataframe and writes to a sheet specified with the `as_sheets_id` function and a URL. If a sheet name isn't specified, it will create a new sheet with the dataframe name.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# Math for journalism
Formulas and concepts I always have to look up. For more, read [Numbers in the Newsroom](https://www.ire.org/product/numbers-in-the-newsroom-using-math-and-statistics-in-news-second-edition-e-version/) by Sarah Cohen.

Also check out Ben Welsh's [Observable collection of calculators](https://observablehq.com/collection/@palewire/numbers-in-the-newsroom) based on Cohen's book.

## Standard deviation
A measure of how tightly clustered, or varied, data is around a set of values. Often described using the Greek letter sigma (σ).

### Normal distribution

![Distribution of values based on standard deviation.](https://github.com/mtdukes/how-to/blob/main/media/standard_deviation_diagram.svg)

[(Image by M. W. Toews)](https://commons.wikimedia.org/w/index.php?curid=1903871)

For normally distributed data (often described as a "bell curve"), about two thirds of the observed values fall within one standard deviation of the average.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

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

## Making sense of symbols
Tips and tricks for understanding mathematical symbols outside the scope of the normal add, subtract, etc.

### Summation/Product
![Check New Hanover County, McDowell County and Winston-Salem](https://github.com/mtdukes/how-to/blob/main/media/summation_product.png)
As [Freya Holmér](https://twitter.com/FreyaHolmer/status/1436696408506212353) points out, these two "scary math symbols" are just for loops (Image courtesy of Freya Holmér).

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# GIS tips
General guidance for working with mapping files and geographic information systems.

## Latitude and Longitude
![enter image description here](https://github.com/mtdukes/how-to/blob/main/media/lat_lng.gif)

(Image credit [Illinois State University](https://journeynorth.org/tm/LongitudeIntro.html))

Latitude is the **Y axis**, with the **equator** at 0. Longitude is the **X axis**, with the **prime meridian** at 0.

North America, located in the north-west quadrant, latitude values will be **positive or N**. Longitude values will be **negative or W**.

## NC bounding box
For subsetting coordinates or geometries that requires a "bounding box," use these coordinates for North Carolina (h/t to [Anthony Louis D'Agostino](https://anthonylouisdagostino.com/bounding-boxes-for-all-us-states/)).
|reference|value|
|:--|--:|
| xmin | -84.321869 |
| ymin | 33.842316 |
| xmax | -75.460621 |
| ymax | 36.588117 |

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

# Troubleshooting
A few common things to check when things get mucked up.

## Location errors
Mapping or working with counties, cities, etc. in North Carolina.

### You three again.
![Check New Hanover County, McDowell County and Winston-Salem](https://github.com/mtdukes/how-to/blob/main/media/you-three.jpeg)

If you're missing one of North Carolina's 100 counties, or your map is inexplicably blank, check New Hanover County, McDowell County and Winston-Salem first. Then think of other location names that might not be a literal, string-to-string match.

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

# Convenience files
Collections of commonly used lists and references in various data structures.

## U.S. states
-   [State names and postal code abbreviations, comma- and line-separated and text-qualified by single quotes](https://gist.github.com/mtdukes/e6636bb7e7423fbf0faf52134257d675)
-   [US counties and FIPS codes by state, tab-delimited](https://gist.github.com/mtdukes/ff6e59acde4858a8b174a83d2fce915c)

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)

## N.C. places

- [NC counties, comma- and line-separated and text-qualified by single quotes](https://gist.github.com/mtdukes/ed03837107a7e173a56b71cf7c785424)
- [NC counties and FIPS codes, tab-delimited](https://gist.github.com/mtdukes/e0c6563927fb4f3e48f4e092b84b7023)
- [NC counties and election/voter data codes, tab-delimited](https://gist.github.com/mtdukes/c3abc68866e884a7b0fa418e712b40c8)
- [NC counties and court codes, as tuples](https://gist.github.com/mtdukes/88e089e6dd08b12e57667dd7fe3b4305)
- [NC counties and court codes, tab-delimited](https://gist.github.com/mtdukes/a1689d5c678cc52b3efd6ac1a3409e70)
- [NC municipalities and counties, tab-delimited](https://gist.github.com/mtdukes/809c44e2e33f35425b9d4a173c6a4151)
- [NC places and census designation, tab-delimited](https://gist.github.com/mtdukes/17073dc3116a082cbb7ff7dcf6efe669)

[▲ BACK TO NAV](https://github.com/mtdukes/how-to#jump-to)
