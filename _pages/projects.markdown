---
title: Projects
layout: single
permalink: /projects/
---

# My Projects

I have several small projects mainly related to the development of R-packages or other software for archaeological purposes that I work on and / or maintain. They are listed here in no particular order. 

## [idaifieldR](https://github.com/lsteinmann/idaifieldR)

[idaifieldR](https://github.com/lsteinmann/idaifieldR) is an R-package that allows to import data from Field Desktop into R with a single command, and somewhat pre-format this data to make working on it in R easier. I published an [article about the package](https://doi.org/10.34780/068b-q6c7) in the [Forum for Digital Archaeology and Infrastructure](https://publications.dainst.org/journals/FdAI/index), which contains a tutorial on why and how to use the package. 

## [milQuant](https://github.com/lsteinmann/milQuant-dist) 

milQuant is an app for quantitative visualization of find data imported from Field Desktop. It uses shiny and shinyDashboard in R to make it accessible for people without programming skills. I used Electron to produce an installable version for my colleagues at the excavation, where the app is actively used.

## [csvGeom](https://github.com/msingr/csvGeom)

csvGeom originally was a messy python script that I only used myself to reformat csv-outputs of older total stations into GeoJSON to facilitate importing data into the excavation database Software [Field Desktop](https://github.com/dainst/idai-field). Together with Marcel Singer, we transformed it into a more user-friendly and robust little App that poses a light-weight alternative to importing and re-exporting data with GIS software to the format needed by Field Desktop.

## [chrongler](https://github.com/lsteinmann/chrongler)

I am currently working on creating an R-package that helps reformat and 'wrangle' categorical chronological data as commonly used in archaeology. It should replace periods with "grouped" periods (e.g. Early, Middle and Late Imperial with Imperial Period) so switch between different levels of accuracy in dating, and replace categorical values with absolute dates according to a user specified chronological framework and vice verse. The package is under development (but I am using some version of it as part of milQuant).

## [datplot](https://github.com/lsteinmann/datplot)

Together with Barbora Weissova, we produced an R-package that prepares chronological object data for display in density plots. The idea originated in the RuColA-group in Bochum when discussing Barbora Weissovas approach to dated inscriptions in her dissertation, which we used as a case study in the [related article](https://doi.org/10.1017/aap.2021.8).  The package is published on CRAN and actively maintained. 

## [Duration of Days](https://github.com/lsteinmann/DurationOfDays)
*Duration of Days* helps travellers calculate their visa allowance upon entering a country they have been to before (most often limited to the somewhat confusing phrase "90 days within the last 180 days"). I originally used a tiny script to keep track of my own travels, but since many of my colleagues had the same problem, I made it accessible to others in the form of a tiny shiny app ([available online](https://lsteinmann.shinyapps.io/DurationOfDays/)).

# Miletus

As part of my work for the Miletus Excavation, I produced and maintained several (online) resources related to Miletus. 

## [Website of the Miletus Excavation](https://www.miletgrabung.uni-hamburg.de/)

The concept of the [website of the Miletus Excavation](https://www.miletgrabung.uni-hamburg.de/) was originally designed by Christof Berns along with several students during a seminar at Universität Hamburg. I was responsible for realizing their concept in the CMS of the Universität Hamburg and have been maintaining, updating and extending the site until the end of 2023. 

## [The Miletus Bibliography](https://github.com/Miletus-Excavation/Miletus_Bibliography)

The bibliography of the Miletus excavation had first been collected and [published by Sabine Huy in the form of three pdf-documents](https://doi.org/10.25592/uhhfdm.8678) sorted by year, author and keyword. I maintained the bibliography between 2022 and 2024. With the collaboration of several students (mainly Caitlin Bamford and Silas Munnecke) we have transformed it into a [Zotero group library](https://www.zotero.org/groups/4475959/milet_bibliography). I created a series of scripts in R and Python to automatically download, serve and update the database in various formats. Each month, this library is automatically fetched and exported to several formats suitable for different literature management solutions and also to the three pdf-documents via a [GitHub-repository (Miletus Bibliography)](https://github.com/Miletus-Excavation/Miletus_Bibliography), so that we can offer it for [download](https://www.miletgrabung.uni-hamburg.de/en/material/bibliographie.html) on the website of the excavation. 

## [Map of Miletus](https://geoserver.dainst.org/catalogue/#/map/5764)

During the MegaMil-project, I conceptualized an online [Map for the city of Miletus](https://www.miletgrabung.uni-hamburg.de/en/material/karten.html) which can be used as a guide through the city and a phase plan, as well as to produce publication ready plans and maps. It is hosted on the [iDAI.geoserver](https://geoserver.dainst.org/catalogue/#/map/5764). The map has been created in close collaboration with P. A. Cardona Villamil, who is responsible for all digital drawings and color designs, and several students who collected the background information on all buildings from past publications. The [Map of Miletus](https://geoserver.dainst.org/catalogue/#/map/5764) is a piece-by-piece recreation of the plan of Berthold Weber, and was significantly expanded with new surveying data, newly georeferenced plans and new research. A [series of scripts](https://github.com/Miletus-Excavation/Miletus_Buildingcatalogue) transform and split the data for publication as the multi-period phase plan that you can see in the map.

## [Miletus Documentation Manual](https://doi.org/10.25592/uhhfdm.10029)

On my initiative, we collaborated with several members of the excavation to compile a documentation of the excavations documentation and inventory system. The [Miletus Documentation Manual](https://doi.org/10.25592/uhhfdm.10029) has since been available online and as a limited print version alongside all forms and resources we use during our work in Miletus. The goal of this manual is to guide the many different groups of researchers working in Miletus to use a unified system of documentation to keep our results comparable and to transparently communicate our methods to the public. 

