# PSION News

| No | Year |            | Prog                                    | Comment |
|----|------|------------|-----------------------------------------|---------|
| 01 | 1987 | Summer     | Printing the organised diary            | bug     |
| 02 | 1987 | Autumn     | Organise your travel money              |         |
| 03 | 1988 | Winter     | Diary update (01)                       | Fix 01  |
| 03 | 1988 | Winter     | Making the Archive/Organiser connection |         |
| 04 | 1988 | Summer     | Locking the Organiser                   |         |
| 04 | 1988 | Summer     | Stopwatch                               |         |
| 05 | 1988 | Autumn     | Sorting entries                         |         |
| 05 | 1988 | Autumn     | Auto                                    |         |
| 05 | 1988 | Autumn     | Travel                                  |         |
| 06 | 1989 | Winter     | Comparing Prices                        |         |
| 06 | 1989 | Winter     | Search                                  |         |
| 06 | 1989 | Winter     | Calculus ASIN()                         |         |
| 06 | 1989 | Winter     | Calculus ACOS()                         |         |
| 07 | 1989 | June-Sept  | Formatting a 32k Rampak                 |         |
| 07 | 1989 | June-Sept  | Keeping a check on Telephone calls      |         |
| 08 | 1989 | Sept-Dec   | Morse                                   |         |
| 08 | 1989 | Sept-Dec   | UDG                                     |         |
| 08 | 1989 | Sept-Dec   | DESK                                    |         |
| 08 | 1989 | Sept-Dec   | PRINTDIG                                |         |
| 09 | 1990 | JAn-April  | QTIME                                   |         |
| 10 | 1990 | April-Sept | Receiving files from a PC               |         |
| 11 | 1990 | Oct-Dec    | Winning the Pools                       |         |
| 12 | 1991 | Autumn     | Copying Records                         |         |
| 13 | 1992 | Summer     | Byorithm                                |         |


Note:

Calculuus, this 2 functions are missing in CM XP, added in LZ.\
In the Maths Pack also others missing functions.


## PSION News 01

Printing the organised diary

There is un bug in this program, crrection published in issue 03.

```basic
pdiary:
LOCAL BASE%,LEN%,C%,END%
BASE%=PEEKW($2004)
PRINT "Print to end of month (1-12) ";
INPUT END%
A::
LEN%=PEEKB(BASE%)
IF LEN%=0
 PRINT " END OF DIARY"
 PRINT " Preas Any Key"
 LPRINT CHR$(12)
 RETURN
ENDIF
IF PEEKB(BASE%+2)+1<=END%
 IF PEEKB(BASE%+3)<=10 :LPRINT "0"; :ENDIF
 LPRINT (PEEKB(BASE%+3)+1);"/";
 IF PEEKB(BASE%+2)<=10 :LPRINT "0"; :ENDIF
 LPRINT (PEEKB(BASE%+2)+1);"/";
 LPRINT PEEKB(BASE%+1);" ";
 IF PEEKB(BASE%+4)<10 :LPRINT "O"; :ENDIF
 LPRINT PEEKB(BASE%+4);":";
 IF PEEKB(BASE%+5)=0
  LPRINT "00 ";
  ELSE
  LPRINT "30 ";
 ENDIF
  C%=PEEKB(BASE%)
  BASE%=BASE%+7
  LPRINT " ";
  DO
   LPRINT CHR$(PEEKB(BASE%));
   C%=C%-1
   BASE%=BASE%+1
  UNTIL C%=0
  LPRINT
  GOTO A::
ENDIF
```

## PSION News 02

Organise your travel money

```basic
exch:
local fm,c%
c%=menu("NEW,SAME")
if c%=1
 print "Rate to the ";chr$(237)
 input fm
 m9=fm
endif
ex::
if m9=0
 print "No exchange rat...run again"
 get
 return
endif
cls
print "Foreign cost: "
input fm
if fm=0
 return
endif
print "=";chr$(237);fix$(fm/m9,2,8)
c%=get
if c%<>1
 goto ex::
endif
```

## PSION News 03

Diary update (01)


```basic
```

Making the Archive/Organiser connection

```basic
proc filexp
REM orders and compiles all "file.dbf" records as "filexp.lis"
trace
order a$;a
spoolon "filexp"
first
all
lprint a$;" ";b$;" ";c$;" ";d$;" ";e$;" ";f$;" ";g$;" ";h$;" ";i$
endall
lprint chr(26)
spool off
trace
endproc
```

## PSION News 04

Locking the Organiser

```basic
LOCK:
LOCAL A$(20),C%, C$( 20 )
C$= "WORD"
REM REPLACE "WORD" BY YOUR PASSWORD (UPPER CASE)
ON ERR START::
START::
OFF
AGAIN::
CLS
VIEW (1,"IF FOUND, RETURN TO TIM ON 01-723-9408")
REM PUT YOUR OWN MESSAGE IN THE ABOVE LINE
C%=C%+1 :CLS
PRINT "PASSWORD PLEASE"
INPUT A$
IF UPPER$(A$)<>C$ AND C%< 2 :BEEP 600,400 :GOTO AGAIN::
ELSEIF UPPER$(A$)<>C$ :C%=0 :BEEP 600,400 :GOTO START::
ENDIF
CLS :BEEP 400,100
```

Stopwatch

```basic
WATCH :
LOCAL K$(1),S%,SE%,MI%
POKEB $007C,O:REM disables auto-switch off
REM *WARNING* screen will not turn off after 5 mins
CLS :PRINT "S=STOP, L=LAP" :PRINT "PRESS ANY KEY .. . " ;
GET :CLS
zero::
MI%=0:SE%=0:S%=SECOND
loop::
 K$=KEY$
 IF UPPER$(K$)="S" : REM press s to stop
  GOTO pause ::
 ENDIF
  IF UPPER$(K$)="L" :REM press L for lap time
  AT 1,2 :PRINT "LAP: ";MI%;": ";
  IF SE%<10 :PRINT "0"; :ENDIF
  PRINT SE%; " ";
 ENDIF
 IF SECOND<>S%
  S%=SECOND : SE%=SE% +1
  IF SE%=60 : SE%=0:MI%=MI%+1 :ENDIF
  PRINT CHR$(14)+"MINS",MI% , "SECS" ;" " ·
  IF SE%<10 : PRINT "O" ; : ENDIF
  PRINT SE%;
 ENDIF
GOTO loop::
pause: :
 PRINT CHR$(15)+"Restart/ Zero/ End" ;
 K$=UPPER$(GET$)
 PRINT CHR$(15) ;
 IF K$="R":REM press R to restart the clock
 GOTO loop::
 ELSEIF K$="Z": REM press Z to zero the clock
 CLS :GOTO zero: :
 ELSEIF K$<>"E" :REM press E to end
 GOTO pause::
ENDIF
POKEB $007C,5 :REM put auto-switch off back to normal
REM
REM *WARNING* if this procedure is left running
REM the screen will not switch off automatically


```

## PSION News 05

```basic
```

## PSION News 06

```basic
.-\SI,: ( A l I
IF AllS(..\ 1 1= 1 :RETURN ,\l *P l/2
ELSElF ARS ( A l l > l : CLS :PRI;>;T" OCT OF IUNGE" ;CI_IHS( JG)
PAUSE 20 :HETU!!\
ELSE :H~run~ OEG(AT.-\N(.-\ 1/SQH( 1-AI •A l I))
E:-IDJ F
.-\COS: I .-\1)
IF Al=! :REHIRN 0
ELSEIF .-\ l=- 1 :HETUR, PI
ELSElF .-\BS(.-111>1 : CL$ :PRl NT" OUT OF RANGE " ;CIIRS(lG)
PAUSE 20 :RETURS
ELSE : llETUR, DEG(I . 5i0i9G- AT,\ N(,\ l/SQR(l - Al* . .\l)II
ENDIF
```

## PSION News 07

```basic
```

## PSION News 08

```basic
```

## PSION News 09

```basic
```

## PSION News 10

```basic
```

## PSION News 11

```basic
```

## PSION News 12

```basic
```

## PSION News 13

```basic
```












