
# Advent of Code Website Template

Here’s a template for making [Quarto](https://quarto.org) websites for
working on and (optionally) publishing [Advent of
Code](https://adventofcode.com) solutions. Essentially, each year is a
listing page, and each day is a post.

It works hand-in-hand with the
[**aochelpers**](https://ellakaye.github.io/aochelpers) package for R,
which makes it incredibly easy to set up new posts, scripts and
listings, using supplied (though personalisable) templates, found in the
`_templates` directory. When a template is copied by functions from
**aochelpers**, e.g. `aoc_new_year(2022)` or `aoc_new_day(1, 2024)` any
occurrence of “DD” and “YYYY” in both the copied files’ titles and the
text inside will be replaced with the value of the `day` and `year`
arguments respectively.

The website corresponding to this template is
<https://ellakaye.github.io/advent-of-code-website-template>, so you can
see it in action there.

## Templates

The `_templates` directory contains the following templates:

- `post-template`, which contains `index.qmd` and `script.R`, which gets
  copied on calls to `aoc_new_day()`
  - `index.qmd` is the template for writing up each day’s solution. It
    automatically provides correct links to the relevant puzzle on the
    Advent of Code website, as well as a link to your input (assuming
    the input is in the same directory, which it will be if the post has
    been created with `aoc_new_day()`). It also reads in the input using
    `aoc_input_vector()`, and notes alternative `aoc_input_*` functions
    if those are more appropriate for the day.
  - `script.R` provides a place to work on your solutions, before
    writing them up.
- `YYYY-intro`, which contains `index.qmd` is the template for an
  introductory post for each year. It gets copied by `aoc_new_year()`
  and is necessary for the website to render after a call to that
  function, but before any other posts are present (Quarto v1.4 onwards
  doesn’t allow empty listings pages.)
- `YYYY.qmd` is the listing page for the year, which gets copied on a
  call to `aoc_new_year()`
- `_metadata.yml`, which gets copied by `aoc_new_year()`, sets the
  options for all the posts for the year. See [this page of the Quarto
  website](https://quarto.org/docs/projects/quarto-projects.html#shared-metadata)
  for more details.

I’ve set up these templates in a way that I think works well, but of
course you can customise them to whatever you want for your version of
the site. **Don’t rename them** though, otherwise the **aochelpers**
functions won’t be able to find them. Do use “DD” and “YYYY” wherever
you want the actual value of the day and year to appear.

## A note on directory structure and file names

The directory structure and file names have been set to echo the Advent
of Code website. So, for example, the Day 1 puzzle for 2022 is at
<https://adventofcode.com/2022/day/1> and the corresponding page on the
template website is
<https://ellakaye.github.io/advent-of-code-template-website/2022/day/1>.
Likewise, the input can be found at
<https://adventofcode.com/2022/day/1/input> and
<https://ellakaye.github.io/advent-of-code-template-website/2022/day/1/input>
respectively. (For your own version of the website, swap out the user
name and repo name accordingly).

## Using the website with **aochelpers**

**aochelpers** can be installed from its
[repo](https://github.com/EllaKaye/aochelpers):

``` r
remotes::install_github("EllaKaye/aochelpers")
```

``` r
library(aochelpers)
```

The two main functions for managing files, already mentioned above, are
`aoc_new_year()` and `aoc_new_day()`.

The calls used to create this template were:

``` r
# Add a listing page a directory for a new year
aoc_new_year() # set up current year (including intro post)
aoc_new_year(2022, intro = FALSE) # set up a specified year (without intro post)

# Add a post for a new day
aoc_new_day(1, 2022) # day 1 of 2022 (don't need to specify year for current year)

# Get input for a day without generating a post 
# (i.e. no index.qmd or script.R in the 2022/day/2 directory)
aoc_get_input(2, 2022) # day 2 of specified year
```

In the descriptions below, the `YYYY` and `DD` placeholders are used to
indicate where the year and day values will be inserted.

[`aoc_new_year()`](https://ellakaye.github.io/aochelpers/reference/aoc_new_year.html)
will

- create a new directory for the specified year, at `./YYYY/`.
- create a new listing page for the year, as `./YYYY.qmd`. The listing
  page will be created using the template `./_templates/YYYY.qmd`. The
  listing page picks up posts from the `YYYY/day` directory. (This
  directory structure echoes the structure of the Advent of Code
  website.)
- optionally create an introductory post for the year, as
  `./YYYY/day/YYYY-introduction`, using the template
  `./_templates/YYYY-intro`. The post will be created only if the
  `intro` argument is `TRUE` (the default). Note that, as of Quarto
  v1.4, there needs to be at least one post in the `YYYY/day` directory
  for the website to render without error.
- if `_templates/_metadata.yml` exists, it will be copied to
  `./YYYY/day/_metadata.yml`.

[`aoc_new_day()`](https://ellakaye.github.io/aochelpers/reference/aoc_new_day.html)
will

- create a new directory for the specified day, at `./YYYY/day/DD/`
- copy the contents of `_templates/post-template` into the above
  directory
- download the puzzle input for the day from the Advent of Code website,
  and save it as `./YYYY/day/DD/input` (via a call to
  [`aoc_get_input()`)](https://ellakaye.github.io/aochelpers/reference/aoc_get_input.html)

There are other functions for creating and deleting directories and
files based on the advent-of-code-website-template. See the package
[Reference](https://ellakaye.github.io/aochelpers/reference/index.html)
page for details.

## Examples posts

The template comes ready to go for 2024, with a placeholder introduction
post, and also with an day 1 post for 2022, so you can see what the
templates look in action. All files related to 2022 can be removed with
a call to `aoc_delete_year(2022)`. The intro post for 2024 can be
removed with `aoc_delete_intro(2024)` (once there’s another post for
2024 present).

## Functions for reading in input

**aochelpers** provides functions for reading in input in various ways.
The input for Day 2 of 2022 allows us to demonstrate all three:

``` r
aoc_input_vector(2, 2022) |> head()
```

    ## [1] "A X" "B Y" "B Y" "C X" "B X" "C Z"

``` r
aoc_input_data_frame(2, 2022) |> head()
```

    ## # A tibble: 6 × 2
    ##   X1    X2   
    ##   <chr> <chr>
    ## 1 A     X    
    ## 2 B     Y    
    ## 3 B     Y    
    ## 4 C     X    
    ## 5 B     X    
    ## 6 C     Z

``` r
aoc_input_matrix(2, 2022) |> head()
```

    ##      [,1] [,2] [,3]
    ## [1,] "A"  " "  "X" 
    ## [2,] "B"  " "  "Y" 
    ## [3,] "B"  " "  "Y" 
    ## [4,] "C"  " "  "X" 
    ## [5,] "B"  " "  "X" 
    ## [6,] "C"  " "  "Z"

`aoc_input_vector()` and `aoc_input_matrix()` both have a `mode`
argument that allow you to specify whether the input is character or
numeric (defaults to character). `aoc_input_matrix()` by default has a
new column for each single character/digit, though that can be changed
with the `split` argument. `aoc_input_data_frame()` can return either a
`tbl_df` or `data.frame`.

## Themes

The website template comes with two custom themes, one light and one
dark. The light theme is clean, with Christmas-y shades of green and
red. The dark theme is reminiscent of the Advent of Code website (though
not identical, since the design of <https://adventofcode.com> is part of
its registered trademark). You can switch between them using the toggle
in the top right corner of the page. Both themes use [fonts from
iA](https://github.com/iaolo/iA-Fonts). The themes can be adapted in the
`custom-light.scss` and `custom-dark.scss` files. For more on Quarto
themes, see the
[documentation](https://quarto.org/docs/output-formats/html-themes.html).

## Publishing

The template is set up with the option to publish automatically to
GitHub pages, using a GitHub action that activates on push to the main
branch. To allow this, when using the template, tick the box to ‘include
all branches’, which will then copy over the `gh-pages` branch as well.
If you do not wish to publish in this way, only copy the default branch,
and then you can delete the `.github` directory as well.

For more information on the many options for publishing Quarto websites,
see the [documentation](https://quarto.org/docs/publishing/).

## Examples

This template is an extension of my work on an Advent of Code website
for myself, links below. If anyone else uses this template and would
like to share the links on this README, please do submit a pull request
to include it here, or raise an issue and I’ll add it. It would be great
to get a collection.

- Ella Kaye: [website](https://adventofcode.ellakaye.co.uk),
  [repo](https://github.com/EllaKaye/advent-of-code-website). This
  version has substantially different theming to the template (to match
  my [personal site](https://ellakaye.co.uk)) and deploys manually to
  netlify (due to purchased, licensed fonts that I can’t check into
  GitHub).

## Other R Advent of Code projects

The project arose because my write-up of my 2020 solutions as one long
blog post was too unwieldy. I was inspired by Emil Hvitfledt’s [R Advent
of Code](https://emilhvitfeldt.github.io/rstats-adventofcode/) website,
which has a separate page for each year, though his site uses a tabset
for the different days, whereas this one has a separeate listing page
for each year, then separate posts for each day.

**aochelpers** adapts and builds upon code from David Robinson’s
[adventdrob](https://github.com/dgrtwo/adventdrob) package. His package
contains other functions for working with Advent of Code input that he
has found useful when approaching the challenges over the years.

TJ Mahr has an [**aoc**](https://github.com/tjmahr/aoc) package that
provides [usethis](https://usethis.r-lib.org)-style functions for Advent
fo Code puzzles. It takes a different approach to **aochelpers** by
organising everything within the structure of an R package, with a new
package for each year.
