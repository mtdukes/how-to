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

Given two named vectors containing values (data) and labels (column names), create a named list.

```R
setNames(as.list(c('DUKES', 'TYLER', '101 MAIN ST', 'RALEIGH', 'NC')),
         as.list(c('last_name', 'first_name', 'st_address', 'city', 'state'))
         )
```

Useful for assigning values in a dataframe.

