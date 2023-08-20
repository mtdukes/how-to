# Working with data in R

A cheat sheet for common data journalism issues in R. An addendum to the main [how-to page](https://github.com/mtdukes/how-to).

## Reformatting data

### Create new rows from a delimited column

Given a table with a column of delimited values...

|column_a |column_b                           |
|:--------|:----------------------------------|
|a        |Wake, Mecklenburg, New Hanover     |
|b        |Wake, Wake, Columbus               |
|c        |Hoke, Beaufort, Craven, Pasquotank |

...separate each comma-delimited value into its own row.

```R
tidyr::tibble(
  column_a = c('a', 'b', 'c'),
  column_b = c('Wake, Mecklenburg, New Hanover', 'Wake, Wake, Columbus', 'Hoke, Beaufort, Craven, Pasquotank')
  ) %>%
  tidyr::separate_longer_delim(cols = column_b, delim = ', ')
```

### Create a named list from a vector of values and labels

Given two vectors containing values (data) and labels (column names), create a named list.

```R
setNames(as.list(c('DUKES', 'TYLER', '101 MAIN ST', 'RALEIGH', 'NC')),
         as.list(c('last_name', 'first_name', 'st_address', 'city', 'state'))
         )
```

Useful for later assigning values in a dataframe by name.

### Convert named list to dataframe

Given two vectors stored into a named list, asign the values to columns in a dataframe or tibble.

```R
user_list <- setNames(as.list(c('DUKES', 'TYLER', '101 MAIN ST', 'RALEIGH', 'NC')),
         as.list(c('last_name', 'first_name', 'st_address', 'city', 'state'))
         ) 
user_table <- tidyr::tibble(
  last_name = user_list$last_name,
  first_name = user_list$first_name,
  st_address = user_list$st_address,
  city = user_list$city,
  state = user_list$state,
)
```

### Convert a list of lists into a dataframe

Given a "big list" containing named lists, convert it to a dataframe/tibble.

```R
parsed_tibble <- list_of_lists %>%
  map_df(as_tibble)
```

### Create an empty dataframe for known columns/data types

Use `tibble` from the `tidyverse` package to create a dataframe with column names and defined [column types](https://tibble.tidyverse.org/articles/types.html).

```R
#define the dataframe and the character types
df_template <- tidyr::tibble(
  column_one = character(),
  column_two = double(),
  columnt_three = logical(),
  column_four = integer()
)

#duplicate tempalte to create an empty table
my_table <- df_template
```

### Create an empty dataframe for unknown columns/data types

When you don't know how many columns you need or what data type they'll be, initialize a table with no rows or defined columns.

```R
new_table <- tidyr::tibble()
```

## Processing data

### Loop through a dataframe

Use a `for` loop to iterate through a dataframe row by row, using the row index number. Useful for when you need to access multiple columns in a dataframe, instead of just one set of values.

```R
df_to_loop -> tidyr::tibble(
  column_a = c(1, 2, 3, 4),
  column_b = c('Bob', 'Jenee', 'Marcus', 'Sally')
  )

# loop through each row and print the variables
for(row in 1:nrow(df_to_loop)){
  
  #store the value of the row item, rather than the item as a dataframe
  column_a_value <- df_to_loop[row, 'column_a'][[1]]
  column_b_value <- df_to_loop[row, 'column_b'][[1]]

  #print the values in a loop
  print(column_a_value)
  print(column_b_value)
}
```
