

# Instructions on writing your semester project with Quarto

## 

![image](https://github.com/ComputationalMovementAnalysis/Project-template/assets/12532091/384a5859-2407-4a0a-9324-c1ad4517663f)


![image](https://github.com/ComputationalMovementAnalysis/Project-template/assets/12532091/b78771d9-9832-4918-8cc3-bb81f7ac7f4e)


## Research proposal

To submit your research proposal, fill out the Document [Readme.md]([Readme.md]). Note that this is a Markdown document where you can use the [markdown syntax](https://www.markdownguide.org/basic-syntax/) to write text. 

## Project Report

### Writing

Write your project report in the file [index.qmd](index.qmd). Please to not use a different filename for your project report other than index.qmd. If you are an advanced user, you can use RMarkdown as well, but make sure you translate all instructions stated here to RMarkdown. 

Note that this file has default [chunk options](https://quarto.org/docs/reference/cells/cells-knitr.html#cell-output) set in the [YAML header](https://quarto.org/docs/get-started/hello/rstudio.html#yaml-header) (first few lines in between the two `---`). 

```
format:
  html:
    code-fold: true     # makes the code in the output collapsable
execute:
  warning: false        # hides warnings from the generated output
  message: false        # hides messages from the generated output
lang: en                # sets the document language to english. Switch to "de" if you write in german
```

Please don't change these specific options (except for the `lang` option). If for some reason you want to change this behaviour for a specific chunk, you can override these options by setting the chunk options within the chunk (more information [here](https://quarto.org/docs/get-started/hello/rstudio.html#code-chunks)). 

Include your *pre*processing steps in the first chunk we prepared for this purpose. 
- **The bulk of your R-code will go here** (since [most time is spent on getting your data into the right form](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/)).  The code in this chunk will appear *before* your actual report and typically does not need to be described in detail in your report. Ideally you will add some inline comments to this code using the `#` character). 
- Typically, the rest of your report will mostly include prose, with a few chunks here and there to make maps, plots, tables, statistical models etc.
- If you have computationally heavy preprocessing to do, read the section "Heavy Computation". 

### Heavy computation

Including *all* your code index.qmd and rendering it each time you want to preview your report makes your report less error prone and more reproducible, but this workflow can be cumbersome when the code takes a long time to execute. This prevents you iterating fast when writing up your report. We suggest the following method to solve this:

Outsource your preprocessing steps and especially the heavy computation into a seperate R-Script called `preprocessing.R`. In this script, generate all outputs that you will need in your report (index.qmd). To "prove" that this script runs on your machine from top to bottom, in a new session and without any errors, use the function `reprex_html("preprocessing.R")` from the package [`reprex`](https://reprex.tidyverse.org/) (you might need to install this first). **Push the resulting file (`preprocessing.html`) to GitHub** (this is a hard requirement).

### Maps, Plots, Tables

If you want to make an interactive map containing (e.g. using `tmap`), make sure you don't add too much data to your map. Adding many data points can blot your report and make it slow to load. If you want to show a large amount of data, consider making a static map instead (using `tmap_mode("plot")`). 

If you want to visualize a dataframe as a table, you can using the function `knitr::kabel(df)` (replace `df` with the name of your `data.frame`). 

To add a caption (text) to a figure, using the `#| fig-cap: ` option (see [here](https://quarto.org/docs/reference/cells/cells-knitr.html#figures)). Similarly, a caption for a table is added via `tbl-cap` (see [here](https://quarto.org/docs/reference/cells/cells-knitr.html#tables)). 

To reference (mention) a figure in your text, first add a label to the specific chunk using `#| label: fig-plot`. You can then reference that figure using the syntax `@fig-plot` (note the label must start with `fig-`). For more information, see [here](https://quarto.org/docs/authoring/cross-references.html#computations). 

Similarly, to reference a table in your text, use a label starting with `tbl-something` and reference it with `@tbl-something`. For more information, read [this](https://quarto.org/docs/authoring/cross-references.html#computations-1).


### Counting Words

To count the number of words in your report, install the rpackage [`wordcountaddin`](https://github.com/benmarwick/wordcountaddin#how-to-install). 

```r
devtools::install_github("benmarwick/wordcountaddin",  type = "source", dependencies = TRUE)
```

Then, add the follwing code chunk to your report:

<!-- todo: add this to the tempalte and test it -->

```
wordcountaddin::word_count("index.Qmd")
```



### Publishing

To publish your report you need to follow the following steps:

1. Render your file to html. If your file is called index.qmd this will create a file called index.html
2. Push this file (and the corresponding folder `index_files/`) to github. Make sure this was sucessfull by looking for these files on GitHub. Alternatively, you can use the methods described [here](https://quarto.org/docs/publishing/github-pages.html)
3. Activate GitHub Pages on your repo (Settings → Pages →Choose your branch (Main or Master) → Select Folder "Root"
4. Once your page is ready, the url to your website is: username.github.io/reponame/index.html (since index.html is the default html file, it can be omitted from the url)

### Advanced

If you are up for some advanced aspects of writing reports with Quarto, checkout 

- Citations & Footnotes: https://quarto.org/docs/authoring/footnotes-and-citations.html
- Journal Article Template: https://quarto.org/docs/journals/formats.html
