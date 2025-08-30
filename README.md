# utl-select-elements-of-a-target-array-based-on-zero-one-values-in-a-state-array-untranspose
Select elements of a target array base on values in another array arts untranspose
    %let pgm=utl-select-elements-of-a-target-array-based-on-zero-one-values-in-a-state-array-untranspose;

    %stop_submission;

    Select elements of a target array base on values in another array arts untranspose

    see github for complete solution

    SOAPBOX ON
      Art's untranspose macro
      I was unabl to get the posted solution to work.
      It looks like it fails when the state arrays have multiple ones in a row.
      This solution finds 4 more observations
    SOAPBOX OFF


    SELECT ELEMENTS OF TARGET ARRAY BASED ON A 1 IN STATE ARRAY


             TARGET ARRAY     STATE ARRAY

      ID       D1 D2 D3        M1 M2 M3

       1        A  B  C         1  1  0

     UNTRANSPOSE (drop C)

      ID        D

       1        A
       1        B

    SOLUTION

    %utl_untranspose(
      data=have
     ,out=prewant(where=(not missing(d)))
     ,by=id
     ,id=nums
     ,var=d m p
     ,missing=yes);

    github
    https://tinyurl.com/4vxktsr8
    https://github.com/rogerjdeangelis/utl-select-elements-of-a-target-array-based-on-zero-one-values-in-a-state-array-untranspose

    sas communities
    https://tinyurl.com/3tt5xn2s
    https://communities.sas.com/t5/SAS-Programming/Transpose-and-rename-with-conditions/m-p/755737#M238540


    /**************************************************************************************************************************/
    /* INPUT                              | PROCESS                               | OUTPUT                                    */
    /* =====                              |                                       | ======                                    */
    /* ID D1 D2 D3  M1 M2 M3  P1 P2 P3    | %utl_untranspose(                     | FRO ID   D   M   P                        */
    /*                                    |   data=have                           |                                           */
    /*  1  A  B  C   1  1  0   0  0  1    |  ,out=prewant(where=(not missing(d))) |  M   1   A   1   0                        */
    /*  2  .  .  C   0  0  1   0  0  0    |  ,by=id                               |  M   1   B   1   0                        */
    /*  3  .  B  .   0  0  0   0  1  0    |  ,id=nums                             |  M   7   A   1   0                        */
    /*  4  .  .  .   0  0  0   0  0  0    |  ,var=d m p                           |  M   9   C   1   0                        */
    /*  5  A  .  .   1  0  0   0  0  1    |  ,missing=yes);                       |  M   5   A   1   0                        */
    /*  6  A  .  C   0  1  1   1  0  0    |                                       |  M   2   C   1   0                        */
    /*  7  A  .  C   1  0  0   0  0  1    |  proc sql;                            |  M   6   C   1   0                        */
    /*  8  .  .      0  0  0   0  0  0    |    create                             |                                           */
    /*  9  A  B  C   0  0  1   1  1  0    |       table want as                   |  P   6   A   0   1                        */
    /*                                    |    select                             |  P   1   C   0   1                        */
    /* data have;                         |       ifc(M=1,"M","P") as fro         |  P   9   I   0   1                        */
    /* input                              |      ,*                               |  P   9   A   0   1                        */
    /*  id d1$ d2$ d3$                    |    from                               |  P   3   B   0   1                        */
    /*     m1  m2  m3                     |      prewant                          |  P   7   C   0   1                        */
    /*     p1  p2  p3;                    |    order                              |                                           */
    /* cards4;                            |      by fro                           |                                           */
    /* 1  A  B  C  1  1  0  0  0  1       |  ;quit                                |                                           */
    /* 2  .  .  C  .  0  1  0  0  0       |                                       |                                           */
    /* 3  .  B  .  .  0  0  0  1  0       |                                       |                                           */
    /* 4  .  .  .  .  0  0  0  0  0       |                                       |                                           */
    /* 5  A  .  .  1  0  0  0  0  1       |                                       |                                           */
    /* 6  A  .  C  .  1  1  1  0  0       |                                       |                                           */
    /* 7  A  .  C  1  0  0  0  0  1       |                                       |                                           */
    /* 8  .  .  .  .  0  0  0  0  0       |                                       |                                           */
    /* 9  A  B  C  .  0  1  1  1  0       |                                       |                                           */
    /* ;;;;                               |                                       |                                           */
    /* run;quit;                          |                                       |                                           */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    data have;
    input
     id d1$ d2$ d3$
        m1  m2  m3
        p1  p2  p3;
    cards4;
    1  A  B  C  1  1  0  0  0  1
    2  .  .  C  .  0  1  0  0  0
    3  .  B  .  .  0  0  0  1  0
    4  .  .  .  .  0  0  0  0  0
    5  A  .  .  1  0  0  0  0  1
    6  A  .  C  .  1  1  1  0  0
    7  A  .  C  1  0  0  0  0  1
    8  .  .  .  .  0  0  0  0  0
    9  A  B  C  .  0  1  1  1  0
    ;;;;
    run;quit;

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    %utl_untranspose(
      data=have
     ,out=prewant(where=(not missing(d)))
     ,by=id
     ,id=nums
     ,var=d m p
     ,missing=yes);


    proc sql;
      create
         table want as
      select
         ifc(M=1,"M","P") as fro
        ,*
      from
        prewant
      order
        by fro
    ;quit


    /**************************************************************************************************************************/
    /* FRO    ID    NUMS    D    M    P                                                                                       */
    /*                                                                                                                        */
    /*  M      1      1     A    1    0                                                                                       */
    /*  M      1      2     B    1    0                                                                                       */
    /*  M      7      1     A    1    0                                                                                       */
    /*  M      9      3     C    1    0                                                                                       */
    /*  M      5      1     A    1    0                                                                                       */
    /*  M      2      3     C    1    0                                                                                       */
    /*  M      6      3     C    1    0                                                                                       */
    /*                                                                                                                        */
    /*  P      6      1     A    0    1                                                                                       */
    /*  P      1      3     C    0    1                                                                                       */
    /*  P      9      2     I    0    1                                                                                       */
    /*  P      9      1     A    0    1                                                                                       */
    /*  P      3      2     B    0    1                                                                                       */
    /*  P      7      3     C    0    1                                                                                       */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
