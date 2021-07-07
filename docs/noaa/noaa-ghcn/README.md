# NOAA Global Historical Climatology Network Daily (GHCN-D)

GHCN-Daily is a dataset that contains daily observations over global
land areas. It contains station-based measurements from land-based
stations worldwide, about two thirds of which are for precipitation
measurements only (Menne et al., 2012). GHCN-Daily is a composite of
climate records from numerous sources that were merged together and
subjected to a common suite of quality assurance reviews (Durre et al.,
2010). The archive includes the following meteorological elements:

-   Daily maximum temperature
-   Daily minimum temperature
-   Temperature at the time of observation
-   Precipitation (i.e., rain, melted snow)
-   Snowfall
-   Snow depth
-   Other elements where available

In this archive, the period of record station files are parsed into
yearly files that contain all available GHCN-Daily station data for that
year plus a time of observation field (where available---primarily for
U.S. Cooperative Observers). The observation times for U.S. Cooperative
Observer data come from the station history archived in NCDC's
Historical Observing Metadata Repository (HOMR). The files are updated
daily on AWS to be in sync with updates to the GHCN-Daily dataset at
NOAA.

This document refers to two other files, ghcnd-stations.txt and
ghcnd-inventory.txt, which are also stored in the noaa-ghcn-pds S3
bucket. You can get these files here:

-   <http://noaa-ghcn-pds.s3.amazonaws.com/ghcnd-stations.txt>: The
    ghcnd-stations file contains summary information for over 160000
    stations used to create this dataset.
-   <http://noaa-ghcn-pds.s3.amazonaws.com/ghcnd-inventory.txt>: The
    ghcnd-inventory file contains the periods of record for each station
    and element.

## Accessing GHCN-D Data on AWS

The GHCH-D data are stored in the noaa-ghcn-pds bucket:

<http://noaa-ghcn-pds.s3.amazonaws.com/>

The directory is structured by year from 1763 to present, with each file
named after the respective year. The data are available in CSV file
format and as .csv.gzip files, so any particular year will be named
yyyy.csv and yyyy.csv.gz. For example to access the gziped version of
the data for 1788 append 1788.csv.gz to the bucket URL:

<http://noaa-ghcn-pds.s3.amazonaws.com/csv.gz/1788.csv.gz>

The uncompressed versions of the files can be accessed by using a
slightly different URL and a filename ending with CSV. For example to
access the uncompressed data for 1788 append 1788.csv to the bucket URL:

<http://noaa-ghcn-pds.s3.amazonaws.com/csv/1788.csv>

If you use the AWS Command Line Interface, you can access the bucket
with this command:

`aws s3 ls noaa-ghcn-pds`

The data set is updated daily.

## Summary of the Day Format

The yearly files are formatted so that every observation is represented
by a single row with the following fields:

-   ID = 11 character station identification code. Please see
    ghcnd-stations section below for an explantation
-   YEAR/MONTH/DAY = 8 character date in YYYYMMDD format (e.g. 19860529
    = May 29, 1986)
-   ELEMENT = 4 character indicator of element type
-   DATA VALUE = 5 character data value for ELEMENT
-   M-FLAG = 1 character Measurement Flag
-   Q-FLAG = 1 character Quality Flag
-   S-FLAG = 1 character Source Flag
-   OBS-TIME = 4-character time of observation in hour-minute format
    (i.e. 0700 =7:00 am)

The fields are comma delimited and each row represents one station-day.

### ELEMENT Summary

The five core elements are:

-   PRCP = Precipitation (tenths of mm)
-   SNOW = Snowfall (mm)
-   SNWD = Snow depth (mm)
-   TMAX = Maximum temperature (tenths of degrees C)
-   TMIN = Minimum temperature (tenths of degrees C)

Please see the **Full Explanation of Elements** section below for a full
description.

#### M-FLAG

MFLAG is the measurement flag. There are ten possible values:

-   Blank = no measurement information applicable
-   B = precipitation total formed from two 12-hour totals
-   D = precipitation total formed from four six-hour totals
-   H = represents highest or lowest hourly temperature (TMAX or TMIN)
    or the average of hourly values (TAVG)
-   K = converted from knots
-   L = temperature appears to be lagged with respect to reported hour
    of observation
-   O = converted from oktas
-   P = identified as "missing presumed zero" in DSI 3200 and 3206
-   T = trace of precipitation, snowfall, or snow depth
-   W = converted from 16-point WBAN code (for wind direction)

#### Q-FLAG

Q-FLAG is the measurement quality flag. There are fourteen possible
values:

-   Blank = did not fail any quality assurance check
-   D = failed duplicate check
-   G = failed gap check
-   I = failed internal consistency check
-   K = failed streak/frequent-value check
-   L = failed check on length of multiday period
-   M = failed mega consistency check
-   N = failed naught check
-   O = failed climatological outlier check
-   R = failed lagged range check
-   S = failed spatial consistency check
-   T = failed temporal consistency check
-   W = temperature too warm for snow
-   X = failed bounds check
-   Z = flagged as a result of an official Datzilla Investigation

#### S-FLAG

S-FLAG is the source flag for the observation. There are twenty nine
possible values (including blank, upper and lower case letters):

-   Blank = No source (i.e., data value missing)
-   0 = U.S. Cooperative Summary of the Day (NCDC DSI-3200)
-   6 = CDMP Cooperative Summary of the Day (NCDC DSI-3206)
-   7 = U.S. Cooperative Summary of the Day -- Transmitted via WxCoder3
    (NCDC SI-3207)
-   A = U.S. Automated Surface Observing System (ASOS) real-time data
    (since January 1, 2006)
-   a = Australian data from the Australian Bureau of Meteorology
-   B = U.S. ASOS data for October 2000-December 2005 (NCDC DSI-3211)
-   b = Belarus update
-   C = Environment Canada
-   E = European Climate Assessment and Dataset (Klein Tank et
    al., 2002)
-   F = U.S. Fort data
-   G = Official Global Climate Observing System (GCOS) or other
    government-supplied data
-   H = High Plains Regional Climate Center real-time data
-   I = International collection (non U.S. data received through
    personal contacts)
-   K = U.S. Cooperative Summary of the Day data digitized from paper
    observer forms (from 2011 to present)
-   M = Monthly METAR Extract (additional ASOS data)
-   N = Community Collaborative Rain, Hail,and Snow (CoCoRaHS)
-   Q = Data from several African countries that had been "quarantined",
    that is, withheld from public release until permission was granted
    from the respective meteorological services
-   R = NCEI Reference Network Database (Climate Reference Network and
    Regional Climate Reference Network)
-   r = All-Russian Research Institute of Hydro-meteorological
    Information-World Data Center
-   S = Global Summary of the Day (NCDC DSI-9618)NOTE: "S" values are
    derived from hourly synoptic reports exchanged on the Global
    Telecommunications System (GTS). Daily values derived in this
    fashion may differ significantly from "true" daily data,
    particularly for precipitation (i.e., use with caution).
-   s = China Meteorological Administration/National Meteorological
    Information Center/Climatic Data Center
    ([http://cdc.cma.gov.cn](http://cdc.cma.gov.cn/))
-   T = SNOwpack TELemtry (SNOTEL) data obtained from the U.S.
    Department of Agriculture's Natural Resources Conservation Service
-   U = Remote Automatic Weather Station (RAWS) data obtained from the
    Western Regional Climate Center
-   u = Ukraine update
-   W = WBAN/ASOS Summary of the Day from NCDC's Integrated Surface Data
    (ISD).
-   X = U.S. First-Order Summary of the Day (NCDC DSI-3210)
-   Z = Datzilla official additions or replacements
-   z = Uzbekistan update

When data are available for the same time from more than one source, the
highest priority source is chosen according to the following priority
order (from highest to lowest): -
Z,R,0,6,C,X,W,K,7,F,B,M,r,E,z,u,b,s,a,G,Q,I,A,N,T,U,H,S

## Full Explanation of the Elements Variable

As mentioned above the five core elements are:

-   PRCP = Precipitation (tenths of mm)
-   SNOW = Snowfall (mm)
-   SNWD = Snow depth (mm)
-   TMAX = Maximum temperature (tenths of degrees C)
-   TMIN = Minimum temperature (tenths of degrees C)

The other elements are:

-   ACMC = Average cloudiness midnight to midnight from 30-second
    ceilometer data (percent)

-   ACMH = Average cloudiness midnight to midnight from manual
    observations (percent)

-   ACSC = Average cloudiness sunrise to sunset from 30-second
    ceilometer data (percent)

-   ACSH = Average cloudiness sunrise to sunset from manual observations
    (percent)

-   AWDR = Average daily wind direction (degrees)

-   AWND = Average daily wind speed (tenths of meters per second)

-   DAEV = Number of days included in the multiday evaporation total
    (MDEV)

-   DAPR = Number of days included in the multiday precipitation total
    (MDPR)

-   DASF = Number of days included in the multiday snowfall total (MDSF)

-   DATN = Number of days included in the multiday minimum temperature
    (MDTN)

-   DATX = Number of days included in the multiday maximum temperature
    (MDTX)

-   DAWM = Number of days included in the multiday wind movement (MDWM)

-   DWPR = Number of days with non-zero precipitation included in
    multiday precipitation total (MDPR)

-   EVAP = Evaporation of water from evaporation pan (tenths of mm)

-   FMTM = Time of fastest mile or fastest 1-minute wind (hours and
    minutes,i.e., HHMM)

-   FRGB = Base of frozen ground layer (cm)

-   FRGT = Top of frozen ground layer (cm)

-   FRTH = Thickness of frozen ground layer (cm)

-   GAHT = Difference between river and gauge height (cm)

-   MDEV = Multiday evaporation total (tenths of mm; use with DAEV)

-   MDPR = Multiday precipitation total (tenths of mm; use with DAPR and
    DWPR, if available)

-   MDSF = Multiday snowfall total

-   MDTN = Multiday minimum temperature (tenths of degrees C; use with
    DATN)

-   MDTX = Multiday maximum temperature (tenths of degrees C; use with
    DATX)

-   MDWM = Multiday wind movement (km)

-   MNPN = Daily minimum temperature of water in an evaporation pan
    (tenths of degrees C)

-   MXPN = Daily maximum temperature of water in an evaporation pan
    (tenths of degrees C)

-   PGTM = Peak gust time (hours and minutes, i.e., HHMM)

-   PSUN = Daily percent of possible sunshine (percent)

-   SN\*\# = Minimum soil temperature (tenths of degrees C) where:

    -   \* corresponds to a code for ground cover

    -   0 = unknown

    -   1 = grass

    -   2 = fallow

    -   3 = bare ground

    -   4 = brome grass

    -   5 = sod

    -   6 = straw mulch

    -   7 = grass muck

    -   8 = bare muck

    -   \# corresponds to a code for soil depth.

    -   1 = 5 cm

    -   2 = 10 cm

    -   3 = 20 cm

    -   4 = 50 cm

    -   5 = 100 cm

    -   6 = 150 cm

    -   7 = 180 cm

-   SX\*\# = Maximum soil temperature (tenths of degrees C) where:

    -   \* corresponds to a code for ground cover (see above)
    -   \# corresponds to a code for soil depth (see above)

-   TAVG = Average temperature (tenths of degrees C) \[Note that TAVG
    from source 'S' corresponds to an average for the period ending at
    2400 UTC rather than local midnight\]

-   THIC = Thickness of ice on water (tenths of mm)

-   TOBS = Temperature at the time of observation (tenths of degrees C)

-   TSUN = Daily total sunshine (minutes)

-   WDF1 = Direction of fastest 1-minute wind (degrees)

-   WDF2 = Direction of fastest 2-minute wind (degrees)

-   WDF5 = Direction of fastest 5-second wind (degrees)

-   WDFG = Direction of peak wind gust (degrees)

-   WDFI = Direction of highest instantaneous wind (degrees)

-   WDFM = Fastest mile wind direction (degrees)

-   WDMV = 24-hour wind movement (km)

-   WESD = Water equivalent of snow on the ground (tenths of mm)

-   WESF = Water equivalent of snowfall (tenths of mm)

-   WSF1 = Fastest 1-minute wind speed (tenths of meters per second)

-   WSF2 = Fastest 2-minute wind speed (tenths of meters per second)

-   WSF5 = Fastest 5-second wind speed (tenths of meters per second)

-   WSFG = Peak gust wind speed (tenths of meters per second)

-   WSFI = Highest instantaneous wind speed (tenths of meters per
    second)

-   WSFM = Fastest mile wind speed (tenths of meters per second)

-   WT\*\* = Weather Type where \*\* has one of the following values:

    -   01 = Fog, ice fog, or freezing fog (may include heavy fog)
    -   02 = Heavy fog or heaving freezing fog (not always distinguished
        from fog)
    -   03 = Thunder
    -   04 = Ice pellets, sleet, snow pellets, or small hail
    -   05 = Hail (may include small hail)
    -   06 = Glaze or rime
    -   07 = Dust, volcanic ash, blowing dust, blowing sand, or blowing
        obstruction
    -   08 = Smoke or haze
    -   09 = Blowing or drifting snow
    -   10 = Tornado, waterspout, or funnel cloud
    -   11 = High or damaging winds
    -   12 = Blowing spray
    -   13 = Mist
    -   14 = Drizzle
    -   15 = Freezing drizzl
    -   16 = Rain (may include freezing rain, drizzle, and freezing
        drizzle)
    -   17 = Freezing rain
    -   18 = Snow, snow pellets, snow grains, or ice crystals
    -   19 = Unknown source of precipitation
    -   21 = Ground fog
    -   22 = Ice fog or freezing fog

-   WV\*\* = Weather in the Vicinity where \*\* has one of the following
    values:

    -   01 = Fog, ice fog, or freezing fog (may include heavy fog)
    -   03 = Thunder
    -   07 = Ash, dust, sand, or other blowing obstruction
    -   18 = Snow or ice crystals
    -   20 = Rain or snow shower

## FORMAT OF "ghcnd-stations.txt" file {#format-of-ghcnd-stations-txt-file}

There are over 106200 stations listed in a seperate file. Found here:

<http://noaa-ghcn-pds.s3.amazonaws.com/ghcnd-stations.txt>

The table below describes the structure of each row of
ghcnd-stations.txt

  Variable       Columns   Type        Example
  -------------- --------- ----------- -------------
  ID             1-11      Character   EI000003980
  LATITUDE       13-20     Real        55.3717
  LONGITUDE      22-30     Real        -7.3400
  ELEVATION      32-37     Real        21.0
  STATE          39-40     Character   
  NAME           42-71     Character   MALIN HEAD
  GSN FLAG       73-75     Character   GSN
  HCN/CRN FLAG   77-79     Character   
  WMO ID         81-85     Character   03980

These variables have the following definitions:

-   ID = the station identification code.
    -   The first two characters denote the FIPS country code
    -   The third character is a network code that identifies the
        station numbering system used
    -   0 = unspecified (station identified by up to eight alphanumeric
        characters)
    -   1 = Community Collaborative Rain, Hail,and Snow (CoCoRaHS) based
        identification number. To ensure consistency with with GHCN
        Daily, all numbers in the original CoCoRaHS IDs have been
        left-filled to make them all four digits long. In addition, the
        characters "-" and "\_" have been removed to ensure that the IDs
        do not exceed 11 characters when preceded by "US1". For example,
        the CoCoRaHS ID "AZ-MR-156" becomes "US1AZMR0156" in GHCN-Daily
    -   C = U.S. Cooperative Network identification number (last six
        characters of the GHCN-Daily ID)
    -   E = Identification number used in the ECA&D non-blended dataset
    -   M = World Meteorological Organization ID (last five characters
        of the GHCN-Daily ID)
    -   N = Identification number used in data supplied by a National
        Meteorological or Hydrological Center
    -   R = U.S. Interagency Remote Automatic Weather Station (RAWS)
        identifier
    -   S = U.S. Natural Resources Conservation Service SNOwpack
        TELemtry (SNOTEL) station identifier
    -   W = WBAN identification number (last five characters of the
        GHCN-Daily ID)
    -   The remaining eight characters contain the actual station ID.

-   LATITUDE = latitude of the station (in decimal degrees).

-   LONGITUDE = longitude of the station (in decimal degrees).

-   STATE = U.S. postal code for the state (for U.S. and Canadian
    stations only).

-   NAME = name of the station.

-   GSN FLAG = flag that indicates whether the station is part of the
    GCOS Surface Network (GSN). The flag is assigned by
    cross-referencing the number in the WMOID field with the official
    list of GSN stations. There are two possible values:

    -   Blank = non-GSN station or WMO Station number not available
    -   GSN = GSN station

-   HCN/CRN FLAG = flag that indicates whether the station is part of
    the U.S. Historical Climatology Network (HCN). There are three
    possible values:

    -   Blank = Not a member of the U.S. Historical Climatology or U.S.
        Climate Reference Networks
    -   HCN = U.S. Historical Climatology Network station
    -   CRN = U.S. Climate Reference Network or U.S. Regional Climate
        Network Station

-   WMO ID is the World Meteorological Organization (WMO) number for the
    station. If the station has no WMO number (or one has not yet been
    matched to this station), then the field is blank.

### Lookup Table of Country Codes

The table of the country is derived from the ghcnd-countries.txt file
available at the link below:

<http://noaa-ghcn-pds.s3.amazonaws.com/ghcnd-countries.txt>

The state codes are used in the station identification number. In the
table below CODE is the FIPS country code of the country where the
station is located.

  Code   Country
  ------ -----------------------------------------------------------------
  AC     Antigua and Barbuda
  AE     United Arab Emirates
  AF     Afghanistan
  AG     Algeria
  AJ     Azerbaijan
  AL     Albania
  AM     Armenia
  AO     Angola
  AQ     American Samoa \[United States\]
  AR     Argentina
  AS     Australia
  AU     Austria
  AY     Antarctica
  BA     Bahrain
  BB     Barbados
  BC     Botswana
  BD     Bermuda \[United Kingdom\]
  BE     Belgium
  BF     Bahamas, The
  BG     Bangladesh
  BH     Belize
  BK     Bosnia and Herzegovina
  BL     Bolivia
  BM     Burma
  BN     Benin
  BO     Belarus
  BP     Solomon Islands
  BR     Brazil
  BU     Bulgaria
  BX     Brunei
  BY     Burundi
  CA     Canada
  CB     Cambodia
  CD     Chad
  CE     Sri Lanka
  CF     Congo (Brazzaville)
  CG     Congo (Kinshasa)
  CH     China
  CI     Chile
  CJ     Cayman Islands \[United Kingdom\]
  CK     Cocos (Keeling) Islands \[Australia\]
  CM     Cameroon
  CO     Colombia
  CQ     Northern Mariana Islands \[United States\]
  CS     Costa Rica
  CT     Central African Republic
  CU     Cuba
  CV     Cape Verde
  CW     Cook Islands \[New Zealand\]
  CY     Cyprus
  DA     Denmark
  DO     Dominica
  DR     Dominican Republic
  EC     Ecuador
  EG     Egypt
  EI     Ireland
  EK     Equatorial Guinea
  EN     Estonia
  ER     Eritrea
  ES     El Salvador
  ET     Ethiopia
  EU     Europa Island \[France\]
  EZ     Czech Republic
  FG     French Guiana \[France\]
  FI     Finland
  FJ     Fiji
  FK     Falkland Islands (Islas Malvinas) \[United Kingdom\]
  FM     Federated States of Micronesia
  FP     French Polynesia
  FR     France
  FS     French Southern and Antarctic Lands \[France\]
  GA     Gambia, The
  GB     Gabon
  GG     Georgia
  GH     Ghana
  GI     Gibraltar \[United Kingdom\]
  GL     Greenland \[Denmark\]
  GM     Germany
  GP     Guadeloupe \[France\]
  GQ     Guam \[United States\]
  GR     Greece
  GT     Guatemala
  GV     Guinea
  GY     Guyana
  HO     Honduras
  HR     Croatia
  HU     Hungary
  IC     Iceland
  ID     Indonesia
  IN     India
  IO     British Indian Ocean Territory \[United Kingdom\]
  IR     Iran
  IS     Israel
  IT     Italy
  IV     Cote D'Ivoire
  IZ     Iraq
  JA     Japan
  JM     Jamaica
  JN     Jan Mayen \[Norway\]
  JO     Jordan
  JQ     Johnston Atoll \[United States\]
  JU     Juan De Nova Island \[France\]
  KE     Kenya
  KG     Kyrgzstan
  KN     Korea, South
  KR     Kiribati
  S      Korea,South
  K      Christmas Island \[Australia\]
  KU     Kuwait
  KZ     Kazakhstan
  LA     Laos
  LE     Lebanon
  LG     Latvia
  LH     Lithuania
  LI     Liberia
  LO     Slovakia
  LQ     Palmyra Atoll \[United States\]
  LT     Lesotho
  LU     Luxembourg
  LY     Libya
  MA     Madagascar
  MB     Martinique \[France\]
  MC     Macau S.A.R
  MD     Moldova
  MF     Mayotte \[France\]
  MG     Mongolia
  MI     Malawi
  MJ     Montenegro
  MK     Macedonia
  ML     Mali
  MO     Morocco
  MP     Mauritius
  MQ     Midway Islands \[United States\]
  MR     Mauritania
  MT     Malta
  MU     Oman
  MV     Maldives
  MX     Mexico
  MY     Malaysia
  MZ     Mozambique
  NC     New Caledonia \[France\]
  NE     Niue \[New Zealand\]
  NF     Norfolk Island \[Australia\]
  NG     Niger
  NH     Vanuatu
  NI     Nigeria
  NL     Netherlands
  NO     Norway
  NP     Nepal
  NS     Suriname
  NT     Netherlands Antilles \[Netherlands\]
  NU     Nicaragua
  NZ     New Zealand
  PA     Paraguay
  PC     Pitcairn Islands \[United Kingdom\]
  PE     Peru
  PK     Pakistan
  PL     Poland
  PM     Panama
  PO     Portugal
  PP     Papua New Guinea
  PS     Palau
  PU     Guinea-Bissau
  QA     Qatar
  RE     Reunion \[France\]
  RI     Serbia
  RM     Marshall Islands
  RO     Romania
  RP     Philippines
  RQ     Puerto Rico \[United States\]
  RS     Russia
  RW     Rwanda
  SA     Saudi Arabia
  SB     Saint Pierre and Miquelon \[France\]
  SE     Seychelles
  SF     South Africa
  SG     Senegal
  SH     Saint Helena \[United Kingdom\]
  SI     Slovenia
  SL     Sierra Leone
  SN     Singapore
  SP     Spain
  ST     Saint Lucia
  SU     Sudan
  SV     Svalbard \[Norway\]
  SW     Sweden
  SX     South Georgia and the South Sandwich Islands \[United Kingdom\]
  SY     Syria
  SZ     Switzerland
  TD     Trinidad and Tobago
  TE     Tromelin Island \[France\]
  TH     Thailand
  TI     Tajikistan
  TL     Tokelau \[New Zealand\]
  TN     Tonga
  TO     Togo
  TS     Tunisia
  TU     Turkey
  TV     Tuvalu
  TX     Turkmenistan
  TZ     Tanzania
  UG     Uganda
  UK     United Kingdom
  UP     Ukraine
  US     United States
  UV     Burkina Faso
  UY     Uruguay
  UZ     Uzbekistan
  VE     Venezuela
  VM     Vietnam
  VQ     Virgin Islands \[United States\]
  WA     Namibia
  WF     Wallis and Futuna \[France\]
  WI     Western Sahara
  WQ     Wake Island \[United States\]
  WZ     Swaziland
  ZA     Zambia
  ZI     Zimbabwe

### Look Up Table of State Codes

The table of the state codes below is a derived from the
ghcnd-states.txt file which is available at the link below

<http://noaa-ghcn-pds.s3.amazonaws.com/ghcnd-states.txt>

The state codes are used in the station identification number, the table
below CODE = is the POSTAL code of the U.S. state/territory or Canadian
province where the station is located.

  Code   State
  ------ -----------------------------
  AB     ALBERTA
  AB     ALBERTA
  AK     ALASKA
  AL     ALABAMA
  AR     ARKANSAS
  AS     AMERICAN SAMOA
  AZ     ARIZONA
  BC     BRITISH COLUMBIA
  CA     CALIFORNIA
  CO     COLORADO
  CT     CONNECTICUT
  DC     DISTRICT OF COLUMBIA
  DE     DELAWARE
  FL     FLORIDA
  FM     MICRONESIA
  GA     GEORGIA
  GU     GUAM
  HI     HAWAII
  IA     IOWA
  ID     IDAHO
  IL     ILLINOIS
  IN     INDIANA
  KS     KANSAS
  KY     KENTUCKY
  LA     LOUISIANA
  MA     MASSACHUSETTS
  MB     MANITOBA
  MD     MARYLAND
  ME     MAINE
  MH     MARSHALL ISLANDS
  MI     MICHIGAN
  MN     MINNESOTA
  MO     MISSOURI
  MP     NORTHERN MARIANA ISLANDS
  MS     MISSISSIPPI
  MT     MONTANA
  NB     NEW BRUNSWICK
  NC     NORTH CAROLINA
  ND     NORTH DAKOTA
  NE     NEBRASKA
  NH     NEW HAMPSHIRE
  NJ     NEW JERSEY
  NL     NEWFOUNDLAND AND LABRADOR
  NM     NEW MEXICO
  NS     NOVA SCOTIA
  NT     NORTHWEST TERRITORIES
  NU     NUNAVUT
  NV     NEVADA
  NY     NEW YORK
  OH     OHIO
  OK     OKLAHOMA
  ON     ONTARIO
  OR     OREGON
  PA     PENNSYLVANIA
  PE     PRINCE EDWARD ISLAND
  PI     PACIFIC ISLANDS
  PR     PUERTO RICO
  PW     PALAU
  QC     QUEBEC
  RI     RHODE ISLAND
  SC     SOUTH CAROLINA
  SD     SOUTH DAKOTA
  SK     SASKATCHEWAN
  TN     TENNESSEE
  TX     TEXAS
  UM     U.S. MINOR OUTLYING ISLANDS
  UT     UTAH
  VA     VIRGINIA
  VI     VIRGIN ISLANDS
  VT     VERMONT
  WA     WASHINGTON
  WI     WISCONSIN
  WV     WEST VIRGINIA
  WY     WYOMING
  YT     YUKON TERRITORY

## FORMAT OF "ghcnd-inventory.txt" {#format-of-ghcnd-inventory-txt}

This is a file listing the periods of record for each station and
element. The file is located here:

<http://noaa-ghcn-pds.s3.amazonaws.com/ghcnd-inventory.txt>

The file structure is described in the table below.

  Variable    Columns   Type
  ----------- --------- -----------
  ID          1-11      CHARACTER
  LATITUDE    13-20     REAL
  LONGITUDE   22-30     REAL
  ELEMENT     32-35     CHARACTER
  FIRSTYEAR   37-40     INTEGER
  LASTYEAR    42-45     INTEGER

-   ID = the station identification code. Please see
    "ghcnd-stations.txt" for a complete list of stations and their
    metadata.
-   LATITUDE = the latitude of the station (in decimal degrees).
-   LONGITUDE = the longitude of the station (in decimal degrees).
-   ELEMENT = the element type. See section III for a definition of
    elements.
-   FIRSTYEAR = the first year of unflagged data for the given element.
-   LASTYEAR = the last year of unflagged data for the given element.

## Contact

For questions regarding data content or quality, go
[here](https://www.ncdc.noaa.gov/ghcn-daily-description). For any
questions regarding data delivery not associated with this platform or
any general questions regarding the NOAA Big Data Project, email
noaa.bdp\@noaa.gov.

## HOW TO CITE:

Note that the GHCN-Daily dataset itself has a DOI (Digital Object
Identifier) so it may be relevant to cite both the methods/overview
journal article as well as the specific version of the dataset used.

The journal article describing GHCN-Daily is:

Menne, M.J., I. Durre, R.S. Vose, B.E. Gleason, and T.G. Houston, 2012:
An overview of the Global Historical Climatology Network-Daily Database.
Journal of Atmospheric and Oceanic Technology, 29, 897-910,
<doi:10.1175/JTECH-D-11-00103.1>

To acknowledge the specific version of the dataset used, please cite:

Menne, M.J., I. Durre, B. Korzeniewski, S. McNeal, K. Thomas, X. Yin, S.
Anthony, R. Ray, R.S. Vose, B.E.Gleason, and T.G. Houston, 2012: Global
Historical Climatology Network - Daily (GHCN-Daily), Version 3.
\[indicate subset used following decimal, e.g. Version 3.25\]. NOAA
National Centers for Environmental Information.
<http://doi.org/10.7289/V5D21VHZ> \[access date\]

## REFERENCES

Durre, I., M. J. Menne, B. E. Gleason, T. G. Houston, and R. S. Vose
(2010), Comprehensive automated quality assurance of daily surface
observations, J. Appl. Meteorol. Climatol., 49, 1615--1633,
[doi:10.1175/2010JAMC2375.1](https://doi:10.1175/2010JAMC2375.1)

Klein Tank, A.M.G. and Coauthors, 2002. Daily dataset of 20th-century
surface air temperature and precipitation series for the European
Climate Assessment. Int. J. of Climatol., 22, 1441-1453. Data and
metadata available at [http://eca.knmi.nl](http://eca.knmi.nl/)

Menne, M.J., I. Durre, R.S. Vose, B.E. Gleason, and T.G. Houston, 2012:
An overview of the Global Historical Climatology Network-Daily Database.
Journal of Atmospheric and Oceanic Technology, 29, 897-910,
[doi.10.1175/JTECH-D-11-00103.1](https://docs.opendata.aws/noaa-ghcn-pds/doi.10.1175/JTECH-D-11-00103.1)
