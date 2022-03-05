# utl-how-to-use-different-weights-on-the-same-variable-in-a-small-fifty-nine-gb-dataset
How to use different weights on the same variable in a small fifty nine gb dataset
    %let pgm=utl=how-to-use-different-weights-on-the-same-variable-in-a-small-fifty-nine-gb-dataset;

    How to use different weights on the same variable in a small fifty nine gb dataset

    githup
    https://tinyurl.com/yw7fnwfx
    https://github.com/rogerjdeangelis/utl-how-to-use-different-weights-on-the-same-variable-in-a-small-fifty-nine-gb-dataset

    This is a small data problem.

    My definition of big data or big execution?
    A single table over 1TB or simulation requiring 100 cores for over a week.

    Run on a 2013 slow Dell T7610 Workstation ($600 on Ebay);

    I was unable to significantly improve performance by parallelizing the solution, even with mutitheaded SPDE processing
    or parrtioning the input.

    Extreeme I/O bound.

    Using a single core it took less that 5 minutes to weight the 20 variables.

    github
    https://tinyurl.com/4a62jf26
    https://stackoverflow.com/questions/71266383/sas-9-4-how-to-use-different-weights-on-the-same-variable-without-datastep-or-p


    /**************************************************************************************************************************/
    /*   _                   _      ____  ___        _                                                                        */
    /*  (_)_ __  _ __  _   _| |_   | ___|/ _ \  __ _| |__                                                                     */
    /*  | | `_ \| `_ \| | | | __|  |___ \ (_) |/ _` | `_ \                                                                    */
    /*  | | | | | |_) | |_| | |_    ___) \__, | (_| | |_) |                                                                   */
    /*  |_|_| |_| .__/ \__,_|\__|  |____/  /_/ \__, |_.__/                                                                    */
    /*          |_|                            |___/                                                                          */
    /*                                                                                                                        */
    /*  SD1.HAVE total obs=987,500,000 05MAR2022:09:09:28                                                                     */
    /*                                                                                                                        */
    /*        Obs    VAR      VAL         W1         W2         W3         W4         W5         W6                           */
    /*                                                                                                                        */
    /*          1     A     1.04876    0.01790    0.07664    0.01959    0.05152    0.01765    0.00726                         */
    /*          2     A     1.02152    0.08917    0.02862    0.00782    0.09124    0.01772    0.18115                         */
    /*          3     A     1.19349    0.14747    0.08059    0.07463    0.06557    0.10206    0.05678                         */
    /*        ...                                                                                                             */
    /*  987499998     T     1.07012    0.05116    0.08728    0.02480    0.14279    0.00055    0.10200                         */
    /*  987499999     T     1.18112    0.04690    0.15963    0.02975    0.02352    0.10683    0.12817                         */
    /*  987500000     T     1.15460    0.10927    0.19552    0.10695    0.08421    0.09864    0.18869                         */
    /*                                                                                                                        */
    /* RULES                                                                                                                  */
    /*                                                                                                                        */
    /*   For each var compute the weighted mean, ie mean(val*w1) as varWgt1 .. mean(val*w6) as varWgt6                        */
    /*                                                                                                                        */
    /*    Var                            VAL       W1                                                                         */
    /*                                                                                                                        */
    /*     A        varWgt1 = mean ( 1.04876 * 0.01790                                                                        */
    /*                               1.02152 * 0.08917                                                                        */
    /*                               1.19349 * 0.14747 ) = 0.0952885709                                                       */
    /*                                                                                                                        */
    /*     A        varWgt2 = mean ( 1.04876 * 0.07664                                                                        */
    /*                               1.02152 * 0.02862                                                                        */
    /*                               1.19349 * 0.08059 ) = 0.068600                                                           */
    /*     ...                                                                                                                */
    /*                                                                                                                        */
    /*     T        varWgt1 = mean ( 1.07012 * 0.05116                                                                        */
    /*                               1.18112 * 0.04690                                                                        */
    /*                               1.15460 * 0.10927 ) = 0.078768                                                           */
    /*                                                                                                                        */
    /*               _               _                                                                                        */
    /*    ___  _   _| |_ _ __  _   _| |_                                                                                      */
    /*   / _ \| | | | __| `_ \| | | | __|                                                                                     */
    /*  | (_) | |_| | |_| |_) | |_| | |_                                                                                      */
    /*   \___/ \__,_|\__| .__/ \__,_|\__|                                                                                     */
    /*                  |_|                                                                                                   */
    /*                                                                                                                        */
    /*    Up to 40 obs WORK.WEIGHTED_VARS total obs=1 05MAR2022:09:55:53                                                      */
    /*                                                                                                                        */
    /*    Obs    VAR     VARWGT1     VARWGT2     VARWGT3     VARWGT4     VARWGT5     VARWGT6                                  */
    /*                                                                                                                        */
    /*     1      A     0.095285    0.068600    0.039202    0.075164    0.052803    0.086811                                  */
    /*    ...                                                                                                                 */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    libame sd1 "f:/sd1";

    data  sd1.have (sortedby=var);
      do var="A","B","C","D","E","F","G","H","I","J","K","L"
             ,"M","N","O","P","Q","R","S","T";
        do rep=1 to 1250*500*79;
           val=1+uniform(1234)* (rank(var)-61)/20;
           W1 = uniform(1234) * (rank(var)-61)/20;
           W2 = uniform(1234) * (rank(var)-61)/20;
           W3 = uniform(1234) * (rank(var)-61)/20;
           W4 = uniform(1234) * (rank(var)-61)/20;
           W5 = uniform(1234) * (rank(var)-61)/20;
           W6 = uniform(1234) * (rank(var)-61)/20;
           if mod(rep,10000000)=0 then put var= rep comma20.;
           output;
        end;
      end;
      stop;
      drop rep;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /* NOTE: The data set SD1.HAVE has 987500000 observations and 8 variables.                                                */
    /* NOTE: DATA statement used (Total process time):                                                                        */
    /*       real time           3:21.20                                                                                      */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  Up to 40 obs from SD1.HAVE total obs=987,500,000 05MAR2022:10:20:09                                                   */
    /*                                                                                                                        */
    /*        Obs    VAR      VAL         W1         W2         W3         W4         W5         W6                           */
    /*                                                                                                                        */
    /*          1     A     1.04876    0.01790    0.07664    0.01959    0.05152    0.01765    0.00726                         */
    /*          2     A     1.02152    0.08917    0.02862    0.00782    0.09124    0.01772    0.18115                         */
    /*          3     A     1.19349    0.14747    0.08059    0.07463    0.06557    0.10206    0.05678                         */
    /*          4     A     1.07012    0.05116    0.08728    0.02480    0.14279    0.00055    0.10200                         */
    /*          5     A     1.18112    0.04690    0.15963    0.02975    0.02352    0.10683    0.12817                         */
    /*          6     A     1.15460    0.10927    0.19552    0.10695    0.08421    0.09864    0.18869                         */
    /*          7     A     1.14126    0.06959    0.03620    0.17576    0.02787    0.06318    0.19325                         */
    /*          8     A     1.11489    0.06291    0.13088    0.10925    0.13864    0.12053    0.04972                         */
    /*          9     A     1.06981    0.05537    0.14565    0.18379    0.12392    0.14256    0.00540                         */
    /*        ...                                                                                                             */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*         _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| `_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    */

    proc sql;
     *reset inobs=10000000;
      create
        table Weighted_Vars as
      select
        var
       ,mean(val * w1 ) as varWgt1
       ,mean(val * w2 ) as varWgt2
       ,mean(val * w3 ) as varWgt3
       ,mean(val * w4 ) as varWgt4
       ,mean(val * w5 ) as varWgt5
       ,mean(val * w6 ) as varWgt6
     from
        sd1.have
     group
        by var
    ;quit;

    /**************************************************************************************************************************/
    /*  _                                                                                                                     */
    /* | | ___   __ _                                                                                                         */
    /* | |/ _ \ / _` |                                                                                                        */
    /* | | (_) | (_| |                                                                                                        */
    /* |_|\___/ \__, |                                                                                                        */
    /*          |___/                                                                                                         */
    /*                                                                                                                        */
    /*  NOTE: Table WORK.WEIGHTED_VARS created, with 20 rows and 7 columns.                                                   */
    /*                                                                                                                        */
    /*  12116!  quit;                                                                                                         */
    /*  NOTE: PROCEDURE SQL used (Total process time):                                                                        */
    /*       real time            4:52.58                                                                                     */
    /*        user cpu time       4:32.61                                                                                     */
    /*        system cpu time     46.04 seconds                                                                               */
    /*        memory              6613.46k                                                                                    */
    /*        OS Memory           104437004.00k                                                                               */
    /*        Timestamp           03/05/2022 10:09:05 AM                                                                      */
    /*        Step Count                        1461  Switch Count  11                                                        */
    /*               _               _                                                                                        */
    /*    ___  _   _| |_ _ __  _   _| |_                                                                                      */
    /*   / _ \| | | | __| `_ \| | | | __|                                                                                     */
    /*  | (_) | |_| | |_| |_) | |_| | |_                                                                                      */
    /*   \___/ \__,_|\__| .__/ \__,_|\__|                                                                                     */
    /*                  |_|                                                                                                   */
    /*                                                                                                                        */
    /*  Up to 40 obs from WEIGHTED_VARS total obs=20 05MAR2022:10:10:51                                                       */
    /*                                                                                                                        */
    /*  Obs    VAR    VARWGT1    VARWGT2    VARWGT3    VARWGT4    VARWGT5    VARWGT6                                          */
    /*                                                                                                                        */
    /*    1     A     0.10999    0.11001    0.10999    0.11001    0.10999    0.11001                                          */
    /*    2     B     0.14062    0.14061    0.14064    0.14063    0.14061    0.14060                                          */
    /*    3     C     0.17249    0.17251    0.17251    0.17249    0.17251    0.17250                                          */
    /*    4     D     0.20563    0.20561    0.20565    0.20561    0.20563    0.20561                                          */
    /*    5     E     0.24001    0.24003    0.23999    0.23998    0.24001    0.24003                                          */
    /*    6     F     0.27565    0.27560    0.27560    0.27562    0.27562    0.27564                                          */
    /*    7     G     0.31251    0.31253    0.31247    0.31252    0.31249    0.31253                                          */
    /*    8     H     0.35059    0.35058    0.35064    0.35063    0.35059    0.35059                                          */
    /*    9     I     0.38997    0.39002    0.39002    0.39000    0.39001    0.38999                                          */
    /*   10     J     0.43064    0.43061    0.43067    0.43061    0.43062    0.43061                                          */
    /*   11     K     0.47252    0.47251    0.47252    0.47244    0.47251    0.47254                                          */
    /*   12     L     0.51564    0.51558    0.51556    0.51562    0.51559    0.51567                                          */
    /*   13     M     0.56003    0.56003    0.55995    0.56002    0.56002    0.55999                                          */
    /*   14     N     0.60554    0.60557    0.60561    0.60565    0.60558    0.60566                                          */
    /*   15     O     0.65252    0.65252    0.65256    0.65249    0.65246    0.65244                                          */
    /*   16     P     0.70064    0.70060    0.70069    0.70061    0.70062    0.70064                                          */
    /*   17     Q     0.75005    0.74998    0.75003    0.74992    0.75003    0.74999                                          */
    /*   18     R     0.80061    0.80064    0.80054    0.80055    0.80061    0.80072                                          */
    /*   19     S     0.85257    0.85249    0.85246    0.85257    0.85255    0.85253                                          */
    /*   20     T     0.90553    0.90564    0.90558    0.90557    0.90551    0.90567                                          */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
