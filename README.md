# OpenRefine: Cleaning data so it is ready for analysis!
<img src="images/____.png" width="150"/>

OpenRefine is...


## Workshop Resources
 - Software - access via the [OpenRefine's homepage](https://openrefine.org/)
 - Documentation - [OpenRefine's Manual](https://openrefine.org/docs)
 - Data - download from this dropbox [OpenRefine workshop data folder](https://www.dropbox.com/___________)

## Workshop Goals
By the end of this workshop, you will be able to:
- Use OpenRefine to accelerate the data cleaning process

## Outline
- [Why do we need clean data?](#why-clean-data)
- 

## <a name="why-clean-data"></a>  Why do we need clean data?
Why do we need clean data?


## GREL
For data manipulation, Open Refine uses GREL (General Refine Expression Language).

https://openrefine.org/docs/manual/grel


## External Data - using APIs

Add column by fetching URLs!!!

some guidance from https://towardsdatascience.com/dataset-manipulation-with-open-refine-a5043b7294a7

Uses the opac.sbn.it website and its API to snag ISBNs....INTERESTING. 

check with " if (condition, true expression, false expression) "

if (value.length () == 1, null, load_identification_code)

To load the identification code, you need to make the call to the API. To do this, just enter the API URL in quotes and add any variables using the + operator. In our case it is necessary to specify the ISBN, which from time to time will be equal to the value of the current line (value):

“http://opac.sbn.it/opacmobilegw/search.json?isbn=" + value
