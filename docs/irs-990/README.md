# IRS 990 Filings on AWS

Machine-readable data from certain electronic 990 forms filed with the IRS from 2011 to present are available for anyone to use via Amazon S3.

Form 990 is the form used by the United States [Internal Revenue Service](https://www.irs.gov/) to gather financial information about nonprofit organizations. Data for each 990 filing is provided in an XML file that contains structured information that represents the main 990 form, any filed forms and schedules, and other control information describing how the document was filed. Some non-disclosable information is not included in the files.

This data set includes Forms 990, 990-EZ and 990-PF which have been electronically filed with the IRS and is updated regularly in an XML format. The data can be used to perform research and analysis of organizations that have electronically filed Forms 990, 990-EZ and 990-PF. Forms 990-N (e-Postcard) are not available withing this data set. Forms 990-N can be viewed and downloaded from the [IRS website](https://www.irs.gov/charities-non-profits/search-for-forms-990-n-filed-by-small-tax-exempt-organizations).

## Accessing IRS 990 Filings on AWS

Each electronic 990 filing is available as a unique XML file in the "irs-form-990" S3 bucket in the US East (N. Virginia) region. [Schemas for electronic 990 filings are available on the IRS website](https://www.irs.gov/charities-non-profits/current-valid-xml-schemas-and-business-rules-for-exempt-organizations-modernized-e-file). Each filing is named based on the year it was filed and a unique identifier. For example, we can tell that the filing named "201541349349307794_public.xml" was filed in 2015 because the file name starts with "2015." "41349349307794" is the unique identifier of the filing.

All of the data is publicly accessible via the S3 bucket's HTTPS endpoint at https://s3.amazonaws.com/irs-form-990. No authentication is required to download data over HTTPS. For example, the example filing mentioned above can be accessed at https://s3.amazonaws.com/irs-form-990/201541349349307794_public.xml.

Index listings of available filings are available in JSON and CSV files, organized based on the year they were filed. Index files exist for each year going back to 2011 and are named based on their year and file type. For example, the CSV index for 2011 is available at https://s3.amazonaws.com/irs-form-990/2011_index.csv, and the JSON index file for 2015 is available at https://s3.amazonaws.com/irs-form-990/index_2015.json
These index files includes basic information about each filing, including the name of the filer, the Employer Identification Number (EIN) of the filer, the date of the filing, and unique identifier for the filing.

If you use the [AWS Command Line Interface](https://aws.amazon.com/documentation/cli/), you can list the index files and calculate the total size of the files with [the following "ls" command](http://docs.aws.amazon.com/cli/latest/reference/s3/ls.html):

`aws s3 ls s3://irs-form-990/index --human-readable --summarize`
