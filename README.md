Read Me File: 
THIS FILE IS ALSO PRESENT IN A NORMAL TEXT FILE TO ALLOW FOR BOLDING TO MAKE IT EASIER TO FIND FILES 

Structure:

Each bolded item represents a file. It would be easiest to follow the order below during review. 

Note on Resources Folder: 
- was used to hold some files ( big ones removed in order to allow upload to GitHub.) Also had several codebooks for the databases. 

Ordered steps with file that accomplishes them: 

1. Obtain NIS data: 
	a. NIS 2016  extraction: File with extraction of NIS 2016 data. The data was coded using ICD-10. Weights for each discharge carrying a diagnosis of MI or Influenza were summed up to create an estimate of the actual number of discharges with that diagnosis in the US.  Two output files sent to Output folder. First file has monthly incidence estimates for MI and Influenza. The second file has the same but stratified by US Census Division ( HOSP_DIV) 
	b. NIS ICD_9 Years: File with extraction for all files that were coded using ICD-9. The method is similar to the NIS 2016 extraction file but the codes are different. 
	c. ( NIS - 2015 - ICD 10, NIS - 2015 ICD 9): NIS  year 2015 is a transition year with both ICD 9 and ICD 10 codes. Extraction files for both.
	d. Combining Regional NIS: File that simply merges output files from the above extraction files to produce one summary data frame summarizing all years.  

2. Obtain BFRS data: 
	a. BFRS data : data is extracted from a list of files. Relevant columns are extracted and then the data frames merged. 

3. Obtain Temperature data: 
	a. Temperatures from API:  file that makes calls to the CDO API to obtain temperature data for each state. Successive attempts are made to call each station in a list of stations for that state. The first station to have temperature data for all required months is used ( 10 attempts before give up) . States are then assigned a number corresponding to their census division using a for loop and lists. In order to weight the state temperature by the corresponding population size to division size ratio ==> an excel file is read and used to create a new column with population size for each state. A lambda function is used in combination with a bumpy weighted mean function and applied on the groupby in order to produce weighted proportions for risk factors and flu vaccine use in each division. The file is then sent to output. 

4. Prepare the Regional Data: 
	a. Preparing Regional Data: 	Temperature and BRFSS files from step 2 and 3 are loaded. We start with BRFSS file. We use a .map function to sort through all the state numbers in the file and assign the corresponding census division. We define a formula wprop to give weighted proportions and we apply them on the groupby object. Since the data only has yearly information but needs to be merged with data having monthly input, we duplicate each row 12 times. we then add a month column and apply a lambda function to input the numbers up to 12 then reset by using ( x + 12) %(12) +1. The temperature data is read and extracted onto our data frame using the following formula: 
for n in range(0,539):
    extracting_temp['Temperature Mean'][n] = Temperature_df.iloc[int((n+1)/60)+1, n%60]
essentially what this is doing is reading the same row for 60 times before switching to the next index for row. For column, it keeps switching to the next index until it gets to the 60th column then resets to start reading from the beginning in conjunction with the movement to the next row. 
The data is sent to an output file. 

5. Analyze and plot : 
	a. Plotting Data and Analysis for total: Merges the total files into one before plotting. There is a part where I experiment by getting data about how effective each year’s vaccine was to see if that impacted results. This was not used in presentation. 

	b. Analyzing Summary Regional: The data from the above sources are combined and used to do exploratory analysis. We also import census data so as to obtain the median age for each census division ( this was mainly for use in SPSS for multivariate analysis.)# PROJECT 1
