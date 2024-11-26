# utl-using-a-function-to-select-specific-columns-Bartosz-Jablonski-array-macro
Using a function to select specific columns Bartosz Jablonski array macro
    %let pgm=utl-using-a-function-to-select-specific-columns-Bartosz-Jablonski-array-macro;

    Using a function to select specific columns Bartosz Jablonski array macro

    barray macro in Barts github(array) and
    in my reppo

    see Barts github
    https://github.com/yabwon

    https://tinyurl.com/y9nfugth
    https://github.com/rogerjdeangelis/utl-macros-used-in-many-of-rogerjdeangelis-repositories

    Bartosz Jablonski
    yabwon@gmail.com

    There are many simpler ways to solve this problem, but I want to show Barts enhancement of the
    array macro with a simple example. This advance has legs.

    Barts macro creates two arrays using a function that genrates odd and even columns

      1. One ARRAY with all the odd  columns x1 x3 x5 ...
      2. One ARRAY with all the even columns x2 x4 x6 ...

      SOLUTIONS (all solutions require meta data manipulation using Barts macro array.
                 Users could move the meta analysis to R or Python.

         1 SAS SQL
         2 R SSQ
         3 PYTHON SQL HARDCODED

           SOAPBOX ON
             Could not create a dynamic Python solution like R and SAS because I have
             not programmed the forced indentation required by Python. Very tricky.
             This is a weakness within Python
           SOAPBOX OFF

         4 Cleanup

         I tried several pivoting, transposing and untranspose macro and could not
         get any of then to work. Seems like a unique tyoe of transpose.

    Relate
    d staxkoverflow
    https://stackoverflow.com/questions/79216486/pivot-longer-with-parallel-unlinked-sets-of-columns



    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /**************************************************************************************************************************/
    /*                          |                                          |                                                  */
    /*         INPUT            |    PROCESS                               |             OUTPUT                               */
    /*                          |                                          |                                                  */
    /*                          |                                          |                                                  */
    /*  X1 X2 X3 X4             |  X1 X2     X3 X4                         |           X1_THEN_    X3_THEN_                   */
    /*                          |          -       -                       |  GROUP       X2          X4                      */
    /*  01 02 03 04             |  02 04   | 01 03 |                       |                                                  */
    /*  11 20 31 40             |  20 40   | 11 31 | MOVE THIS             |  ODDS        01          03                      */
    /*  13 22 33 44             |  22 44   | 13 33 |                       |  odds        11          31                      */
    /*                          |          -       -                       |  odds        13          33                      */
    /* options                  |                                          |  even        02          04                      */
    /* validvarname=upcase;     |                                          |  even        20          40                      */
    /* libname sd1 "d:/sd1";    |                                          |  even        22          44                      */
    /* data sd1.have;           |                                          |                                                  */
    /* input                    |           X1   X3                        |                                                  */
    /*    X1$ X2$ X3$ X4$ ;     |          THEN THEN                       |                                                  */
    /* cards4;                  |  GROUP    X2   X4                        |                                                  */
    /* 01 02 03 04              |                                          |                                                  */
    /* 11 20 31 40              |  odds     01   03                        |                                                  */
    /* 13 22 33 44              |  odds     11   31                        |                                                  */
    /*                          |  odds     13   33                        |                                                  */
    /* ;;;;                     |         -         -                      |                                                  */
    /* run;quit;                |  even   | 02   04 |                      |                                                  */
    /*                          |  even   | 20   40 | TO HERE              |                                                  */
    /* %put &=evn1; EVN1=2      |  even   | 22   44 |                      |                                                  */
    /* %put &=evn2; EVN2=4      |         -         -                      |                                                  */
    /* %put &=evnn  EVNN=2      |                                          |                                                  */
    /*                          |  BARRAY FUNCTION GENERATES MACRO ARRAYS  |                                                  */
    /* %put &=odd1; ODD1=1      |                                          |                                                  */
    /* %put &=odd2; ODD2=3      |  %put &=evn1;  /*--- EVN1=2   ---*/      |                                                  */
    /* %put &=oddn; ODDN=2      |  %put &=evn2;  /*--- EVN2=4   ---*/      |                                                  */
    /*                          |  %put &=evnn   /*--- EVNN=2   ---*/      |                                                  */
    /*                          |                                          |                                                  */
    /*                          |  %put &=odd1;  /*--- ODD1=1   ---*/      |                                                  */
    /*                          |  %put &=odd2;  /*--- ODD2=3   ---*/      |                                                  */
    /*                          |  %put &=oddn;  /*--- ODDN=2   ---*/      |                                                  */
    /*                          |                                          |                                                  */
    /*                          |                                          |                                                  */
    /*                          | -----------------------------------------|                                                  */
    /*                          |                                          |                                                  */
    /*                          |  1 SAS SQL                               |                                                  */
    /*                          |  =========                               |                                                  */
    /*                          |                                          |                                                  */
    /*                          |   select                                 |                                                  */
    /*                          |     'odds' as group                      |                                                  */
    /*                          |     ,%do_over(odd evn                    |                                                  */
    /*                          |        ,phrase= ?odd as ?odd_then_?evn   |                                                  */
    /*                          |        ,between=comma)                   |                                                  */
    /*                          |   from                                   |                                                  */
    /*                          |      sd1.have                            |                                                  */
    /*                          |   union                                  |                                                  */
    /*                          |      all                                 |                                                  */
    /*                          |   select                                 |                                                  */
    /*                          |     'even' as group                      |                                                  */
    /*                          |     ,%do_over(evn,phrase= ?              |                                                  */
    /*                          |       ,between=comma)                    |                                                  */
    /*                          |   from                                   |                                                  */
    /*                          |      sd1.have                            |                                                  */
    /*                          |                                          |                                                  */
    /*                          | -----------------------------------------|                                                  */
    /*                          |                                          |                                                  */
    /*                          |  2 R SQL                                 |                                                  */
    /*                          |  =======                                 |                                                  */
    /*                          |                                          |                                                  */
    /*                          |    select                                |                                                  */
    /*                          |      'odds' as grp                       |                                                  */
    /*                          |      ,&selodd                            |                                                  */
    /*                          |    from                                  |                                                  */
    /*                          |       have                               |                                                  */
    /*                          |    union                                 |                                                  */
    /*                          |       all                                |                                                  */
    /*                          |    select                                |                                                  */
    /*                          |      'even' as grp                       |                                                  */
    /*                          |      ,&selevn                            |                                                  */
    /*                          |    from                                  |                                                  */
    /*                          |       have                               |                                                  */
    /*                          |                                          |                                                  */
    /*                          | -----------------------------------------|                                                  */
    /*                          |                                          |                                                  */
    /*                          |                                          |                                                  */
    /*                          |  3 PYTHON HARDCODED WITH GENERATED CODE  |                                                  */
    /*                          |                                          |                                                  */
    /*                          |  select                                  |                                                  */
    /*                          |    'odds' as grp                         |                                                  */
    /*                          |    ,x1 as x1_then_x2 , x3 as x3_then_x4  |                                                  */
    /*                          |  from                                    |                                                  */
    /*                          |     have                                 |                                                  */
    /*                          |  union                                   |                                                  */
    /*                          |     all                                  |                                                  */
    /*                          |  select                                  |                                                  */
    /*                          |    'even' as grp                         |                                                  */
    /*                          |    ,x2 , x4                              |                                                  */
    /*                          |  from                                    |                                                  */
    /*                          |    have                                  |                                                  */
    /*                          |                                          |                                                  */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options
    validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input
       X1$ X2$ X3$ X4$ ;
    cards4;
    01 02 03 04
    11 20 31 40
    13 22 33 44
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  SD1.HAVE total obs=3                                                                                                  */
    /*                                                                                                                        */
    /*    X1    X2    X3    X4                                                                                                */
    /*                                                                                                                        */
    /*    01    02    03    04                                                                                                */
    /*    11    20    31    40                                                                                                */
    /*    13    22    33    44                                                                                                */
    /*                                                                                                                        */
    /* %put &=evn1;  /*--- EVN1=2   ---*/                                                                                     */
    /* %put &=evn2;  /*--- EVN2=4   ---*/                                                                                     */
    /* %put &=evnn   /*--- EVNN=2   ---*/                                                                                     */
    /*                                                                                                                        */
    /* %put &=odd1;  /*--- ODD1=1   ---*/                                                                                     */
    /* %put &=odd2;  /*--- ODD2=3   ---*/                                                                                     */
    /* %put &=oddn;  /*--- ODDN=2   ---*/                                                                                     */
    /*                                                                                                                        */
    /**************************************************************************************************************************/


    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    %utl_gettable(sd1,have)
    /*--- PROVIDES MACRO VARIABLE _NVAR - NUMBER OF COLUMNS  ---*/
    /*--- NUMBER OF PAIRS = &_NVAR/2                         ---*/

    %array(vr,values=%utl_varlist(sd1.have));
    %barray(odd[%eval(&_nvar/2)] $
      ,function = (cats('x',2*_I_-1)) )
    %barray(evn[%eval(&_nvar/2)] $
      ,function = (cats('x',2*_I_)) )

    %let oddn=&_nvar/2;
    %let evnn=&_nvar/2;

    /*--- OUTPUT OD ARRAY FUNCTION       ---*/

    %put &=evn1;  /*--- EVN1=2   ---*/
    %put &=evn2;  /*--- EVN2=4   ---*/
    %put &=evnn   /*--- EVNN=2   ---*/

    %put &=odd1;  /*--- ODD1=1   ---*/
    %put &=odd2;  /*--- ODD2=3   ---*/
    %put &=oddn;  /*--- ODDN=2   ---*/

    proc sql;
      create
        table want as
      select
        'odds' as group
        ,%do_over(odd evn
           ,phrase= ?odd as ?odd_then_?evn
           ,between=comma)
      from
         sd1.have
      union
         all
      select
        'even' as group
        ,%do_over(evn,phrase= ?
          ,between=comma)
      from
         sd1.have
    ;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  GENERATED CODE                                                                                                        */
    /*                                                                                                                        */
    /*  select                                                                                                                */
    /*    'odds' as grp                                                                                                       */
    /*    ,x1 as x1_then_x2 , x3 as x3_then_x4                                                                                */
    /*  from                                                                                                                  */
    /*     have                                                                                                               */
    /*  union                                                                                                                 */
    /*     all                                                                                                                */
    /*  select                                                                                                                */
    /*    'even' as grp                                                                                                       */
    /*    ,x2 , x4                                                                                                            */
    /*  from                                                                                                                  */
    /*    have                                                                                                                */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  WORK.WANT                                                                                                             */
    /*                                                                                                                        */
    /*            X1_THEN_    X3_THEN_                                                                                        */
    /*   GROUP       X2          X4                                                                                           */
    /*                                                                                                                        */
    /*   odds        01          03                                                                                           */
    /*   odds        11          31                                                                                           */
    /*   odds        13          33                                                                                           */
    /*   even        02          04                                                                                           */
    /*   even        20          40                                                                                           */
    /*   even        22          44                                                                                           */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    YOU CAN CODE THIS INSIDE R AND PYTHON
    GENERATE SELECTS

    %let selodd=
           %do_over(odd evn
             ,phrase= ?odd as ?odd_then_?evn
             ,between=comma);

    %put &=selodd;

    SELODD=x1 as x1_then_x2,x3 as x3_then_x4

    %let selevn=
           %do_over(evn
             ,phrase= ?
             ,between=comma);

    %put &=selevn;

    SELEVN=x2 , x4

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-read_sas("d:/sd1/have.sas7bdat")
    print(have)

    want<-sqldf("
      select
        'odds' as grp
        ,&selodd
      from
         have
      union
         all
      select
        'even' as grp
        ,&selevn
      from
         have
      ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                                              |                                                                         */
    /* GENERATED SELECT PHRASING                    |                                                                         */
    /*                                              |                                                                         */
    /* SELODD=x1 as x1_then_x2 , x3 as x3_then_x4   |                                                                         */
    /*                                              |                                                                         */
    /* SELEVN=x2 , x4                               |                                                                         */
    /*                                              |                                                                         */
    /*                                              |                                                                         */
    /*  R                                           |      SAS                                                                */
    /*                                              |                          X1_THEN_    X3_THEN_                           */
    /*     grp x1_then_x2 x3_then_x4                |      ROWNAMES    GRP        X2          X4                              */
    /*                                              |                                                                         */
    /*  1 odds         01         03                |          1       odds       01          03                              */
    /*  2 odds         11         31                |          2       odds       11          31                              */
    /*  3 odds         13         33                |          3       odds       13          33                              */
    /*  4 even         02         04                |          4       even       02          04                              */
    /*  5 even         20         40                |          5       even       20          40                              */
    /*  6 even         22         44                |          6       even       22          44                              */
    /*                                              |                                                                         */
    /*                                              |                                                                         */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read());
    have,meta = ps.read_sas7bdat('d:/sd1/have.sas7bdat');
    want=pdsql("""
     select
       'odds' as grp
       ,x1 as x1_then_x2 , x3 as x3_then_x4
     from
        have
     union
        all
     select
       'even' as grp
       ,x2 , x4
     from
       have
    """)
    print(want);
    fn_tosas9x(want,outlib='d:/sd1/',outdsn='pywant',timeest=3);
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /*                                  |                                                                                     */
    /*  Python                          |     SAS                                                                             */
    /*                                  |                                                                                     */
    /*                                  |             X1_THEN_    X3_THEN_                                                    */
    /*      grp x1_then_x2 x3_then_x4   |     GRP        X2          X4                                                       */
    /*                                  |                                                                                     */
    /*  0  odds         01         03   |     odds       01          03                                                       */
    /*  1  odds         11         31   |     odds       11          31                                                       */
    /*  2  odds         13         33   |     odds       13          33                                                       */
    /*  3  even         02         04   |     even       02          04                                                       */
    /*  4  even         20         40   |     even       20          40                                                       */
    /*  5  even         22         44   |     even       22          44                                                       */
    /*                                  |                                                                                     */
    /**************************************************************************************************************************/

    /*  _          _
    | || |     ___| | ___  __ _ _ __  _   _ _ __
    | || |_   / __| |/ _ \/ _` | `_ \| | | | `_ \
    |__   _| | (__| |  __/ (_| | | | | |_| | |_) |
       |_|    \___|_|\___|\__,_|_| |_|\__,_| .__/
                                           |_|
    */

    %symdel selevn selodd _nvar / nowarnd

    %arraydelete(evn)
    %arraydelete(odd)

    %put &=oddn;


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
