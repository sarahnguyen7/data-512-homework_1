# HCD_HW1
# Monthly Wikipedia Rare Disease Pageviews Analysis

## Project Goal
The goal of this assignment is to construct, analyze, and publish a dataset of monthly article traffic for a select set of rare disease pages from English Wikipedia from July 1, 2015 through September 30, 2024. Pageview data was pulled from desktop, mobile-web, and mobile-app using the Wikipedia API. This is shown through `HCD_Homework1.ipynb`.

## Table of Contents

- [License](#license)
- [API Documentation](#api-documentation)
- [Data Files](#data-files)
- [Data Schema](#data-schema)
- [Known Issues](#known-issues)
- [Instructions for Use](#instructions-for-use)
- [Functions](#functions)

## Source Data
The rare diseases data is a [subset](https://drive.google.com/drive/u/0/folders/1aR9pDJV2KWMSl_LR7BgMR5smwqqAHbWw) of Wikipedia article pages. This list of pages was collected by using a database of rare diseases maintained by the National Organization for Rare Diseases (NORD) and matching them to Wikipedia articles that are either about a rare disease or have a section that mentions a rare disease.

## License
The code written was developed in combination of myself, ChatGPT, Dr. David. W. McDonald for use in Data 512, a UW MS Data Science Degree program. The authors of the code will be referenced in the specifc code they were used in. Licenses for all are listed down below. 

#### Wikipedia API Example Code
The source data is governed by the [Wikimedia Foundation terms of use](https://foundation.wikimedia.org/wiki/Terms_of_use). The datasets created from this project is subject to the same terms, allowing for reuse and redistribution with appropriate attribution.

#### Dr. David W. McDonald's Example Code
This code example was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024

#### Sarah Nguyen's Code
This code was developed by Sarah Nguyen for use in DATA 512 homework, which is coursework provided under the [MIT License](https://chatgpt.com/c/67048ab0-3d24-8001-88ce-c354bb934b32#:~:text=under%20the%20MIT-,License,-.).

#### GPT-4
Portions of this project were assisted by GPT-4, an AI model by OpenAI.  These are indicated in the notebook and subject to Open AI's [Terms of Use](https://openai.com/policies/row-terms-of-use/). It was also used to help rewrite some of my documentation to help standardize format.

## API Documentation

For more details about the Wikimedia API used in this project, please refer to the following resources:
- [Wikimedia REST API](https://wikimedia.org/api/rest_v1/)

## Data Files
Input file:
- `rare-disease_cleaned.AUG.2024.csv`
- 
The following intermediary and output files are generated by this code and are located in `Output_Data`:

Intermediary files which contain API request for mobile data:
- `rare-disease_pageviews_mobile-web201507-202409.json`
- `rare-disease_pageviews_mobile-app201507-202409.json`

Final Data Input Files which contain API request for all mobile, desktop, and cumulative data:
- `rare-disease_pageviews_desktop201507-202409.json`
- `rare-disease_pageviews_mobile201507-202409.json`
- `rare-disease_pageviews_cumulative201507-202409.json`

The following plots are located in `Output_Graphs` and address our basic visualization analysis prompts:
- `max_min_avg.png`
- `top_10_peak.png`
- `fewest_months_of_data.png`

## Data Schema
The schema was designed to look up by disease name/article title to help with indexing. 
Each JSON mentioned above file contains the following structure:

```json
{
    "article_title": {
        [
            {
                "project": "string",
                "granularity": "string",
                "timestamp": "YYYYMMDDHH",
                "agent": "string",
                "views": "integer"
            }
        ]
    }
}
```
## Known Issues
Some issues a user may encounter URL encoding issues, non-disease articles in the dataset, and tied rankings in the analysis. 

URL Encoding Issues:

Some article titles containing special characters, particularly a forward slash (/), caused issues when requesting pageview data from the Wikimedia API. As a result, pageview data for articles with slashes in the title could not be retrieved, and these articles are not included in the final JSON output.
Non-Disease Articles in the Dataset:

The dataset includes several articles that are not directly related to diseases but are disease-adjacent. Examples include "Yellowstone Park bison herd" and "Maui dolphin," which are topics that have sections related to diseases or are in close proximity to disease-related content. These articles are included in the data but may not be relevant to the primary focus on diseases.

Rank Ties in Graphs:

In some of the generated graphs, certain diseases have tied rankings based on their pageview data. However, these ties are not visually indicated on the graphs, and only the top 10 articles are displayed. This may result in tied diseases being excluded from the displayed results.


## Instructions for Use

Ensure you have Python 3.8 and above to install the required packages:
- `pip install requests`
- `pip install pandas` 
- `pip install matplotlib`

## Functions
- `request_pageviews_per_article` (created by Dr. McDonald) is an API request made using one function to make the code resuable. It has paramters but relies on constants listed in the Notebook.
- `collect_pageviews` is a function that takes an article title and platform as inputs. It  collects/requests the pageview data from the REST API using the function request_pageviews_per_article (originally developed by Dr. McDonald). It formats it into the desired input and saves the data for each access point
If successful, the function returns the JSON response containing pageview data for the given article and platform. If the request fails, it prints an error message.
- `load_data` is a function that opens the file path for a list of file paths
- `combine_json_objects` is a function to combine pageview data across JSON files based on article (disease) and timestamp
- `combine_files` is a function to combine data from multiple files and save the result
- `json_to_df`is a function to convert JSON data to Pandas DataFrame which makes it easier for data manipulation
- `setup_plot` is designed to be a helper function for setting up the plot since they all use the same axis
- `save_plot` is a function that saves and shows the plot


