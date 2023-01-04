---
title:  "The day I collected all Milesian inscriptions from the *Searchable Greek Inscriptions*-tool in a few minutes and then did not do anything with it"
date:   2023-01-04 12:32:00 +0100
categories: update
---

This is neither a great feat nor particularly hard, but I figured I would note it here, just in case me or someone else can do anything with this tiny guide. Sometime during the holidays I got bored and thought it would be great to make a sort of diagram about the inscriptions of Miletus -- all of them! 

Now, we do not (currently) have a database with them, and basically all the information is in books and articles and would take a ridiculous amount of time to compile manually. This is boring, so I decided not to. I did want to try to webscrape the inscriptions.packhum.org-site though, mainly to see what happens. It turns out it is absolutely easy, but the resulting data is not what I want. Anyway, here is how to do it: 

## The "Searchable Greek Inscriptions"-Tool

The ["Searchable Greek Inscriptions"-Tool](https://inscriptions.packhum.org/) is a great site hosted by the Packard Humanities Institute. It contains basic information and the text about and of a plethora of inscriptions compiled from different publications. Each inscription has an identifier that is prefixed with "PH".

So, I navigated to one of the "Inschriften von Milet"-books ([Milet VI,2](https://inscriptions.packhum.org/book/880?location=1688)) and saw that the identifiers are consecutive from the first one in the book to the last. Each inscription can also be found by its identifier in the URL, e.g. [PH351132](https://inscriptions.packhum.org/text/351132) (though without the prefix "PH" for the URL) for the first one, and [PH351810](https://inscriptions.packhum.org/text/351810) for the last one in this book.

## The Basic Idea

Now, if we want to get this into R, we need the html-reading capabilities of the `rvest`-package:


{% highlight r %}
install.packages("rvest")
library(rvest)
{% endhighlight %}

We can note one of our identifiers, lets say PH351132 as the first one in the book, and simply get the complete html of the site under this address into memory. We won't be able to view it, but it doesn't matter. 


{% highlight r %}
id <- 351132
url <-  paste("https://inscriptions.packhum.org/text/", id, sep = "")
page <- read_html(url)
str(page)
{% endhighlight %}



{% highlight text %}
## List of 2
##  $ node:<externalptr> 
##  $ doc :<externalptr> 
##  - attr(*, "class")= chr [1:2] "xml_document" "xml_node"
{% endhighlight %}

The only thing that is left for us -- basically -- is knowing what we want. [This tutorial](https://www.r-bloggers.com/2020/04/tutorial-web-scraping-in-r-with-rvest/) explains what to look for and how to find it when trying to understand html and get the info you need to select the elements from a html-page that you want to scrape. As an example: In the html of each inscriptions' page at the [Searchable Greek Inscriptions-Tool](https://inscriptions.packhum.org/) is a line that links the title of the corresponding book to the books index: 


{% highlight html %}
<a href="/book/880?location=1688" class="booklink">Milet VI,2</a>
{% endhighlight %}

As you can see, the link has the `class="booklink"`, and its contents are the (short) name of the book, in this case "Milet VI,2".

We can extract this from the page we just loaded into memory using the `html_element()`-function to select it, and then the `html_text()`-function to extract it as a character. (Please note that from here on out, the code needs to be able to use tidyverse-grammar.)


{% highlight r %}
library(tidyverse)
page %>%
  html_element(".booklink") %>% 
  html_text()
{% endhighlight %}



{% highlight text %}
## [1] "Milet VI,2"
{% endhighlight %}
This would be the same result for the second note below the reference on the site: 


{% highlight r %}
page %>%
  html_element(".ti") %>% 
  html_text()
{% endhighlight %}



{% highlight text %}
## [1] "Ionia — Miletos — 5th c. BC — SbBerlin (1900.1) 111 (mention) — Kadmos 37 (1998) 164-165, n. 3 (cf.)"
{% endhighlight %}

## Looping in a Function

To make it easier for me, I wrote a function that would safe all the information I can get into a dataframe. Because this takes an annoying amount of time, it sometimes echoes how far it has progressed, just to keep track. You feed it only a vector of all the identifiers you want to scrape, and then it does just that. 

This is not particularly fast or efficient, so if you plan on getting 163.202 inscriptions, maybe don't. 


{% highlight r %}
collect_packhum <- function(refs = 351132:351134) {
  collection <- as.data.frame(matrix(nrow = length(refs), ncol = 7))
  colnames(collection) <- c("book", "inv", "ph_ref", "note_one", "note_two", "text", "url")
  rownames(collection) <- refs
  
  print("Starting....")
  # loop for every reference number
  for (i in 1:length(refs)) {
    url <-  paste("https://inscriptions.packhum.org/text/", refs[i], sep = "")
    
    # get the whole page (probably inefficient)
    page <- read_html(url)
    
    # Short name of the Book
    collection$book[i] <- page %>%
      html_element(".booklink") %>% 
      html_text()
    
    # Inventory number as referenced after the book
    collection$inv[i] <- page %>%
      html_element(".ref") %>% 
      html_text()
    
    # packhum reference number, though it is technically the same as refs[i]
    ph_ref <- page %>%
      html_element(".docref") %>% 
      html_text() 
    # remove newline
    collection$ph_ref[i] <- gsub("\\n", "", ph_ref)
  
    # the note (first line of entry)
    collection$note_one[i] <- page %>%
      html_element(".note") %>% 
      html_text()
    
    # the note containing the dating (second line of entry)
    collection$note_two[i] <- page %>%
      html_element(".ti") %>% 
      html_text()
    
    # text of the inscription, will probably have formatting issues
    collection$text[i] <- page %>%
      html_element(".text-nowrap") %>% 
      html_text()
    
    # the url, just in case, though we could easily reconstruct it
    collection$url[i] <- url
    
    # this part is just for keeping track
    prog <- pretty(refs, n = ifelse(length(refs > 100), 100, 10))
    perc <- i / length(refs)
    perc <- round(perc * 100, digits = 1)
    if (refs[i] %in% prog) {
      print(paste("... ", perc, "% done", sep = ""))
    }
  }
  print("Done!")
  return(collection)
}
{% endhighlight %}

To collect your inscriptions, then, just run the function with a bunch of identifiers: 


{% highlight r %}
refs <- c(351132:351134)
ivm <- collect_packhum(refs)
{% endhighlight %}



{% highlight text %}
## [1] "Starting...."
## [1] "Done!"
{% endhighlight %}

Now we can save this for later as a csv, and take a look at the resulting dataframe, which now contains basically a table of all that information:


{% highlight r %}
write.csv(ivm, "ivm_table_example.csv")
ivm
{% endhighlight %}
<table class=" lightable-material lightable-striped lightable-hover" style='font-family: "Source Sans Pro", helvetica, sans-serif; margin-left: auto; margin-right: auto;'>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> book </th>
   <th style="text-align:left;"> inv </th>
   <th style="text-align:left;"> ph_ref </th>
   <th style="text-align:left;"> note_one </th>
   <th style="text-align:left;"> note_two </th>
   <th style="text-align:left;"> text </th>
   <th style="text-align:left;"> url </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 351132 </td>
   <td style="text-align:left;"> Milet VI,2 </td>
   <td style="text-align:left;"> 407 </td>
   <td style="text-align:left;"> PH351132 </td>
   <td style="text-align:left;"> Epitaph of Androssos (?) of Halikarnassos.  Upper part of a small stele of bluish marble. </td>
   <td style="text-align:left;"> Ionia — Miletos — 5th c. BC — SbBerlin (1900.1) 111 (mention) — Kadmos 37 (1998) 164-165, n. 3 (cf.) </td>
   <td style="text-align:left;"> 1

&lt;U+0391&gt;&lt;U+03BD&gt;d&lt;U+03C1&gt;&lt;U+03BF&gt;ss&lt;U+0323&gt;-



&lt;U+03C9&gt;d&lt;U+03BF&gt;&lt;U+03C2&gt; &lt;U+1F09&gt;&lt;U+03BB&gt;-



&lt;U+03B9&gt;&lt;U+03BA&gt;a&lt;U+03C1&gt;&lt;U+03BD&gt;a-



ss&lt;U+1F73&gt;&lt;U+03C9&gt;&lt;U+03C2&gt;. </td>
   <td style="text-align:left;"> https://inscriptions.packhum.org/text/351132 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 351133 </td>
   <td style="text-align:left;"> Milet VI,2 </td>
   <td style="text-align:left;"> 408 </td>
   <td style="text-align:left;"> PH351133 </td>
   <td style="text-align:left;"> Epitaph of Herostratos, son of Python.  Block of white marble; late archaic script. </td>
   <td style="text-align:left;"> Ionia — Miletos — Kalabaktepe — early 5th c. BC </td>
   <td style="text-align:left;"> 1

&lt;U+1F29&gt;&lt;U+03C1&gt;&lt;U+03BF&gt;st&lt;U+03C1&gt;&lt;U+1F71&gt;t&lt;U+03BF&gt;



&lt;U+1F10&gt;µ&lt;U+1F76&gt; s&lt;U+1FC6&gt;µa



t&lt;U+03BF&gt;&lt;U+0342&gt; &lt;U+03A0&gt;&lt;U+1F7B&gt;&lt;U+03B8&gt;&lt;U+03C9&gt;&lt;U+03BD&gt;&lt;U+03BF&gt;&lt;U+03C2&gt;. </td>
   <td style="text-align:left;"> https://inscriptions.packhum.org/text/351133 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 351134 </td>
   <td style="text-align:left;"> Milet VI,2 </td>
   <td style="text-align:left;"> 409 </td>
   <td style="text-align:left;"> PH351134 </td>
   <td style="text-align:left;"> Epitaph of Leontis.  Two joining fragments of a block or plaque of light gray marble, broken on all sides. </td>
   <td style="text-align:left;"> Ionia — Miletos — early 5th c. BC </td>
   <td style="text-align:left;"> 1

&lt;U+039B&gt;&lt;U+1F73&gt;&lt;U+03BF&gt;&lt;U+03BD&gt;t&lt;U+03B9&gt;&lt;U+03C2&gt;. </td>
   <td style="text-align:left;"> https://inscriptions.packhum.org/text/351134 </td>
  </tr>
</tbody>
</table>

Obviously, the text did not fare well, but it should not be impossible to reformat it into readability. Sadly (but we could have foreseen this) we do not actually have any information that lends itself to a fast and easy clean-up, but the first step (getting it from the site into your R-memory) is done, at least. If you are so inclined, I guess cleaning the chronological information up in an Excel-Sheet is always a lot easier than to copy and paste everything from the homepage and then STILL clean it manually... But with a rather long and involved process you would also be able to automate it, somewhat. See, as an example, [this monstrosity of data cleaning](https://cran.r-project.org/web/packages/datplot/vignettes/data_preparation.html), that may or may not have been easier to do by hand. 

## Conclusion

This was nice, but also didn't help in any way. Anyhow -- if you could use this function, go ahead and have fun.

