---
title: An introduction to knitcitations
author: Carl Boettiger
date: 27 May 2014
output: 
  html_document:
    pandoc_args: [
      "--biblio", "references.bib",
      "--csl", "ecology.csl"
      ]
---

<!--
%\VignetteEngine{knitr::knitr}
%\VignetteIndexEntry{An introduction to knitcitations}
-->

[![Build Status](https://travis-ci.org/cboettig/knitcitations.svg)](https://travis-ci.org/cboettig/knitcitations)

knitcitations
=============



- **Author**: [Carl Boettiger](http://www.carlboettiger.info/)
- **License**: [CC0](http://creativecommons.org/publicdomain/zero/1.0/)
- [Package source code on Github](https://github.com/cboettig/knitcitations)
- [**Submit Bugs and feature requests**](https://github.com/cboettig/knitcitations/issues)


`knitcitations` is an R package designed to add dynamic citations to dynamic documents created with [Yihui's knitr package](https://github.com/yihui/knitr).



Installation 
------------

Install the development version directly from Github 

```r
library(devtools)
install_github("knitcitations", "cboettig")
```

Or install the current release from your CRAN mirror with `install.packages("knitcitations")`.  


Quick start: rmarkdown (pandoc) mode
------------------------------------

Start by loading the library.  It is usually good to also clear the bibliographic environment after loading the library, in case any citations are already stored there:  


```r
library("knitcitations")
```

```
## Loading required package: bibtex
## Loading required package: RefManageR
## 
## Attaching package: 'knitcitations'
## 
## The following object is masked from 'package:utils':
## 
##     cite
```

```r
cleanbib()
```




### Cite by DOI

Cite an article by DOI and the full citation information is gathered automatically. By default this now generates a citation in pandoc-flavored-markdown format. We use the inline command `citep("10.1890/11-0011.1")` to create this citation [@Abrams2012].  

An in-text citation is generated with `citet`, such as `citet("10.1098/rspb.2013.1372")` creating the citation to @Boettiger2013.  


### Cite by URL

Not all the literature we may wish to cite includes DOIs, such as [arXiv](http://arxiv.org) preprints, Wikipedia pages, or other academic blogs.  Even when a DOI is present it is not always trivial to locate.  With version 0.4-0, `knitcitations` can produce citations given any URL using the [Greycite API](http://greycite.knowledgeblog.org). For instance, we can use the call `citep("http://knowledgeblog.org/greycite")` to generate the citation to the Greycite tool [@greycite2739].  

### Cite bibtex and bibentry objects directly 

We can also use `bibentry` objects such as R provides for citing packages (using R's `citation()` function): `citep(citation("knitr")` produces [@Xie2013; @Xie2013_; @Xie2014].  Note that this package includes citations to three objects, and pandoc correctly avoids duplicating the author names.  In pandoc mode, we can still use traditional pandoc-markdown citations like `@Boettiger2013` which will render as @Boettiger2013 without any R code, provided the citation is already in the `.bib` file we name (see below).


### Re-using Keys

When the citation is called, a key in the format `FirstAuthorsLastNameYear` is automatically created for this citation, so we can now continue to cite this article without remembering the DOI, using the command `citep("Abrams2012")` creates the citation [@Abrams2012] without mistaking it for a new article.  


### Displaying the final bibliography

At the end of the document, include a chunk containing the command:


```r
write.bibtex(file="references.bib")
```

Use the chunk option `echo=FALSE` to hide the chunk command.  This creates a Bibtex file with the name given.  [Pandoc](http://johnmacfarlane.net/pandoc) can then be used to compile the markdown into HTML, MS Word, LaTeX, PDF, or many other formats, each with the desired journal styling. Pandoc is now integrated with [RStudio](http://rstudio.com) through the [rmarkdown](http://rmarkdown.rstudio.com) package.  Pandoc appends these references to the end of the markdown document automatically.  In this example, we have added a yaml header to our Rmd file which indicates the name of the bib file being used:

```yaml
---
output: 
  html_document:
    pandoc_args: [
      "--biblio", "references.bib",
      "--csl", "ecology.csl"
      ]
---
```


Then calling `rmarkdown::render("tutorial.Rmd")` from R on the tutorial compiles the output markdown, with references in the format of the ESA journals.  

# References





<!--

At the end of our document we can generate the traditional "References" or "Works Cited" list in a knitr block using the chunk option `results='asis'` to display as text rather than code:  


```r
bibliography()
```


- Peter A. Abrams, Lasse Ruokolainen, Brian J. Shuter, Kevin S. McCann,   (2012) Harvesting Creates Ecological Traps: Consequences of Invisible Mortality Risks in Predator–Prey Metacommunities.  *Ecology*  **93**  [10.1890/11-0011.1](http://dx.doi.org/10.1890/11-0011.1)
- Carl Boettiger,   (2012) knitcitations: Citations for knitr markdown files.  [https://github.com/cboettig/knitcitations](https://github.com/cboettig/knitcitations)
- Yihui Xie,   (2013) knitr: A general-purpose package for dynamic report generation in R.  [http://yihui.name/knitr/](http://yihui.name/knitr/)
- Phillip Lord,   (2012) Greycite.  *Knowledge Blog*  [http://knowledgeblog.org/greycite](http://knowledgeblog.org/greycite)


Other formats can be given as options to `bibliography`, as described in the help documentation, `?bibliography`.  For instance, we can specify the format as "markdown".  The custom formats "markdown" and "rdfa" take an additional argument, "ordering", which can specify what elements we want to print and what order they should be given in.  For instance, we can omit everything but the authors, year, and journal, given in that order:


```r
bibliography("markdown", ordering = c("authors", "year", "journal"))
```


- Peter A. Abrams, Lasse Ruokolainen, Brian J. Shuter, Kevin S. McCann,   (2012)  *Ecology*
- Carl Boettiger,   (2012)
- Yihui Xie,   (2013)
- Phillip Lord,   (2012)  *Knowledge Blog*


(Note that since version 0.5, "markdown" is the default and can be omitted)

### Links and tooltips

In-text citations are now linked by default to the article.  We can turn this on or off in a single citation like so: `citep("Abrams2012", linked=TRUE)`, creating the citation (<a href="http://dx.doi.org/10.1890/11-0011.1">Abrams _et. al._ 2012</a>).  We can toggle this behavior on or off globally using `cite_options(linked=TRUE)` at the beginning of our document.  


Using the popular javascript library from [bootstrap](http://twitter.github.com/bootstrap), we can tell `knitcitations` to include a javascript tooltip on mouseover.  (This effect will not work inside a github repo due the the lack of the javascript library, but can easily be deployed on a website, see Boettiger (2013). This function is off by default can be toggled on or off in the same way, `cite_options(tooltip=TRUE)`.  

![Screenshot of citation produced with a tooltip.](http://farm9.staticflickr.com/8233/8499745634_04a13fe93e_o.png)

## Semantics 

### CiTO  

Additional semantic markup can be added the the citations themselves, such as the reason for the citation.  For instance, we can identify that we have used the method from Shotton (2010) with the inline command `citet("10.1186/2041-1480-1-S1-S6", cito = "usesMethodIn")`.  

More discussion on using `knitcitations` for CITO and semantic markup can be found in Boettiger (2013).  



-->


