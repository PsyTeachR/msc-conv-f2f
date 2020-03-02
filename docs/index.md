
--- 
title: "MSc Conversion in Psychological Studies/Science"
date: "2020-03-02"
author: "Emily Nordmann, Heather Cleland-Woods, Phil McAleer"
bibliography:
- book.bib
- packages.bib
description: This is the materials for the one-year MSc Conversion in Psychological Studies/Science.
documentclass: book
link-citations: yes
site: bookdown::bookdown_site
biblio-style: apalike
---





# Overview {-}

<img src="images/msc_conv.png" style="width: 200px">

## Notes for instructors

### Overview of MSc Conversion course

This book contains the quantitative research methods materials for students on the MSc Conversion in Psychological Studies/Science. This course is a one-year postgraduate degree where students who already hold a non-psychology undergraduate degree receive a British Psychological Society accredited MSc. The students are typically a diverse cohort and range from those with no STEM or programming background to engineering and computing science graduates. Compared to the undergraduate degree, the students are older, and there is a greater incidence of computer anxiety.

As a consequence of the intense nature of the programme and the diversity of the cohort, the approach to R is slightly different to that taken in the undergraduate programmes. The focus for the MSc is to provide a basic but solid competency in core data skills and statistics that can be built on in further study. Students who wish to push themselves beyond the core competencies are encouraged to consult the MSc Data Skills course where they can learn about e.g., simulation and custom functions. To support those students who may have very limited computer literacy, the beginning stages are more supported than in the undergraduate programme e.g., with an increased use of screenshots and explanations for terminology.

### Course structure

All students on the MSc must complete Research Methods 1 (RM1) and Research Methods 2 (RM2) in the first and second semester respectively. These are 10 week, 20 credit courses that cover all aspects of research design and statistics. In RM1, the focus is predominantly on research design and qualitative methods, with only 2 lectures and 3 one-hour labs devoted to R and statistics. By the end of RM1, students should understand descriptive statistics and probability, and as their first inferential test, chi-square. RM2 is solely focused on quantitative methods. In the lectures students cover t-test, correlation, power and effect size, ANOVA, and regression. There are 10 one-hour labs, 5 of which are devoted to a quantitative mini-project, and 5 of which focus on R.

### Teaching open and reproducible science

It is important to remember that this course is not an R course. It is a research methods course that uses R as a tool to teach reproducible science. Whilst students cover a fairly standard set of statistics, we ensure that they do so with reproducibility at the forefront. 

For each type of analysis and design students are taught:

* How to wrangle their data into an appropriate format & document this process
* How to create informative visualisations (we do not show our students how to create bar plots of means) 
* How to check the assumptions for each test and possible options for how to deal with failed assumptions
* How to calculate power and effect size for each type of analysis
* How to correct for multiple comparisons  
* What decisions they should pre-register for each type of analysis (this information is included in the lectures)

We believe that this is preferable to stand-alone lectures and labs where e.g., power and effect size are explained but then are not a foundational component of  practical statistical training. By ensuring that these considerations are covered in each lab and/or lecture, we can reinforce their importance and make them part of a student's standard analysis pipeline.

### Assessment

Students' skills in R are assessed through regular R worksheets that form a portfolio. In RM1, students will complete three worksheets, the first of which is formative to help them understand the assessment procedure. The second and third worksheets cover data wrangling and chi-square and ask them to repeat tasks that are covered in the relevant chapters with new data. In RM2, students complete four worksheets which cover correlation, t-test, ANOVA, and regression. Each summative worksheet is worth 5% of their total course grade. All worksheets are created and assessed using the `assessr` package. If you would like to know more about `assessr`, [please see here](https://github.com/dalejbarr/assessr).

In addition to the portfolio, students complete a quantitative mini-project where they analyse secondary data. This is split into two summative assessment components, first, a pre-registration that includes a research proposal,  pre-registration, and analysis code performed on simulated data, and second, a standard research report of their project. The requirements for the project are that students must conduct a minimum of one t-test and a correlation. This is the first quantitative report that most of our students will ever have written, and our ethos is that it is better to do simple, reproducible analyses than to push them into a complicated car crash. By using assessments such as these we can reinforce the principles of reproducibility, for example, students are assessed on whether they have transparently report any deviations from the pre-registration.

### Contact details

This book should be considered a living document. Parts of it are still a work in progress and the content will be updated and edited as part of the natural lifespan and progress of teaching materials. If you have any feedback on this book or would like more information about the course, please contact <Emily.Nordmann@Glasgow.ac.uk>.

## Student overview

In RM1 and RM2 you will learn core data skills that allow you to manipulate and analyse quantitative data, a key component of an accredited psychology programme. Each week we will build your skills through pre-class, in-class, and homework activities. In addition to this book, there are video walkthroughs of each in-class activity available on Moodle and there will be drop-in help sessions run by our Graduate Teaching Assistants.

The ability to work with quantitative data is a key skill for psychologists and by using R as our tool we can also promote reproducible research practices. Although it may seem like writing a programming script is more time-consuming than other point-and-click software you may have used, this is not the case! Once you have a script you can easily re-run your analysis without having to go through each step again manually which is a) easier and b) less likely to result in errors if you do something slightly different or forget one of the steps. 

Crucially, with an analysis script other researchers can also see how you got from the raw data to the statistics you report in your final paper. Sharing  analysis scripts online on sites such as the [Open Science Framework](https://osf.io/) is now seen as an important open science practice. Even if you don't continue with quantitative research in the future, the skills you develop on this course will allow you to evaluate quantitative research and to understand what goes on behind the scenes with data before the conclusions are presented.

## How to use this book

For each R lab for both RM1 and RM2 there will be pre-class and in-class activities. **It is crucial that you attempt the pre-class activities before attending your lab**. You will learn R much more easily if you work on it consistently each week and give yourself time to build and practice your skills. This is so important to your success on this course that your tutor will keep a record of your progress each week. If you are comfortable with R and/or have programming experience, please feel free to work through this book at your own pace and complete more advanced chapters.

## Data files

If you are a student at the University of Glasgow then the relevant data files will be made available on Moodle each week. You can also download them all as a zip file. <a href="all_data.zip" download>Click to Download.</a>

## Intended Learning Outcomes

By the end of this course students will be able to:

* Clean and wrangle data into appropriate forms for analysis
* Visualise data using a range of plots
* Conduct and interpret a core set of statistical tests (chi-square, t-test, correlation, ANOVA, regression)
