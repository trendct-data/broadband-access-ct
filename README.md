# FCC 477

The FCC collects data from Internet service providers on the type and speed of Internet access they provide. Geographically, it's very detailed. It's available at the block level, the smallest U.S. Census Bureau designated area. The key data point I looked at was the maximum advertised upload speed and the max advertised download speed for consumer-level Internet service in each census tract.

Some drawbacks are, it doesn't indicate actual speeds, but advertised speeds.

If you don't look at the data at the block level -- say, if you group it into Census tracts or even the town level -- you won't see the lack of availability of high speed broadband in some areas.

# Approach

In order to present the data at the town level, I calculated how the percentage of Census blocks in each town that had slow Internet access. I set the cutoff at the federal definition of broadband, which is 25 Mbps down/ 3 up. So if a town has 52% slow blocks, that 52% of the Census tracts in each town had a maximum advertised speed below 25/3.

# Findings

I found that 16 percent of Census tracts that had Internet access, had only sub-broadband service available.

Anecdotally it looks like the rural towns suffer more, but population density didn't explain why Hartford, which is a very dense city, has 27% percent slow-Internet blocks. New Haven, another city, had 23% slow blocks. Many towns had zero slow blocks.

# Files

* Internet speeds by census block and town.ipynb - Python analysis notebook
* data/CT-Fixed-Jun2015-v3.csv - From FCC - The main data set used for this analysis from CT, June 2015
* data/Provider_List_Jun2015_v3.xlsx - From FCC - List of ISPs with some information about each. Not much info.
* data/tracts_to_towns.csv - A table for mapping census tracts to towns. Compiled by Andrew Ba Tran.
