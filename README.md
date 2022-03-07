# utl-how-to-use-different-weights-on-the-same-variable-in-a-small-fifty-nine-gb-dataset
   %let pgm=utl-how-to-use-different-weights-on-the-same-variable-in-a-small-fifty-nine-gb-dataset;

    How to use different weights on the same variable in a small fifty nine gb dataset

    Using SPDE and Mark's code I was able to reduce the time to 68 seconds (20 simultaneous tasks)

      Solutions

           1. SQL single core 4:52 seconds
           2. Datastep 3:24 seconds
              Keintz, Mark
              mkeintz@outlook.com
              mkeintz@wharton.upenn.edu
           3. Mark's datastep with SPDE 1:08 (68 seconds)
           4, HASH (single threaded)  Probably the fastest for unsorted data, I have enough ram 128gb.
              (I think there is an issue with the base SAS hash because it is single threaded)
              Took almost 7 minutes on the sorted data.
              I woud use the multithreaded DS2 SAS hash if SAS had a tool to convert base
              SAS language to a DS2 language. Don't have time for other sas languages like DS2 and FCMP.
           5. Parallel Hash 20 tasks
              Not much better when when running 20 parallel tasks

    FYI if a substute sum for mean in SQL and use SPDE it runs in 64 seconds.
    However, if I use the mean function it takes over 7 minutes.
    Might be able to use SPDE if I use count and sum and have a second sql to get the mean.

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
    /* RULES                                                                                                                   */
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
               _
     ___  __ _| |   ___  _ __   ___    ___ ___  _ __ ___
    / __|/ _` | |  / _ \| `_ \ / _ \  / __/ _ \| `__/ _ \
    \__ \ (_| | | | (_) | | | |  __/ | (_| (_) | | |  __/
    |___/\__, |_|  \___/|_| |_|\___|  \___\___/|_|  \___|
            |_|
    */
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

    /*___          _       _            _
    |___ \      __| | __ _| |_ __ _ ___| |_ ___ _ __
      __) |    / _` |/ _` | __/ _` / __| __/ _ \ `_ \
     / __/ _  | (_| | (_| | || (_| \__ \ ||  __/ |_) |
    |_____(_)  \__,_|\__,_|\__\__,_|___/\__\___| .__/
                                               |_|
    */

    data want (keep=var varwgt:);
      set sd1.have;
      by var;
      array vrw varwgt1-varwgt6;
      array wgt w1-w6;
      do over vrw;
        vrw+val*wgt;
      end;
      if last.var then do;
        denom=coalesce(dif(_n_),_n_);
        do over vrw;
          vrw=vrw/denom;
        end;
        output;
        call missing(of varwgt1-varwgt6);
      end;
      format varwgt:  8.5;
    run;

    /*
    NOTE: There were 987500000 observations read from the data set SD1.HAVE.
    NOTE: The data set WORK.WANT has 20 observations and 7 variables.
    NOTE: DATA statement used (Total process time):
          real time           3:24.51
    */
    /*   _       _                                           _
      __| | __ _| |_ __ _ ___ _ __ ___ _ __    ___ _ __   __| | ___
     / _` |/ _` | __/ _` / __| `__/ _ \ `_ \  / __| `_ \ / _` |/ _ \
    | (_| | (_| | || (_| \__ \ | |  __/ |_) | \__ \ |_) | (_| |  __/
     \__,_|\__,_|\__\__,_|___/_|  \___| .__/  |___/ .__/ \__,_|\___|
                                      |_|         |_|
    */

    * create spde partitioned data;

    libname spde spde
     ('c:\spde_c','d:\spde_d','f:\spde_f','g:\spde_g','m:\spde_m')
        metapath =('d:\spde_d\metadata')
        indexpath=(
              'c:\spde_c'
              ,'d:\spde_d'
              ,'f:\spde_f'
              ,'g:\spde_g'
              ,'m:\spde_m')

        datapath =(
              'c:\spde_c'
              ,'d:\spde_d'
              ,'f:\spde_f'
              ,'g:\spde_g'
              ,'m:\spde_m')
        partsize=500m ;
    ;


    data  spde.have (sortedby=var);
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

    * store macro in autocall library;

    filename ft15f001 "c:/oto/datx.sas";
    parmcards4;
    %macro datx(var);

    /* %let var=A; */

    libname mwk "m:/wrk";

    libname spde spde
     ('c:\spde_c','d:\spde_d','f:\spde_f','g:\spde_g','m:\spde_m')
        metapath =('d:\spde_d\metadata')
        indexpath=(
              'c:\spde_c'
              ,'d:\spde_d'
              ,'f:\spde_f'
              ,'g:\spde_g'
              ,'m:\spde_m')

        datapath =(
              'c:\spde_c'
              ,'d:\spde_d'
              ,'f:\spde_f'
              ,'g:\spde_g'
              ,'m:\spde_m')
        partsize=500m access=readonly
    ;
       data mwk.&var (keep=var varwgt:);

         set spde.have (where=(var="&var")) end=dne;
         array vrw varwgt1-varwgt6;
         array wgt w1-w6;
         do over vrw;
              vrw+val*wgt;
          end;
         if dne then do;
           do over vrw;
             vrw=vrw/_n_;
           end;
           output;
           stop;
         end;
         format varwgt:  8.5;
       run;

    %mend datx;
    ;;;;
    run;quit;

    %let _s=%sysfunc(compbl(C:\Progra~1\SASHome\SASFoundation\9.4\sas.exe -sysin c:\nul
     -sasautos c:\oto -work d:\wrk -nosplash -rsasuser));

    options noxwait noxsync;
    %let tym=%sysfunc(time());
    systask kill sys1 sys2 sys3 sys4  sys5 sys6 sys7 sys8 sys9 sys10
                 sys11 sys12 sys13 sys14  sys15 sys16 sys17 sys18 sys19 sys20 ;
    systask command "&_s -termstmt %nrstr(%datx(A);) -log d:\log\a1.log" taskname=sys1;
    systask command "&_s -termstmt %nrstr(%datx(B);) -log d:\log\a2.log" taskname=sys2;
    systask command "&_s -termstmt %nrstr(%datx(C);) -log d:\log\a3.log" taskname=sys3;
    systask command "&_s -termstmt %nrstr(%datx(D);) -log d:\log\a4.log" taskname=sys4;
    systask command "&_s -termstmt %nrstr(%datx(E);) -log d:\log\a5.log" taskname=sys5;
    systask command "&_s -termstmt %nrstr(%datx(F);) -log d:\log\a6.log" taskname=sys6;
    systask command "&_s -termstmt %nrstr(%datx(G);) -log d:\log\a7.log" taskname=sys7;
    systask command "&_s -termstmt %nrstr(%datx(H);) -log d:\log\a8.log" taskname=sys8;
    systask command "&_s -termstmt %nrstr(%datx(I);) -log d:\log\a9.log" taskname=sys9;
    systask command "&_s -termstmt %nrstr(%datx(J);) -log d:\log\a10.log" taskname=sys10;
    systask command "&_s -termstmt %nrstr(%datx(K);) -log d:\log\a11.log" taskname=sys11;
    systask command "&_s -termstmt %nrstr(%datx(L);) -log d:\log\a12.log" taskname=sys12;
    systask command "&_s -termstmt %nrstr(%datx(M);) -log d:\log\a13.log" taskname=sys13;
    systask command "&_s -termstmt %nrstr(%datx(N);) -log d:\log\a14.log" taskname=sys14;
    systask command "&_s -termstmt %nrstr(%datx(O);) -log d:\log\a15.log" taskname=sys15;
    systask command "&_s -termstmt %nrstr(%datx(P);) -log d:\log\a16.log" taskname=sys16;
    systask command "&_s -termstmt %nrstr(%datx(Q);) -log d:\log\a17.log" taskname=sys17;
    systask command "&_s -termstmt %nrstr(%datx(R);) -log d:\log\a18.log" taskname=sys18;
    systask command "&_s -termstmt %nrstr(%datx(S);) -log d:\log\a19.log" taskname=sys19;
    systask command "&_s -termstmt %nrstr(%datx(T);) -log d:\log\a20.log" taskname=sys20;
    waitfor sys1 sys2 sys3 sys4  sys5 sys6 sys7 sys8 sys9 sys10
            sys11 sys12 sys13 sys14  sys15 sys16 sys17 sys18 sys19 sys20 ;
    %let secs=%sysevalf(%sysfunc(time()) - &tym);
    %put &=secs;
    63 seconds


    data want;

      set
         mwk.a
         mwk.b
         mwk.c
         mwk.d
         mwk.e
         mwk.f
         mwk.g
         mwk.h
         mwk.i
         mwk.j
         mwk.k
         mwk.l
         mwk.m
         mwk.n
         mwk.o
         mwk.p
         mwk.q
         mwk.r
         mwk.s
         mwk.t ;
    run;quit;

    /*   _             _        _   _                        _    _               _
     ___(_)_ __   __ _| | ___  | |_| |__  _ __ ___  __ _  __| |  | |__   __ _ ___| |__
    / __| | `_ \ / _` | |/ _ \ | __| `_ \| `__/ _ \/ _` |/ _` |  | `_ \ / _` / __| `_ \
    \__ \ | | | | (_| | |  __/ | |_| | | | | |  __/ (_| | (_| |  | | | | (_| \__ \ | | |
    |___/_|_| |_|\__, |_|\___|  \__|_| |_|_|  \___|\__,_|\__,_|  |_| |_|\__,_|___/_| |_|
                  |__/

    * SORTED INPUT;

    data want (keep=var varwgt:);

      if 0 then set spde.have;
      declare hash H(ordered:"A");
      H.defineKey("var");
      array vrw varwgt1-varwgt6;
      array dn  denom1 -denom6;

      H.defineData("var");
      do over vrw;
        H.defineData(vname(vrw));
        H.defineData(vname(dn));
      end;

      H.defineDone();
      declare hiter I("H");

      array wgt w1-w6;

      do until(eof);
        set spde.have end=eof;

        if H.find() then
            call missing(of vrw[*], of dn[*]);

        do over vrw;
          vrw + val * wgt;
          dn  + (wgt > .z < val); /* ignore missing values */
        end;
        H.replace();

      end;

      rc = i.first();
      do while (rc = 0);
        do over vrw;
          vrw =vrw/dn;
        end;
        output;
        rc = i.next();
      end;

      format varwgt:  8.5;
      stop;
    run;

    /*
    NOTE: There were 987500000 observations read from the data set SPDE.HAVE.
    NOTE: The data set WORK.WANT has 20 observations and 7 variables.
    NOTE: DATA statement used (Total process time):
          real time           6:50.17
          user cpu time       6:30.67
    */

    /*               _   _       _            _      _               _
     _ __ ___  _   _| |_(_)     | |_ __ _ ___| | __ | |__   __ _ ___| |__
    | `_ ` _ \| | | | __| |_____| __/ _` / __| |/ / | `_ \ / _` / __| `_ \
    | | | | | | |_| | |_| |_____| || (_| \__ \   <  | | | | (_| \__ \ | | |
    |_| |_| |_|\__,_|\__|_|      \__\__,_|___/_|\_\ |_| |_|\__,_|___/_| |_|

    */

    * save in autcall folder;
    filename ft15f001 "c:/oto/hshx.sas";
    parmcards4;
    %macro hshx(var);

    /* %let var=A;  */

    libname spde spde
     ('c:\spde_c','d:\spde_d','f:\spde_f','g:\spde_g','m:\spde_m')
        metapath =('d:\spde_d\metadata')
        indexpath=(
              'c:\spde_c'
              ,'d:\spde_d'
              ,'f:\spde_f'
              ,'g:\spde_g'
              ,'m:\spde_m')

        datapath =(
              'c:\spde_c'
              ,'d:\spde_d'
              ,'f:\spde_f'
              ,'g:\spde_g'
              ,'m:\spde_m')
        partsize=500m access=readonly
    ;

    libname mwk "m:/wrk";

    data mwk.&var.h (keep=var varwgt:);

      if 0 then set spde.have;

      declare hash H(ordered:"A");
      H.defineData("var");
      array vrw varwgt1-varwgt6;
      array dn  denom1 -denom6;
      H.defineKey("var");
      do over vrw;
        H.defineData(vname(vrw));
        H.defineData(vname(dn));
      end;

      H.defineDone();

      array wgt w1-w6;

      do until(eof);
        set spde.have(where=(var="&var")) end=eof;
        by var;
        do over vrw;
          vrw + val * wgt;
          dn  + (wgt > .z < val); /* ignore missing values */
        end;
        H.replace();
      end;

      do over vrw;
        vrw =vrw/dn;
      end;

      output;

      format varwgt:  8.5;
      stop;
    run;
    %mend hshx;
    ;;;;
    run;quit;

    2017


    %let _s=%sysfunc(compbl(C:\Progra~1\SASHome\SASFoundation\9.4\sas.exe -sysin c:\nul
     -sasautos c:\oto -work d:\wrk -nosplash -rsasuser));

    options noxwait noxsync;
    %let tym=%sysfunc(time());
    systask kill sys1 sys2 sys3 sys4  sys5 sys6 sys7 sys8 sys9 sys10
                 sys11 sys12 sys13 sys14  sys15 sys16 sys17 sys18 sys19 sys20 ;
    systask command "&_s -termstmt %nrstr(%hshx(A);) -log d:\log\a1.log" taskname=sys1;
    systask command "&_s -termstmt %nrstr(%hshx(B);) -log d:\log\a2.log" taskname=sys2;
    systask command "&_s -termstmt %nrstr(%hshx(C);) -log d:\log\a3.log" taskname=sys3;
    systask command "&_s -termstmt %nrstr(%hshx(D);) -log d:\log\a4.log" taskname=sys4;
    systask command "&_s -termstmt %nrstr(%hshx(E);) -log d:\log\a5.log" taskname=sys5;
    systask command "&_s -termstmt %nrstr(%hshx(F);) -log d:\log\a6.log" taskname=sys6;
    systask command "&_s -termstmt %nrstr(%hshx(G);) -log d:\log\a7.log" taskname=sys7;
    systask command "&_s -termstmt %nrstr(%hshx(H);) -log d:\log\a8.log" taskname=sys8;
    systask command "&_s -termstmt %nrstr(%hshx(I);) -log d:\log\a9.log" taskname=sys9;
    systask command "&_s -termstmt %nrstr(%hshx(J);) -log d:\log\a10.log" taskname=sys10;
    systask command "&_s -termstmt %nrstr(%hshx(K);) -log d:\log\a11.log" taskname=sys11;
    systask command "&_s -termstmt %nrstr(%hshx(L);) -log d:\log\a12.log" taskname=sys12;
    systask command "&_s -termstmt %nrstr(%hshx(M);) -log d:\log\a13.log" taskname=sys13;
    systask command "&_s -termstmt %nrstr(%hshx(N);) -log d:\log\a14.log" taskname=sys14;
    systask command "&_s -termstmt %nrstr(%hshx(O);) -log d:\log\a15.log" taskname=sys15;
    systask command "&_s -termstmt %nrstr(%hshx(P);) -log d:\log\a16.log" taskname=sys16;
    systask command "&_s -termstmt %nrstr(%hshx(Q);) -log d:\log\a17.log" taskname=sys17;
    systask command "&_s -termstmt %nrstr(%hshx(R);) -log d:\log\a18.log" taskname=sys18;
    systask command "&_s -termstmt %nrstr(%hshx(S);) -log d:\log\a19.log" taskname=sys19;
    systask command "&_s -termstmt %nrstr(%hshx(T);) -log d:\log\a20.log" taskname=sys20;
    waitfor sys1 sys2 sys3 sys4  sys5 sys6 sys7 sys8 sys9 sys10
            sys11 sys12 sys13 sys14  sys15 sys16 sys17 sys18 sys19 sys20 ;
    %let secs=%sysevalf(%sysfunc(time()) - &tym);
    %put &=secs;


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
