---
layout: post
title: Intro to ordination
subtitle: Finding patterns in your data
date: 2018-03-11 12:15:00
author: Koenraad
meta: "Tutorials"
tags: data_manip, datavis
---
<div class="block">
	<center>
		<img src="{{ site.baseurl }}/img/tutheaderordination.png" alt="Img">
	</center>
</div>

### Tutorial Aims

#### <a href="#section1"> 1. The first section</a>

#### <a href="#section2"> 2. The second section</a>

#### <a href="#section3"> 3. The third section</a>

You can read this text, then delete it, and replace it with your text about your tutorial - what are the aims, what code do you need to achieve them?
---------------------------
We are using `<a href="#section_number">text</a>` to create anchors within our text - for example when you click on section one, the page will automatically go to where you have put `<a name="section_number"></a>`.

To create subheadings, you can use `#`, e.g. `# Subheading 1` creates a subheading with a large font size. The more hashtags you add, the smaller the text becomes. If you want to make text bold, you can surround it with `__text__`, which creates __text__. For italics, use only one understore around the text, e.g. `_text_`, _text_.

# Subheading 1
## Subheading 2
### Subheading 3

This is some introductory text for your tutorial. Explain the skills that will be learned and why they are important. Set the tutorial in context.

You can get all of the resources for this tutorial from <a href="https://github.com/ourcodingclub/CC-EAB-tut-ideas" target="_blank">this GitHub repository</a>. Clone and download the repo as a zip file, then unzip it.

<a name="section1"></a>

## 1. The first section


At the beginning of your tutorial you can ask people to open `RStudio`, create a new script by clicking on `File/ New File/ R Script` set the working directory, and load some packages, for example `ggplot2` and `dplyr`. You can surround package names, functions, actions ("File/ New...") and small chunks of code with backticks, which defines them as inline code blocks and makes them stand out among the text, e.g. `ggplot2`.

When you have a larger chunk of code, you can paste the whole code in the `Markdown` document and add three backticks on the line before the code chunks starts and on the line after the code chunks ends. After the three backticks that go before your code chunk starts, you can specify in which language the code  is written, in our case `R`.

To find the backticks on your keyboard, look towards the top left corner on a Windows computer, perhaps just above `Tab` and before the number one key. On a Mac, look around the left `Shift` key. You can also just copy the backticks from below.

```r
# Set the working directory
setwd("your_filepath")

# Load packages
library(adehabitatLT)
library(lme4)
library(nlme)
library(car)
library(ggplot2)
```

<a name="section2"></a>

## 2. The second section

You can add more text and code, e.g.

```r
# Grab your data
Dispersion <- read.csv("Dispersion.txt", sep="")

# Take a look at them: what is what?
View(Dispersion)
# The individuals represent mites from different treatments that were put on a plastic arena 
# and monitored for 10 minutes to highlight differences in their movement patterns

# create a variable with the date sequence
date <- as.POSIXct(strptime(as.character(Dispersion$Slice.n.), format = "%j"),tz = "GMT")

# create a trace file with all the coordinate sequences: take a good look at the syntax
trace <- as.ltraj(xy=Dispersion[,c("X", "Y")], date = date, id = Dispersion$ID)

trace

is.regular(trace)

summ1 <- summary.ltraj(trace)
subset(summ1, nb.reloc < 60)

# irregular burst: add missing values

refda <- strptime("2017-01-01", "%Y-%m-%d", tz = "GMT") # set a reference date

trace2 <- setNA(trace, refda, 1, units = "day") # check if every value is separated from refdate by a multiple of our unit

is.regular(trace2)
is.sd(trace2)     # to check if, in a regular trace, all bursts have the same duration

# Visualise path
plot(trace2[5])    # path; start = BLUE, last used observation = RED

ind <- trace2[5]

# Check for autocorrelation and visualise results
wawotest(ind)                  # test for autocorrelation of x, y and dist: low P values mean strong autocorrelation
acfdist.ltraj(ind, lag = 20, which = "dist") # autocorrelation function (Dray, 2010): the black dots are significantly autocorrelated  at 95%

```


At this point it would be a good idea to include an image of what the plot is meant to look like, so students can check they've done it right. Replace `IMAGE_NAME.png` with your own image file:

<center> <img src="{{ site.baseurl }}/IMAGE_NAME.png" alt="Img" style="width: 800px;"/> </center>

<a name="section1"></a>

## 3. The third section

More text, code and images.

This is the end of the tutorial. Summarise what the student has learned, possibly even with a list of learning outcomes. In this tutorial we learned:

##### - how to generate fake bivariate data
##### - how to create a scatterplot in ggplot2
##### - some of the different plot methods in ggplot2

We can also provide some useful links, include a contact form and a way to send feedback.

For more on `ggplot2`, read the official <a href="https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf" target="_blank">ggplot2 cheatsheet</a>.

Everything below this is footer material - text and links that appears at the end of all of your tutorials.

<hr>
<hr>

#### Check out our <a href="https://ourcodingclub.github.io/links/" target="_blank">Useful links</a> page where you can find loads of guides and cheatsheets.

#### If you have any questions about completing this tutorial, please contact us on ourcodingclub@gmail.com

#### <a href="INSERT_SURVEY_LINK" target="_blank">We would love to hear your feedback on the tutorial, whether you did it in the classroom or online!</a>

<ul class="social-icons">
	<li>
		<h3>
			<a href="https://twitter.com/our_codingclub" target="_blank">&nbsp;Follow our coding adventures on Twitter! <i class="fa fa-twitter"></i></a>
		</h3>
	</li>
</ul>

### &nbsp;&nbsp;Subscribe to our mailing list:
<div class="container">
	<div class="block">
        <!-- subscribe form start -->
		<div class="form-group">
			<form action="https://getsimpleform.com/messages?form_api_token=de1ba2f2f947822946fb6e835437ec78" method="post">
			<div class="form-group">
				<input type='text' class="form-control" name='Email' placeholder="Email" required/>
			</div>
			<div>
                        	<button class="btn btn-default" type='submit'>Subscribe</button>
                    	</div>
                	</form>
		</div>
	</div>
</div>
