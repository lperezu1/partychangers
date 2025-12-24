# partychangers
In this project, I use three databases to identify candidates who may have changed their party affiliation ahead of next year’s primary and general elections.


# The databases are: 

- the statewide voter registration file found here: https://www.ncsbe.gov/results-data/voter-registration-data. According to the North Carolina State Board of Elections, these files "contain the most up-to-date publicly available information for individuals registered or formerly registered to vote in North Carolina, as well as individuals who have attempted to register or have steps left uncompleted in the registration process."  An important note from the file layout documentation  (https://s3.amazonaws.com/dl.ncsbe.gov/data/layout_ncvoter.txt) is that the voter_reg_num is unique only within a county. As a result, voters in different counties can share the same voter registration number 

- The other database is the party change file pulled from here: https://dl.ncsbe.gov/?prefix=data/PartyChange/  I pulled the file dated 12/1/2025.That file does not show the names of the people who changed parties. 

- The final file is the 2026 candidate list also maintianted by the NCSBE  [https://www.ncsbe.gov/results-data/candidate-lists  ](https://s3.amazonaws.com/dl.ncsbe.gov/Elections/2026/Candidate%20Filing/Candidate_Listing_2026.csv) 
 
# Method 
- I removed inactive and removed voters from the statewide voter registration file.

- I then joined the voter registration file with the party change file using county and voter registration number. This allowed names and other voter details to be attached to individuals who changed party affiliation. The party change file unfortunately does not list an NC ID. 

Although the voter registration file is the most up-to-date list of voter registrations, per the NCSBE, about 3,000 records from the party change file did not match to the voter registration file. In these cases, there was no record in the voter registration file with both the same county and voter registration number as those listed in the party change file.

I also checked the county-by-county voter registration files maintained by the NCSBE and found no matches there either. All unmatched records reflect voters who changed party affiliation in 2025, making the absence of corresponding voter registration records unclear.

- Despite this, there were still 145,651 matches so the over 3724 is just over 2.5% of the total. The party change file has 149,375 rows.
- Next, I pulled the candidate list from the NCSBE and filtered and edited it so there was one row per unique candidate. I also grouped counties into a single field listing all counties in which each candidate is running. The original candidate file lists multiple rows per candidate because candidates often run in districts that span multiple counties.

-Finally, I matched the unique candidate list to the dataset created by joining the party change and voter registration files. This match was done using the name as it appears on the ballot, but only when there was also a county match and when the individual’s party affiliation matched the party to which they had changed.
- This left a database comprised of 156 rows. This dataset needs to be checked manually as there can be instances where there may not in fact be a match. You can see those mathces in the candidates_party_change2026.csv file, which you can also access via this google sheets link: https://docs.google.com/spreadsheets/d/1XqE8iEuPcFNKZeG6BRyrB3eQiuelCUMXLduG_fUF2Ss/edit?usp=sharing 
