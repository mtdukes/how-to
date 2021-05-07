
```
ooooo   ooooo   .oooooo.   oooooo   oooooo     oooo         ooooooooooooo   .oooooo.   
`888'   `888'  d8P'  `Y8b   `888.    `888.     .8'          8'   888   `8  d8P'  `Y8b  
 888     888  888      888   `888.   .8888.   .8'                888      888      888 
 888ooooo888  888      888    `888  .8'`888. .8'                 888      888      888 
 888     888  888      888     `888.8'  `888.8'     8888888      888      888      888 
 888     888  `88b    d88'      `888'    `888'                   888      `88b    d88' 
o888o   o888o  `Y8bood8P'        `8'      `8'                   o888o      `Y8bood8P'  
```
A cheat sheet for common data journalism stuff.

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