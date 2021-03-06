# utl_creating_two-way_counts_and_percentage_datasets
Creating Two-way counts and percentage datasets. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Creating Two-way counts and percentage datasets

    github
    https://github.com/rogerjdeangelis/utl_creating_two-way_counts_and_percentage_datasets
    and
    https://github.com/rogerjdeangelis/utl_compare_corresp_vs_report_output_datasets

    I find it much more useful to have a dataset with the structures you want then a
    static proc tabulate. ODS tabulate output datasets are pretty useless.
    Proc corresp and proc report output datasets are more usefull.
    These procedures can sort, transpose and summarize in one proc.

    Here are solutions for marginals and percents

    SAS Forum
    https://communities.sas.com/t5/SAS-Procedures/two-way-percentages-in-proc-tabulate/m-p/459548

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    WORK.CLASS total obs=331

    Obs    year    sex    cnt

      1    2014     F      75
      2    2014     M     100
      3    2014     M      57
      4    2014     F      14
      5    2014     F      32
      6    2014     F       6
      7    2014     M     100
      8    2014     M      27
      9    2014     F      53
    ...


    *                    _
    __      ____ _ _ __ | |_
    \ \ /\ / / _` | '_ \| __|
     \ V  V / (_| | | | | |_
      \_/\_/ \__,_|_| |_|\__|

    ;

    COUNTS
    ======

    XPOCNT total obs=3

    Obs    Sex    2014    2015    2016    2017     Sum

     1     F      1588    2264    2136    1950     7938
     2     M      2360    1474    2331    1737     7902

     3     Sum    3948    3738    4467    3687    15840


    CELL PERCENT
    ============

    XPOPCT total obs=3

    Obs    Sex      2014       2015       2016       2017       Sum

     1     F      10.0253    14.2929    13.4848    12.3106     50.114
     2     M      14.8990     9.3056    14.7159    10.9659     49.886

     3     Sum    24.9242    23.5985    28.2008    23.2765    100.000


    COLUMN PERCENT
    ============

    XPOCOLPCT total obs=2

    Obs    Sex      2014       2015       2016       2017

     1      F     40.2229    60.5671    47.8173    52.8885
     2      M     59.7771    39.4329    52.1827    47.1115


    COLUMN FRACTIONS
    ================

    XPOCOLFRAC total obs=2

    bs    Sex      2014       2015       2016       2017

    1      F     0.40223    0.60567    0.47817    0.52889
    2      M     0.59777    0.39433    0.52183    0.47111

    ROW PERCENT
    ===========

    XPOROWPCT total obs=2

    Obs    Sex      2014       2015       2016       2017

     1      F     20.0050    28.5210    26.9085    24.5654
     2      M     29.8659    18.6535    29.4989    21.9818


    ROW FRACTIONS
    =============

    XPOROWFRAC  total obs=2

    Obs    Sex      2014       2015       2016       2017

     1      F     0.20005    0.28521    0.26909    0.24565
     2      M     0.29866    0.18654    0.29499    0.21982




    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;


    options validvarname=any;
    ods exclude all;
    ods output ObservedPct=xpopct(rename=(label=Sex ));
    ods output observed   =xpocnt(rename=label=Sex);
    ods output colprofiles=xpocolfrac(rename=(label=Sex));
    ods output rowprofiles=xporowfrac(rename=(label=Sex));
    ods output colprofilespct=xpocolpct(rename=(label=Sex));
    ods output rowprofilespct=xporowpct(rename=(label=Sex));
    proc corresp data=class all dimens=1 print=both;
     tables   sex, year;
     weight cnt;
    run;quit;
    ods select all;

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;

    data class(drop=rec);
      call streaminit(1234);
      do year=2014 to 2017;
        do rec=1 to 100;
        if rand("uniform")<.5 then sex='M';
        else sex='F';
        cnt=int(100*rand("uniform"))+1;
        if rand("uniform")<.8 then output;
        end;
      end;
      stop;
    run;quit;



