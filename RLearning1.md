Lesson2 is about importing and exporting different types of data in  RStudio.

At first I had some trouble following the instructions of importing the CSV file. Then I found I didn’t pass the argument 
install.packages("readr")
library(readr)

And I need to go to the Environment window (I opened the History window initially), click the data file in the global environment list to read it. Or I can type “View(data)” in the console. It was quite a small problem, yet it did take me some time to find the file I’d already imported. 

We can use read.csv() to import CSVs. Since the default mode of RStudio is to interpret CSVs as factors, so we need to pass the argument stringsAsFactors=FALSE.

I used the readr library, so I could use read_csv( )without stringsAsFactors=FALSE.
url <- "https://data.ct.gov/api/views/iyru-82zq/rows.csv?accessType=DOWNLOAD"
data <- read_csv(url)
And we can use str(data) to see the structure of the data.

![pic1](https://github.com/rhondaqian/JMM692-post/blob/master/scs1.jpg)

To import a csv, we can both directly link it from the website, or download it to local. The address or file name need to be put in quotation marks, and I got an error first because I used an unexpected token””””.

![pic2](https://github.com/rhondaqian/JMM692-post/blob/master/scs2.jpg)

local_file <- "Admissions_to_DMHAS_Addiction_Treatment_by_Town__Year__and_Month.csv"
data_local <- read_csv(local_file)

We use write_csv() to export a csv. We pass the name of the file we want to save as in the brackets. We can pass the address where we want to move the slash in quotation marks, or it will save to the current working directory by default. We can also use slash to add a subdirectory. If the program don’t understand na, then pass na=“ ”.

After practicing with CSVs, importing excel seems easier. 
We need to download the excel file first, and need a package specifically for excel files. 

install.packages("readxl")
library(readxl)
drug_death <- read_excel("DrugDeaths.xlsx", skip=2)
The function from readxl is similar to that in readr. We pass skip= to skip column names.

![pic3](https://github.com/rhondaqian/JMM692-post/blob/master/scs3.jpg)

colnames(drug_death) is a function to list the column names of the dataframe.
![pic4](https://github.com/rhondaqian/JMM692-post/blob/master/scs4.jpg)

We pass “drug_death$`42005`” to clean up column names.
And to change the column names, we pass argument like 
colnames(drug_death)[colnames(drug_death)=="X42005"] <- “Year”

![pic5](https://github.com/rhondaqian/JMM692-post/blob/master/scs5.jpg)

All we can use library(dplyr), and use the function rename() from dplyr.
drug_death <- rename(drug_death, Place=X29)
To get rid of NAs, we can pass subset() and filter from dplyr.
drug_death <- subset(drug_death, !is.na(Year))
drug_death <- filter(drug_death, !is.na(Year))

![pic6](https://github.com/rhondaqian/JMM692-post/blob/master/scs6.jpg)


When dealing with delimited text, we may encounter pipe-delimited files or tab-delimited files.
To read in delimited pipe files, we use read_delim() function from readr library.

df1 <- read_delim("data/Employee_Payroll_Pipe.txt", delim=“|”)

![pic7](https://github.com/rhondaqian/JMM692-post/blob/master/scs7.jpg)

We pass read_tsv()to import tab delimited pipe files.

We use the read_fwf() function from the readr package for fixed width column. We need to pass the width of each variables and the names of the columns.

![pic8](https://github.com/rhondaqian/JMM692-post/blob/master/scs8.jpg)

Fo JSON data, we first need to find a json file on the website.
Then we use the jsonlite library.
And we use the fromJSON() function.

install.packages("jsonlite")
library(jsonlite)
json_url <-"http://sbgi.net/resources/assets/sbgi/MetaverseStationData.json"
stations <- fromJSON(json_url)
![pic9](https://github.com/rhondaqian/JMM692-post/blob/master/scs9.jpg)


SPSS stands for Statistical Package for the Social Sciences and is owned by IBM. SPSS file is layered. The data have a label and value. And it’s too big for Excel to handle.
If we want to take the SPSS file, we need to save it as a data frame in R. 
The library foreign is needed.
install.packages("foreign")
library(foreign)
data_labels <- read.spss("data/SHR76_16.sav", to.data.frame=TRUE)
To bing it in without the labels, we use a variable
data_only <- read.spss("data/SHR76_16.sav", to.data.frame=TRUE, use.value.labels=F)

![pic10](https://github.com/rhondaqian/JMM692-post/blob/master/scs10.jpg)

Then we need to bring the two data frames together, while perverse both labels and values.
We use dplyr packages. And create a new data frame each for new labels and new value.
![pic12](https://github.com/rhondaqian/JMM692-post/blob/master/scs12.jpg)

Then we pass cbind() function which means column binding, but it only works if the number of rows are the same.
![pic11](https://github.com/rhondaqian/JMM692-post/blob/master/scs11.jpg)


