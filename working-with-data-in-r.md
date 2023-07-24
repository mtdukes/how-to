# Working with data in R

A cheat sheet for common data journalism issues in R. An addendum to the main [how-to page](https://github.com/mtdukes/how-to).

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