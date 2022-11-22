# Assessing the Impact of Affordable Housing Development

In the last decade, Davidson County has experienced tremendous population growth. With this population growth has come skyrocketing housing costs. From 2010 to 2019, both home values and rents have grown by more than 150%, while wages increased only slowly. High housing costs can squeeze household budgets, reducing the money available for other daily needs, including food, clothing, health care, utilities, and transportation, as well as money needed for education or for future savings.

One method of addressing rising housing costs is by building affordable housing developments. Despite the potential benefits, property owners who live near proposed housing developments often oppose such projects, citing fear that the developments will cause their property values to decline or will increase crime rates.

In this project, you'll be examining the impact of housing units built in Davidson under the the Low Income Housing Tax Credit (LIHTC) or which were funded by the [Barnes Housing Trust Fund](https://www.nashville.gov/departments/mayor/housing/barnes-fund). Established in 1986, the LIHTC program has become an integral component of federal housing policy, funding 21 percent of all multifamily developments over the period 1987-2008. The Barnes Fund, created in 2013, is Metro Nashville's first housing trust fund and was created to leverage affordable housing developments throughout Davidson County.

You have been provided with several data sources. These data sources can be downloaded from here: https://drive.google.com/drive/folders/1zWaLMIVQQuZXp4HHvtdeGpOZ5QYeWGPT?usp=share_link

* filtered_sales.csv: This dataset was obtained from the [Nashville Planning Department's Parcel Viewer](https://maps.nashville.gov/ParcelViewer/). It contains sales of single family homes from 1995 to the present day. It has been filtered to remove sales that are likely not arms-length transactions and transactions for parcels which did not have a house on them at the time of sale. This was done by removing any transations for $0, any transactions for which the adjacent appraisal values showed $0 for improvents, and any for which the transaction amount was less than half of the adjacent appraisals. If you would like to work with the full dataset, all transactions are contained in full_sales.csv and the assessment values are in assessment.csv.
* LIHTC.csv: Contains information on all Davidson County developments from the Department of Housing and Urban Development (HUD) National Low Income Housing Tax Credit Database.
    - For more information about the variables contained in this dataset, see the included data dictionary
* barnes.csv: Contains information on rental properties that are completed and have more than 10 units which were funded by the Barnes Fund.
* property_details.csv: Contains detailed information on each property. Obtained from the [Metro Nashville Assessor of Property's website](https://www.padctn.org/). Includes year built, square footage, number of rooms, and location (lat/lng).

For this project, we'll be focusing on single family homes and look at affordable housing developments that were placed in service in 2000 or later.
## Part 1: Statistical Analysis
For the first part of this project, you'll mimic the methodology of the working paper ["Does Federally Subsidized Rental Housing Depress Neighborhood Property Values?"](https://furmancenter.org/research/publication/does-federally-subsidized-rental-housing-depress-neighborhood-property), building a statistical model to explore the effect on sales price of a home being within a half mile of an affordable housing development.

1. Using the sf library, find the closest development to each home. Hint: You can convert a tibble to an sf object using the [`st_as_sf` function](https://r-spatial.github.io/sf/reference/st_as_sf.html). See, for example, this stackoverflow post: https://gis.stackexchange.com/questions/222978/lon-lat-to-simple-features-sfg-and-sfc-in-r. Once converted, you can use the [`get_nearest_feature` function](https://r-spatial.github.io/sf/reference/st_nearest_feature.html).
2. Calculate the distance from each home its closest development.
3. Filter the homes down to those that are within one mile of an affordable housing development.
4. For each remaining home, calculate a new column called "group", which is defined according to the following rules. Hint: Use the `case_when` function to do this.  
	* "pre" - for homes where the distance is less than half a mile and whose sale date was 2-5 years prior to the input year  
	* "mid" - for homes where the distance is less than half a mile and whose sale date was 0-2 years prior to the input year  
	* "post" - for homes where the distance is less than half a mile and whose sale date was after the input year  
	* "outside" - for homes where the distance is more than half a mile and whose sale date was no more than 5 years prior to the input year  
	* "other" - All other rows  
5. Filter out all rows whose group is "other".
6. Add an id column containing the id value for the development.
7. Create a column "Tpost" that, for homes in the "post" group gives the number of years that the sale occurred after the housing development was placed in service.
8. Create a column named "age" which gives the age of the home at the time of sale.
9. Filter down to only sales that took place within the five years before or after the associated development was placed in service. Then build a linear model with target variable the sales amount using the following features:
	- square_footage
	- age of home at time of sale
	- group
	- year
	- tract
How can you interpret the coefficients of this model?
10. Now, try a model with target being the log of the sale price.
	- square_footage
	- age of home at time of sale
	- group
	- year
	- tract
How can you interpret the coefficients of this model?
11. Continue to explore the data to see if you can improve the models you have.

**Bonus:** [Assessing the Impact of Affordable Housing on Nearby Property Values in Alexandria, Virginia](https://www.urban.org/research/publication/assessing-impact-affordable-housing-nearby-property-values-alexandria-virginia) also looks at the impact of affordable housing developments on nearby property values, but uses a different model which focuses only on homes that were sold both before and after the development was placed in service. Use a similar setup to analyze the data from Davidson County.

## Part 2: R Shiny App for Further Exploration
Build an R Shiny app to facilitate further exploration. Your app should allow the user to select an affordable housing development and get information about nearby home sales and trends in those sales prices over time. Optionally, your app can also display demographic or other data about the surrounding area.