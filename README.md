# How long are murder cases taking in Cook County?
This analysis is one of several for a project the Chicago Tribune is doing on delays in the Cook County court system. It uses [R](https://www.r-project.org/) code, with source data from the Cook County State’s Attorney’s office which is updated several times a year. (Here’s a [nice primer](https://www.cookcountystatesattorney.org/about/open-data) on its data.) This analysis uses data published by that office on 1/23/2023. If you run this code with future files, you should expect different results.

I've included an R project file (Length_of_cases.Rproj) that walks you through the code. There's also rmd and html versions, if you prefer. It may simply be easier to download a zip file of this entire repository and work from your computer. (Side note: I'm a relative Github newbie, so I may be making rookie mistakes in how I upload these files. Apologies in advance.)

### **Getting the data**
This is all explained in the project, rmd and html files, but a quick recap.

The biggest thing is getting the raw data. Github doesn't allow hosting huge datasets -- and these are huge. I've stuggled finding ways to finagle them through Large File Storage. So you'll need to go a bit old school-ish, create a folder called raw_data in your working directory, and then put three files in there.

You can download the most up-to-date data here:    
-SA_dispositions *(Updated data [here](https://datacatalog.cookcountyil.gov/api/views/apwk-dzx8/rows.csv?accessType=DOWNLOAD).)*   
-SA_initiations *(Updated data [here](https://datacatalog.cookcountyil.gov/api/views/7mck-ehwz/rows.csv?accessType=DOWNLOAD).)*   
-SA_sentencings *(Updated data [here](https://datacatalog.cookcountyil.gov/api/views/tg8v-tm6u/rows.csv?accessType=DOWNLOAD).)*

But if you'd prefer, it may be easier to snag all three from my [Google Drive file](https://drive.google.com/drive/folders/1oP5FXeJV98sO1oUkh1UHbMIj2mb1nKga?usp=sharing).

The key file is the dispositions file. It has unique IDs for each case and each participant in each case. And it offers, among other things, a date each case was disposed and a date of each subject's arrest. (Comparing the two will tell you how long each case took.) Of course, like most datasets, it's not perfect. A sliver of disposition records were missing arrest dates. To try to correct for that, I looked for arrest dates for these cases in the other two datasets, which also chronicled some aspects of each case (including, in theory, an arrest date). Unfortunately, those datasets also were missing arrest dates for this sliver of cases. Still, I've included that double-check code anyway, in case it's helpful for others pursuing this kind of research in the future.

### **Preparing/ crunching the data**

Again, there's more detail in the actual project, rmd and html files, but the gist is to work with the Dispositions data. You'll need to clean it up a bit. We're defining a case as person-based. So if somebody is charged in the same case with 10 different charges, it's one case, not 10. That means the dataset needs to be massaged to find the most serious charge -- then be filtered for just for those categorized as murder.

Another challenge is figuring out when a case was "disposed." Most people are charged with multiple things, and not all charges are listed as disposed on the same date. Because of that, I had to decide *when* a case was truly disposed. Here's my logic:  
-If someone was found guilty of anything, the case disposition date is the *first* date that the person was found guilty of anything. That's because, on that date, the person's status switched from being accused but not convicted of anything, to being convicted of some level of wrongdoing related to the case.  
-If someone was not found guilty of any charge, the disposition date is the date that the *last* charge was disposed. That's because, on that date, the person left the court no longer formally accused of any wrongdoing.  

This analysis follows the common convention of only measuring the days to get to a result of guilt or innocence, and does not include any delays in sentencing someone found guilty. Also, this analysis measures from the arrest date. In Cook County, the court technically starts one case after an arrest, and then another at an arraignment. When the court measures how long cases take, it typically measures just that second case (from arraignment). But when most poeple think of how long a case has taken, they think of it lasting from the arrest date. So that's what we use.

### **Final thoughts**

You'll see some other things in this repository too that you may find helpful:    
-A *cleaned data* file, which is the half-way point data. It's the set of whittled down cases from which we do the counting.    
-A *final data* file, if you just want to skip to the good stuff. 

### **Questions/ comments?**

Contact Joe Mahr at jmahr@chicagotribune.com
