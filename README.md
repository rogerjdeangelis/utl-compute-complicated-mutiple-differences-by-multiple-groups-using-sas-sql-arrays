# utl-compute-complicated-mutiple-differences-by-multiple-groups-using-sas-sql-arrays
Compute complicated mutiple differences by multiple groups using sas sql arrays
    %let pgm=utl-compute-complicated-mutiple-differences-by-multiple-groups-using-sas-sql-arrays;

    %stop_submission;

    Compute complicated mutiple differences by multiple groups using sas sql arrays

    For full solution see
    github
    https://tinyurl.com/59f3kjs8
    https://github.com/rogerjdeangelis/utl-compute-complicated-mutiple-differences-by-multiple-groups-using-sas-sql-arrays

    Calculate the difference of the means of mutliple variables (e.g., size, weight)
    by categories (e.g., color, taste) and groups (e.g., fruit) in R.

    INPUT
    =====

    FRUIT    COLOR     TASTE    SIZE    WEIGH

    apple    green     sweet      5       18
    apple    green     sour       7       12
    apple    yellow    sweet      6       18
    apple    yellow    sour       9       19
    pear     green     sweet      3       17
    pear     green     sour       2       18
    pear     yellow    sweet      8       17
    pear     yellow    sour       2       13


    PROCESS
    =======

    Generated code using Teds sql arrays.
    We get good documentation and very meaningful column names.
     ros.

    SELF EXPANATORY SQL (HARDCODING IS CLEANER AND FASTER?).

    proc sql;
      create
         table want as
      select
         fruit
        ,avg(case when color = "green"  then size   end) as avg_size_green
        ,avg(case when color = "green"  then weight end) as avg_weight_green
        ,avg(case when taste = "sour"   then size   end) as avg_size_sour
        ,avg(case when taste = "sour"   then weight end) as avg_weight_sour
        ,avg(case when color =" yellow" then size   end) as avg_size_yellow
        ,avg(case when color = "yellow" then weight end) as avg_weight_yellow
        ,avg(case when taste = "sweet"  then size   end) as avg_size_sweet
        ,avg(case when taste = "sweet"  then weight end) as avg_weight_sweet

        ,calculated avg_size_yellow   - calculated avg_size_green   as size_difavg_bycolor
        ,calculated avg_size_sweet    - calculated avg_size_sour    as size_difavg_bytaste
        ,calculated avg_weight_yellow - calculated avg_weight_green as weight_difavg_bycolor
        ,calculated avg_weight_sweet  - calculated avg_weight_sour  as weight_difavg_bytaste
      from
        sd1.have
      group
        by fruit ;

    OUTPUT
    ======

       Variables in Creation Order
     Variable              Comment

     FRUIT                 - apple pear
     AVG_SIZE_GREEN        -All the means by color and taste
     AVG_WEIGHT_GREEN
     AVG_SIZE_SOUR
     AVG_WEIGHT_SOUR
     AVG_SIZE_YELLOW
     AVG_WEIGHT_YELLOW
     AVG_SIZE_SWEET
     AVG_WEIGHT_SWEET

     SIZE_DIFAVG_BYCOLOR   -size   mean yellow - mean green
     WEIGHT_DIFAVG_BYCOLOR -weight mean yellow - mean green

     SIZE_DIFAVG_BYTASTE   -size    mean sweet -  mean sour
     WEIGHT_DIFAVG_BYTASTE -weight  mean sweet -  mean sour

    stackoverflow r
    https://tinyurl.com/4b558hwb
    https://stackoverflow.com/questions/79717096/calculate-difference-of-means-of-multiple-variables-by-cateogries-and-groups

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input fruit$  color$ taste$
         size weight;
    cards4;
    apple  green sweet 5 18
    apple  green  sour 7 12
    apple  green sweet 6 17
    apple  green  sour 9 11
    apple yellow sweet 6 18
    apple yellow  sour 7 12
    apple yellow sweet 4 11
    apple yellow  sour 9 19
     pear  green sweet 3 17
     pear  green  sour 2 18
     pear  green sweet 9 15
     pear  green  sour 4 11
     pear yellow sweet 8 17
     pear yellow  sour 5 18
     pear yellow sweet 7 19
     pear yellow  sour 2 13
    ;;;;
    run;quit;

    %array(_clr,values=%utl_concat(sd1.have,var=color,unique=Y));
    %array(_tst,values=%utl_concat(sd1.have,var=taste,unique=Y));

    %put &=_clr1; * greem  ;
    %put &=_clr2; * yellow ;
    %put &=_clrn; * 2      ;

    %put &=_tst1; * sour   ;
    %put &=_tst2; * sweet  ;
    %put &=_tstn; * 2      ;

    /**************************************************************************************************************************/
    /* FRUIT    COLOR     TASTE    SIZE    WEIGHT                                                                             */
    /*                                                                                                                        */
    /* apple    green     sweet      5       18                                                                               */
    /* apple    green     sour       7       12                                                                               */
    /* apple    green     sweet      6       17                                                                               */
    /* apple    green     sour       9       11                                                                               */
    /* apple    yellow    sweet      6       18                                                                               */
    /* apple    yellow    sour       7       12                                                                               */
    /* apple    yellow    sweet      4       11                                                                               */
    /* apple    yellow    sour       9       19                                                                               */
    /* pear     green     sweet      3       17                                                                               */
    /* pear     green     sour       2       18                                                                               */
    /* pear     green     sweet      9       15                                                                               */
    /* pear     green     sour       4       11                                                                               */
    /* pear     yellow    sweet      8       17                                                                               */
    /* pear     yellow    sour       5       18                                                                               */
    /* pear     yellow    sweet      7       19                                                                               */
    /* pear     yellow    sour       2       13                                                                               */
    /*                                                                                                                        */
    /* %put &=_clr1; * greem  ;                                                                                               */
    /* %put &=_clr2; * yellow ;                                                                                               */
    /* %put &=_clrn; * 2      ;                                                                                               */
    /*                                                                                                                        */
    /* %put &=_tst1; * sour   ;                                                                                               */
    /* %put &=_tst2; * sweet  ;                                                                                               */
    /* %put &=_tstn; * 2      ;                                                                                               */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    proc sql;
    create
      table want as
    select
      fruit
      %do_over(_clr _tst,phrase=%str(
        ,avg(case when color = "?_clr"  then size   end) as avg_size_?_clr
        ,avg(case when color = "?_clr"  then weight end) as avg_weight_?_clr
        ,avg(case when taste = "?_tst"  then size   end) as avg_size_?_tst
        ,avg(case when taste = "?_tst"  then weight end) as avg_weight_?_tst
      ))
      ,calculated avg_size_&_clr2   - calculated avg_size_&_clr1   as size_difavg_bycolor
      ,calculated avg_size_&_tst2   - calculated avg_size_&_tst1   as size_difavg_bytaste
      ,calculated avg_weight_&_clr2 - calculated avg_weight_&_clr1 as weight_difavg_bycolor
      ,calculated avg_weight_&_tst2 - calculated avg_weight_&_tst1 as weight_difavg_bytaste
    from
      sd1.have
    group
      by fruit
    ;quit;

    proc print data=want split='_';
    run;quit;

    %arraydelete(_clr);
    %arraydelete(_tst);

    /**************************************************************************************************************************/
    /*                                                              AVG YELLOW-GREEN             AVG SWEET-SOUR               */
    /*                                                              ----------------             ---------------              */
    /*         AVG    AVG   AVG   AVG    AVG    AVG   AVG    AVG       SIZE   WEIGHT               SIZE   WEIGHT              */
    /*         SIZE WEIGHT SIZE WEIGHT  SIZE  WEIGHT  SIZE WEIGHT     DIFAVG  DIFAVG              DIFAVG  DIFAVG              */
    /*  FRUIT GREEN  GREEN SOUR  SOUR  YELLOW YELLOW SWEET  SWEET    BYCOLOR BYCOLOR             BYTASTE BYTASTE              */
    /*                                                                                                                        */
    /*  apple  6.75  14.50 8.00  13.5    6.5   15.00  5.25   16       -0.25    0.5(15-14.5)       -2.75    2.5                */
    /*  pear   4.50  15.25 3.25  15.0    5.5   16.75  6.75   17        1.00    1.51(6.75-15.25)    3.50    2.0                */
    /*                                                                                                                        */
    /* GENERATED CODE (USE DEBUG MACRO AND THEN DO MINOR EDITS)                                                               */
    /*                                                                                                                        */
    /* proc sql;                                                                                                              */
    /*   create                                                                                                               */
    /*      table want as                                                                                                     */
    /*   select                                                                                                               */
    /*      fruit                                                                                                             */
    /*     ,avg(case when color = "green"  then size   end) as avg_size_green                                                 */
    /*     ,avg(case when color = "green"  then weight end) as avg_weight_green                                               */
    /*     ,avg(case when taste = "sour"   then size   end) as avg_size_sour                                                  */
    /*     ,avg(case when taste = "sour"   then weight end) as avg_weight_sour                                                */
    /*     ,avg(case when color =" yellow" then size   end) as avg_size_yellow                                                */
    /*     ,avg(case when color = "yellow" then weight end) as avg_weight_yellow                                              */
    /*     ,avg(case when taste = "sweet"  then size   end) as avg_size_sweet                                                 */
    /*     ,avg(case when taste = "sweet"  then weight end) as avg_weight_sweet                                               */
    /*                                                                                                                        */
    /*     ,calculated avg_size_yellow   - calculated avg_size_green   as size_difavg_bycolor                                 */
    /*     ,calculated avg_size_sweet    - calculated avg_size_sour    as size_difavg_bytaste                                 */
    /*     ,calculated avg_weight_yellow - calculated avg_weight_green as weight_difavg_bycolor                               */
    /*     ,calculated avg_weight_sweet  - calculated avg_weight_sour  as weight_difavg_bytaste                               */
    /*   from                                                                                                                 */
    /*     sd1.have                                                                                                           */
    /*   group                                                                                                                */
    /*     by fruit ;                                                                                                         */
    /* ;quit;                                                                                                                 */
    /**************************************************************************************************************************/

    /*
                    _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
