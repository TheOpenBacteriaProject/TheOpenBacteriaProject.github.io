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

#### <a href="#section1"> 1. Get familiar with ordination?</a>

#### <a href="#section2"> 2. Learn about the different ordination techniques</a>

#### <a href="#section3"> 3. Learn to interpret ordination results</a>


`__In this tutorial, we will learn to use ordination to explore patterns in multivariate ecological datasets. We will mainly use the ´vegan´ package to introduce you to three (unconstrained) ordination techniques: Principal Component Analysis (PCA), Principal Coordinate Analysis (PCoA) and Non-metric Multidimensional Scaling (NMDS).__´

To get all you need for this session, go to the repository for this <a href="https://github.com/ourcodingclub/XXXX" target="_blank">tutorial</a>, click on Clone+Download, select Download ZIP and then unzip the files to a folder on your computer. Finally, set the folder as your working directory by running the code ´setwd(choose.dir())´, or by clicking Session/ Set Working Directory/ Choose Directory from the RStudio menu.

If you are registered on GitHub, you can also clone the repository to your computer and start a version-controlled project in RStudio. For more details on how to start a version-controlled project, please check out our <a href="https://ourcodingclub.github.io/2017/02/27/git.html" target="_blank">Intro to Github for version control tutorial</a>.

Make a new script file using ´File/ New File/ R Script´ and we are all set to explore the world of ordination.

---------------------------


To create subheadings, you can use `#`, e.g. `# Subheading 1` creates a subheading with a large font size. The more hashtags you add, the smaller the text becomes. If you want to make text bold, you can surround it with `__text__`, which creates __text__. For italics, use only one understore around the text, e.g. `_text_`, _text_.



<a name="section1"></a>

# 1. What is ordination?

## Goals of ordination
Ordination is a collective term for multivariate techniques which summarizes a multidimensional dataset in such a way that when it is projected onto a two dimensional space any intrinsic pattern the data may possess becomes apparent upon visual inspection (Pielou, 1984). 

In ecological terms: Ordination serves to summarize community data (such as species abundance data: samples by species) by producing a low-dimensional ordination space in which similar species and samples are plotted close together, and dissimilar species and samples are placed far apart.  Ideally and typically, dimensions of this ‘low dimensional space’ will represent important and interpretable environmental gradients.

Generally, ordination techniques are used in ecology to describe relationships between species composition patterns and the underlying environmental gradients (asking, what environmental variables structure the community?). Two very important advantages of ordination is that 1) we can determine the relative importance of different gradients and 2) the graphical results from most techniques often lead to ready and intuitive interpretations of species-environment relationships.

To give you an idea about how ordination results look like, we´ll run the following code.

```r
# Set the working directory (if you didn´t do this already)
setwd("your_filepath")

# Load packages
library(vegan)

library(vegan)
library(dplyr)

#Load the community dataset which we´ll use in the examples today
data(varespec)

#Open the dataset and look if you can find any patterns
View(varespec)

#It is probably very difficult to see any patterns 
#With this command, you´ll perform a NMDS and plot the results
#Try to interpret the results
varespec%>%
  metaMDS(trace=F)%>%
  ordiplot(type="none")%>%
  text("sites")

```
The plot you´ve made should look like this:
<center> <img src="{{ site.baseurl }}/ordexample.png" alt="Img" style="width: 800px;"/> </center>

In this tutorial, we only focus on unconstrained ordination or indirect gradient analysis. This ordination goes in two steps. First, we will perfom an ordination on a species abundance matrix. Then we will use environmental data (samples by environmental variables) to interpret the gradients that were uncovered by the ordination. The final result will look like this:

<center> <img src="{{ site.baseurl }}/ordexample2.png" alt="Img" style="width: 800px;"/> </center>

## Ordination vs. classification

Ordination and classification (or clustering) are the two main classes of multivariate methods that community ecologists employ. To some degree, these two approaches are complementary. Classification, or putting samples into (perhaps hierarchical) classes, is often useful when one wishes to assign names to, or to map, ecological communities. However, given the continuous nature of communities, ordination can be considered a more ‘natural’ approach. Ordination aims at arranging samples or species continuously along gradients.

If you want to know how to do a classification, please check out our <a href="https://ourcodingclub.github.io/2017/03/21/data-clustering.html" target="_blank">Intro to data clustering</a>.

<a name="section2"></a>

# 2. Different ordination techniques
In this section you will learn more about how and when to use three of the main (unconstrained) ordination techniques: Principal Component Analysis (PCA), Principal Coordinate Analysis (PCoA) and Non-metric Multidimensional Scaling (NMDS).

## Principal Component Analysis (PCA)
PCA uses a rotation of the original axes to derive new axes, which maximize the variance in the data set. in 2D, this looks as follows:
<center> <img src="{{ site.baseurl }}/PCAexample.png" alt="Img" style="width: 800px;"/> </center>

Computationally, PCA is an eigenanalysis. To explain what this is and how it works is beyond the scope of this tutorial. We will only discuss the most important consequences:
1) There is a unique solution to the eigenanalysis, no matter the order of data.
2) The axes (also called principal components) are orthogonal to each other (and thus independent).
3) Each ordination axis is associated with an eigenvalue. The sum of the eigenvalues will equal the sum of the variance of all variables in the data set. The eigenvalues represent the variance extracted by each axis, and are often conveniently expressed as a percentage of the sum of all eigenvalues (i.e. total variance). The relative eigenvalues thus tell how much variation that PC is able to 'explain'.
4) Axes are ranked by their eigenvalues. Thus, the first axis has the highest eigenvalue and thus explains the most variance, the second axis has the second highest eigenvalue, etc.
5) There are a potentially large number of axes (usually, the number of samples minus one, or the number of species minus one, whichever is less) so there is no need to specify the dimensionality in advance. However, the number of dimensions worth interpreting is usually very low.
6) Species and samples are ordinated simultaneously, and can hence both be represented on the same ordination diagram (if this is done, it is termed a biplot)
7) The variable loadings of the original variables on the PC´s may be understood as how much each variable 'contributed' to building a PC. The absolute value of the loadings should be considered as the signs are arbitrary.

In most applications of PCA, variables are often measured in different units. For example, PCA of environmental data may include pH, soil moisture content, soil nitrogen, temperature and so on. For such data, the data must be standardized to zero mean and unit variance. For ordination of ecological communities, however, all species are measured in the same units, and data should not be standardized.

Let´s have a look how to do a PCA in R:

```r
#You can use several packages to perform a PCA:
#The rda() function in the package vegan
#The prcomp() function in the package stats
#The pca() function in the package labdsv

#We will use the rda() function and apply it to our varespec dataset
PCA<-rda(varespec, scale = FALSE)
#Use scale = TRUE if your variables are on different scales (e.g. for abiotic variables).
#Here, all species are measured on the same scale

#Now plot a bar plot of relative eigenvalues. This is the percentage variance explained by each axis
barplot(as.vector(PCA$CA$eig)/sum(PCA$CA$eig)) 
#How much of the variance in our dataset is explained by the first principal component?

#Calculate the percent of variance explained by first two axes
sum((as.vector(PCA$CA$eig)/sum(PCA$CA$eig))[1:2]) #79%, this is good!
#Also try to do it for the first three axes

#Now, we´ll plot our results with the plot function
plot(PCA)
plot(PCA,display="sites", type = "points")
plot(PCA,display="species", type = "text")
#Try to display both species and sites with points

#You can extract the species and site scores on the new PC axes:
sitePCA<-PCA$CA$u #Site scores
speciesPCA<-PCA$CA$v #Species scores

#In a biplot of a PCA, species´ scores care drawn as arrows that point in the direction of increasing values for that variable
biplot(PCA, choices=c(1,2), type=c("text","points")) #biplot of axis 1 vs 2
biplot(PCA, choices=c(1,3), type=c("text","points")) #biplot of axis 1 vs 3

#Check out the help file how to pimp your biplot further:
?biplot.rda

#You can even go beyond that, and use the ggbiplot package.
#You can install this package, by running:
library(devtools)
install_github("ggbiplot", "vqv")
library(ggbiplot)

```

<center> <img src="{{ site.baseurl }}/PCAresult.png" alt="Img" style="width: 800px;"/> </center>

In contrast to some of the other ordination techniques, species are represented by arrows. This implies that the abundance of the species is continuously increasing in the direction of the arrow, and decreasing in the opposite direction. Thus PCA is a ‘linear method’. PCA is extremely useful when we expect species to be linearly (or even monotonically) related to each other. Unfortunately, we rarely encounter such a situation in nature. It is much more likely that species have a unimodal species response curve. 

<center> <img src="{{ site.baseurl }}/Unimodalresp.png" alt="Img" style="width: 800px;"/> </center>

Unfortunately, this linear assumption causes PCA to suffer from a serious problem, the horseshoe effect, which makes it unsuitable for most ecological datasets. The PCA solution is often distorted into a horseshoe shape (with the toe either up or down) if beta diversity is moderate to high. The horseshoe can appear even if there is an important secondary gradient. Can you detect a horseshoe shape in the previous plot?

### Principal Coordinate Analysis (PCoA)
Principal coordinates analysis (PCoA, also known as metric multidimensional scaling) attempts to represent the distances between samples in a low-dimensional, Euclidean space. In particular, it maximizes the linear correlation between the distances in the distance matrix, and the distances in a space of low dimension (typically, 2 or 3 axes are selected). The PCoA algorithm is analogous to rotating the multidimensional object such that the distances (lines) in the ‘shadow’ are maximally correlated with the distances (connections) in the object:

<center> <img src="{{ site.baseurl }}/PCOA.gif" alt="Img" style="width: 800px;"/> </center>

The first step of a PCoA is the construction of a (dis)similarity matrix. While PCA is based on Euclidean distances, (dis)similarity matrices calculated from quantitative, semi-quantitative, qualitative, and mixed variables can be handled by PCoA. As always, the choice of (dis)similarity measure is critical and must be suitable to the data in question. If you want to know more about distance measures, please check out our <a href="https://ourcodingclub.github.io/2017/03/21/data-clustering.html#Distance" target="_blank">Intro to data clustering</a>. For abundance data, Bray-Curtis distance is often recommended. You can use Jaccard index for presence/absence data. When the distance metric is Euclidean, PCoA is equivalent to Principal Components Analysis. Although PCoA is based on a (dis)similarity matrix, the solution can be found by eigenanalysis. The interpretation of the results is the same as with PCA. 

```r
#First step is to calculate a distance matrix
dist<-vegdist(varespec,  method="bray")

#PCoA is not included in vegan. We will use the ape package instead
library(ape)
PCOA<-pcoa(dist)

#plot the eigenvalues and interpret
barplot(PCOA$values$Relative_eig[1:10]
#Can you also calculate the cumulative explained variance of the first 3 axes?

#Some distance measures may result in negative eigenvalies.
# If you have negative eigenvalues, then add a correction:
PCOA<-pcoa(dist, correction="cailliez")

#Plot your results
biplot.pcoa(PCOA)
biplot.pcoa(PCOA,varespec)

#Extract scores from first two PCoA axes:
PCOAaxes<-PCOA$vectors[,c(1,2)]

#Compare this result with the PCA plot
par(mfrow=c(1, 2)) 
biplot.pcoa(PCOA)
plot(PCA)

#reset plot window
par(mfrow=c(1, 1)) 
```

<center> <img src="{{ site.baseurl }}/PCOA.png" alt="Img" style="width: 800px;"/> </center>

PCoA suffers from a number of flaws, in particular the arch effect (see PCA for more information). These flaws stem, in part, from the fact that PCoA maximizes a linear correlation. Non-metric Multidimensional Scaling (NMDS) rectifies this by maximizing the rank order correlation.

## Non-metric Multidimensional Scaling (NMDS)
NMDS attempts to represent, as closely as possible, the pairwise dissimilarity between objects in a low-dimensional space. Any dissimilarity coefficient or distance measure may be used to build the distance matrix used as input. NMDS is a rank-based approach. This means that the original distance data is substituted with ranks. Thus, rather than object A being 2.1 units distant from object B and 4.4 units distant from object C, object C is the "first" most distant from object A while object C is the "second" most distant. While information about the magnitude of distances is lost, rank-based methods are generally more robust to data which do not have an identifiable distribution.

NMDS is an iterative algorithm. NMDS routines often begin by random placement of data objects in ordination space. The algorithm then begins to refine this placement by an iterative process, attempting to find an ordination in which ordinated object distances more closely match the order of object dissimilarities in the original distance matrix. The stress value reflects how well the ordination summarizes the observed distances among the samples.

NMDS is not an eigenanalysis. This has three important consequences:
1) There is no unique ordination result
2) The axes of the ordination are not ordered according to the variance they explain
3) The number of dimensions of the low-dimensional space must be specified before running the analysis

There is no unique solution. The end solution depends on the random placement of the objects in the first step. Running the NMDS algorithm multiple times to ensure that the ordination is stable is necessary, as any one run may get "trapped" in local optima which are not representative of true distances. Note: this automatically done with the ´metaMDS()´ in ´vegan´

Axes are not ordered in NMDS. ´metaMDS()´ in ´vegan´ automatically rotates the final result of the NMDS using PCA to make axis 1 correspond to the greatest variance among the NMDS sample points. This doesn´t change the interpretation, cannot be modified, and is a good idea, but you should be aware of it.

A plot of stress (a measure of goodness-of-fit) vs. dimensionality can be used to assess the proper choice of dimensions. The stress values themselves can be used as an indicator. Stress values >0.2 are generally poor and potentially uninterpretable, whereas values <0.1 are good and <0.05 are excellent, leaving little danger of misinterpretation. Stress values between 0.1 and 0.2 are useable but some of the distances will be misleading. Finding the inflexion point can instruct the selection of a minimum number of dimensions.

<center> <img src="{{ site.baseurl }}/NMDSstress.PNG" alt="Img" style="width: 800px;"/> </center>


Methodology of NMDS
Step 1: Perform NMDS with 1 to 10 dimensions
Step 2: Check the stress vs dimension plot
Step 3: Choose optimal number of dimensions
Step 4: Perform final NMDS with that number of dimensions
Step 5: Check for convergent solution and final stress

```r
#First step is to calculate a distance matrix. See PCOA for more information about the distance measures
#Here we use bray-curtis distance, which is recommended for abundance data
dist<-vegdist(varespec,  method="bray")

#In this part, we define a function NMDS.scree() that automatically performs a NMDS for 1-10 dimensions and plots the nr of dimensions vs the stress
#
NMDS.scree<-function(x) { #where x is the name of the data frame variable
  plot(rep(1,10),replicate(10,metaMDS(x,autotransform=F,k=1)$stress),xlim=c(1,10),ylim=c(0,0.30),xlab="# of Dimensions",ylab="Stress",main="NMDS stress plot")
  for (i in 1:10) {
    points(rep(i+1,10),replicate(10,metaMDS(x,autotransform=F,k=i+1)$stress))
  }
}

#Use the function that we just defined to choose the optimal nr of dimensions
NMDS.scree(dist)
```
<center> <img src="{{ site.baseurl }}/NMDSscree.png" alt="Img" style="width: 800px;"/> </center>

On this graph, we don´t see a data point for 1 dimension. Do you know what happened? Tip: Run a metaNMDS with one dimension. Then adapt the function above to fix this problem.

We further see on this graph that the stress decreases with the number of dimensions. This is a normal behavior of a stress plot. This graph doesn´t have a very good inflexion point. So here, you would select a nr of dimensions for which the stress meets the criteria. This would be 3-4 D. To make this tutorial easier, let´s select 2 dimensions. This also an ok solution. Now, we will perform the final analysis with 2 dimensions 

```r
#Because the final result depends on the initial random placement of the points, we´ll set a seed to make the results reproducible
set.seed(2)

#Here, we perform the final analysis and check the result
NMDS1<-metaMDS(dist,k=2,trymax=100,  trace = F)
NMDS1

#If you don´t provide a dissimilarity matrix, metaMDS automatically applies Bray-Curtis. So in our case, the results will not change
NMDS2<-metaMDS(varespec,k=2,trymax=100)
NMDS2
#metaMDS has indeed calculated the Bray-Curtis distances, but first applied a square root transformation on the community matrix

#Check the help file for metaNMDS and try the function for NMDS2, so that the automatic transformation is turned of 


stressplot(NMDS1)


plot(NMDS1, type = "t")

plot(NMDS1, display = "sites", type = "n")
points(NMDS1, display = "sites", cex = 2*gof/mean(gof))
ordiplot(NMDS1,type="n")
orditorp(NMDS1,display="species",col="red",air=0.01)
orditorp(NMDS1,display="sites",cex=1.25,air=0.01)

treat=c(rep("Treatment1",12),rep("Treatment2",12))
# First, create a vector of color values corresponding of the 
# same length as the vector of treatment values
colors=c(rep("red",12),rep("blue",12))
ordiplot(NMDS1,type="n")
#Plot convex hulls with colors baesd on treatment
for(i in unique(treat)) {
  ordihull(NMDS1$point[grep(i,treat),],draw="polygon",
   groups=treat[treat==i],col=colors[grep(i,treat)],label=F) } 
orditorp(NMDS1,display="species",col="red",air=0.01)
orditorp(NMDS1,display="sites",col=c(rep("red",12),
  rep("blue",12)),air=0.01,cex=1.25)

```

<a name="section3"></a>

# 3. Interpretation of the results

More text, code and images.


data(varechem)
vare.nmds<-metaMDS(varespec, trace=F)
ef<-envfit(vare.nmds, varechem, permu = 999)
plot(vare.nmds,type="t",display="sites")
plot(ef,p.max=0.1)

# 4. Your turn

data(dune)

This is the end of the tutorial. Summarise what the student has learned, possibly even with a list of learning outcomes. In this tutorial we learned:

##### - how to generate fake bivariate data
##### - how to create a scatterplot in ggplot2
##### - some of the different plot methods in ggplot2

We can also provide some useful links, include a contact form and a way to send feedback.

For more on `vegan` and how to use it for multivariate analysis of ecological communities, read this <a href="http://cc.oulu.fi/~jarioksa/opetus/metodi/vegantutor.pdf" target="_blank">vegan tutorial</a>.

Another good website to learn more about statistical analysis of ecological data is <a href="https://sites.google.com/site/mb3gustame/home" target="_blank">GUSTA ME</a>

To construct this tutorial We borrowed 

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
