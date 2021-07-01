# EPA RSEI Geographic Microdata on AWS

[Risk-Screening Environmental Indicators (RSEI) Geographic Microdata](https://registry.opendata.aws/epa-rsei-pds/) from the United States Environmental Protection Agency (EPA) is available for anyone to use via Amazon S3.

## About RSEI Geographic Microdata

[EPA's RSEI Geographic Microdata](https://www.epa.gov/rsei) is a unique dataset that provides detailed air model results from EPA’s Risk-Screening Environmental Indicators (RSEI) model. RSEI models chemical releases and transfers reported to the from EPA’s Toxics Release Inventory (TRI), which tracks toxic chemical releases and waste management activities at industrial and federal facilities across the United States and territories. RSEI contains results for over 400 TRI-listed chemicals and 45,000 TRI-reporting facilities.

You can learn more about this data at [the RSEI website](https://www.epa.gov/rsei), and learn more about TRI reporting at the [Toxics Release Inventory Program website](https://www.epa.gov/toxics-release-inventory-tri-program).

The results include chemical concentration, toxicity-weighted concentration and score, calculated for each 810 meter square grid cell in a 49-km circle around the emitting facility, for every year from 1988 through 2014. As with any model, RSEI is subject to the limitations of the underlying data sources and models that it incorporates. You should carefully consider the impact that the RSEI method may have on the results for any analysis. The [RSEI Microdata Guidance](http://www.epa.gov/rsei/guidance-risk-screening-environmental-indicators-rsei-geographic-microdata-rsei-gm) contains important information about using the RSEI Microdata, and the [RSEI methodology document and appendices](http://www.epa.gov/rsei/methodology-and-supporting-documents-risk-screening-environmental-indicators-rsei-model) contain full documentation of methods and data sources used in the RSEI model.

The data can be used to examine trends in air pollution from industrial facilities over time and across geographies. Users can examine relationships between RSEI impacts and population demographics for environmental justice analyses. RSEI data are unique because they provide a fully comparable 27-year time series, as well as a national matched source-receptor relationship – that is, users can match the estimated impact on an area to the facilities responsible.

## Accessing RSEI Geographic Microdata on AWS

RSEI Microdata is available in the `epa-rsei-pds` Amazon S3 bucket in the US East region.

If you use the AWS Command Line Interface, you can list the contents of the bucket with this command:

`aws s3 ls epa-rsei-pds`

This information is also available in the listing for RSEI Microdata on the [Registry of Open Data on AWS](https://registry.opendata.aws/epa-rsei-pds/).

### Microdata files in CSV format

A RSEI Microdata file is a CSV file in which each row represents the metrics for a single chemical release as it affects a single grid cell. RSEI data on AWS provides files that include all Microdata for each year, as well as files that include Microdata broken down by state for each year. All files are compressed using gzip.

### Year Microdata Files

Annual RSEI Microdata files are available in `s3://epa-rsei-pds/v234/microdata` and are named `vxxx_micro_yyyy.csv.gz`, where `xxx` refers to the data version number, and `yyyy` refers to the year. For example, the file for 1989 RSEI data is at `s3://epa-rsei-pds/v234/microdata/v234_micro_1989.csv.gz`.

### State Microdata Files

RSEI Microdata files broken down by state are available in `s3://epa-rsei-pds/v234/microdata/<two-character state identifier>` and are named `vxxx_micro_state_ss_yyyy.csv.gz`, where `xxx` refers to the data version number, `ss` refers to the state, and `yyyy` refers to the year.

For example, the file for Utah's 2002 RSEI data is at `s3://epa-rsei-pds/v234/microdata/ut/v234_micro_state_ut_2002.csv.gz`.

A list of state identifiers is at `s3://epa-rsei-pds/v234/data_tables/states.csv`.

### RSEI CSV Columns

Each row of a Microdata file refers to a single 810m grid cell, and contains scores, concentrations, and toxicity-weighted concentrations for each chemical release. Note that if two releases for the same chemical (either from different facilities or one from a stack release and one from a fugitive release from the same facility) affect the same grid cell, there will be separate rows for each grid cell/release combination.

Each Microdata CSV contains 13 columns, described in the table below.

<table class="table table-striped">
  <thead>
    <tr>
      <th>Column Number</th>
      <th>Name</th>
      <th>Description.</th>
    </tr>
  </thead>  
  <tbody>
    <tr>
      <td>1</td>
      <td>GridCode</td>
      <td>
        <p>Identifies grid.</p>
        <ul> 
          <li>14=Conterminous US</li> 
          <li>24=Alaska</li> 
          <li>34=Hawaii</li> 
          <li>44=Puerto Rico/Virgin Islands</li> 
          <li>54=Guam/Marianas</li> 
          <li>64=American Samoa</li> 
        </ul>
      </td> 
    </tr> 
    <tr>
      <td>2</td> 
      <td>X</td> 
      <td>X-coordinate of grid.<br> </td> 
    </tr> 
    <tr> 
      <td>3</td> 
      <td>Y</td> 
      <td>Y-coordinate of grid.<br> </td> 
    </tr> 
    <tr> 
      <td>4</td> 
      <td>ReleaseNumber</td> 
      <td>Internal unique identifier for release. Identifiers are described in the <a href="http://epa-rsei-pds.s3.amazonaws.com/v234/data_tables/release.csv.gz">"Release" lookup table</a> (46 MB CSV compressed using gzip).</td>
    </tr>
    <tr>
      <td>5</td>
      <td>ChemicalNumber</td>
      <td>Internal unique identifier of released chemical. Identifiers are described in the <a href="http://epa-rsei-pds.s3.amazonaws.com/v234/data_tables/chemical.csv">"Chemical" lookup table</a> (208 KB CSV).</td>
    </tr>
    <tr> 
      <td>6</td> 
      <td>FacilityNumber</td> 
      <td>
        <p>Internal unique identifier of releasing facility.</p>
        <ul>
          <li>If "Media" = 1 or 2, look up the facility in the <a href="http://epa-rsei-pds.s3.amazonaws.com/v234/data_tables/facility.csv.gz">"Facility" lookup table</a> (7 MB CSV compressed using gzip)</li>
          <li>If "Media" = 6, 750 or 754, look up the facility in the <a href="http://epa-rsei-pds.s3.amazonaws.com/v234/data_tables/offsite.csv.gz">"Offsite" lookup table</a> (5 MB CSV compressed using gzip)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>7</td>
      <td>Media</td>
      <td>Code describing media into which chemical is released. Codes are described in the <a href="http://epa-rsei-pds.s3.amazonaws.com/v234/data_tables/media.csv">"Media" lookup table</a> (2 KB CSV).
      </td>
    </tr>
    <tr>
      <td>8</td>
      <td>Conc</td>
      <td>Concentration of chemical for release/media at grid cell.</td>
    </tr>
    <tr>
      <td>9</td>
      <td>ToxConc</td>
      <td>Concentration multiplied by inhalation toxicity weight.</td>
    </tr> 
    <tr> 
      <td>10</td> 
      <td>Score</td> 
      <td>Risk-related score (surrogate dose &times; toxicity weight &times; population).</td> 
    </tr> 
    <tr> 
      <td>11</td> 
      <td>ScoreCancer</td> 
      <td>Risk-related score (surrogate dose &times; toxicity weight &times; populationonly toxicity values for cancer effects.</td> 
    </tr> 
    <tr> 
      <td>12</td> 
      <td>ScoreNonCancer</td> 
      <td>Risk-related score (surrogate dose &times; toxicity weight &times; populationonly toxicity values for noncancer effects.</td> 
    </tr> 
    <tr> 
      <td>13</td> 
      <td>Pop</td> 
      <td>Number of people in grid cell (may be interpolated).</td> 
    </tr>
  </tbody>
</table>

### RSEI Data Lookup Tables

The Microdata uses internal identifiers that link to subject lookup tables that contain names and parameters used for chemicals, facilities, etc. Some of those tables are linked to in the table above, and all lookup tables can be found at `s3://epa-rsei-pds/V234/data_tables`. Field names and descriptions can be found in [the RSEI data dictionary](https://www.epa.gov/rsei/risk-screening-environmental-indicators-rsei-data-dictionary).