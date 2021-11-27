[![Discord](https://img.shields.io/discord/423767742546575361.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/vpEv3HJ)
<a href=" https://github.com/moshix/mvs/blob/master/codenotary.com"><img src="https://raw.githubusercontent.com/moshix/mvs/master/secured-by-immudb.svg" width="100px;"/></a>
<a href="https://cas.codenotary.com"><img src="https://raw.githubusercontent.com/codenotary/cas/master/extra/badge/secured-by-cas.svg" width="119px;"/></a>
<br>

IBM OS/VS1 Release 6.0 Distribution Library Tapes
-------------------------------------------------

This is a repo that simply stores the DLIB tapes of OS/VS1 rel 6.0 as announced and made available by Kevin Leonard and (I think) Mark Waterbury. 
I did not contribute anything to these tapes, I simply make them available to myself here so I don't have to log on to groups.io each time to download them. 

All the credit goes to the two above mentioned gentlemen... and to International Business Machines aka IBM. 

thanks

Moshix
<br><br><b>
For a better guide than mine, read this guide by Simon Toffler (with screen shots!):  https://github.com/moshix/OSVS1/blob/main/OSVS1%20Sysgen.pdf </b>



SYSGEN INSTRUCTIONS
===================


The IBM VS1 6.0 starter system is a very basic system, with the bare functionality needed to generate your own system. You will end up with a very basic system which is certainly capable of getting all the necessary compilers installed. Once fully configured this bad boy will run a 70s Savings and Loan institution, or a freight train company, no problem. 

Note:  VS1's NIP console interface is primitive, so early replies in NIP must be of the format:

  R 00,'TEXT'

with an ID of 00 and single quotes enclosing the reply text.



<br><br><b>First Step:</b>

Use this Hercules config file:

<pre>

  CPUSERIAL 060305        # CPU serial number 
  CPUMODEL  3032          # CPU model number
  MAINSIZE  8             # Main storage size in megabytes 
  XPNDSIZE  0             # Expanded storage size in megabytes
  CNSLPORT  3270          # TCP port number to which consoles connect
  NUMCPU    1             # Number of CPUs
  LOADPARM  ......        # IPL parameter
  SYSEPOCH  1900 -28
  ARCHMODE  S/370
  CODEPAGE  437/037
  000C      3505    eof
  000D      3525    pun/pch00d.txt ascii crlf
  000E      1403    prt/prt00e.txt crlf
  001F      3215    noprompt
  0480      3420
  0481      3420
  0150      3330    dasd/dliba1.150.cckd. 
  0149      3350    dasd/fgen60.149.cckd 
  014a      3350    dasd/fdlb60.14a.cckd
</pre>

Use your telnet client to connect to Hercules (not a 3270 emulator!!) to address 01F, this will act also as the system log to make things easier. 

IPL from address 150. You will see (as the prophecy foretold)

<pre>
    IEA760A SPECIFY VIRTUAL STORAGE SIZE
</pre>    

Press Enter. You will see (if you look)

<pre>
  IEA791I  DEVICE 130 NOT READY                                        
  IEA761I PAGE=(U=148,BLK=1024)                                        
  IEA791I  DEVICE 148 NOT READY                                        
  IEA761I PAGE=(U=150,BLK=1024)                                        
  IEA761I PAGE=(U=158,BLK=1024)                                        
  IEA791I  DEVICE 158 NOT READY                                        
  IEA761I PAGE=(U=1C0,BLK=1024)                                        
  IEA791I  DEVICE 1C0 NOT READY                                        
  IEA761I PAGE=(U=1D0,BLK=1024)                                        
  IEA791I  DEVICE 1D0 NOT READY                                        
  IEE054I DATE=92.366,CLOCK=18.17.48
  IEE054I DATE=92.366,CLOCK=18.17.48,GMT
  IEA101A SPECIFY SYSTEM AND/OR SET PARAMETERS FOR RELEASE 06.0E OS/VS1 
  
</pre>

Reply with:
<pre>
  r 00,'auto=starter,page=(v=dliba1,blk=4096),q=(,f)'
</pre>

This will cold start the job queue (remember, no HASP or JES in OS/VS1), and allocate the required page data set on volume DLIBA1. 

You will see, according to the legend of Zork

<pre>
 IEA764I NIP01,CMD01,DFN01,JESNULL,SET01          
  IEA765I  HARDCPY=,DEVSTAT=ALL                    
  IEA103I DATASET SYS1.DUMP NOT FOUND BY LOCATE    
  IEA135A SPECIFY SYS1.DUMP TAPE UNIT ADDRESS OR NO 
</pre>

Press ENTER. You will see

<pre>
  IEA208I SYS1.DUMP FUNCTION INOPERATIVE
  IEA210I  SYS1.PAGE ALLOCATED ON DLIBA1
  IEA106I IEAAPF00 NOT FOUND IN SYS1.PARMLIB
  181903 8000  IEE140I SYSTEM CONSOLES
  001    CONSOLE/ALT  COND AUTH    ID AREA  ROUTCD                        
  001      01F/009    M  ALL      01      ALL
  001      009/01F    N  INFO    02      NONE                          
  001      010/01F    N  INFO    03 Z,A  NONE                          
  001      011/01F    N  NONE    04      NONE                          
  001      014/01F    N  INFO    05 Z,A  NONE                          
  001      015/01F    N  NONE    06      NONE                          
  001      019/01F    N  INFO    07 Z,A  NONE                          
  001      01A/01F    N  INFO    08 Z,A  ALL
  001      01B/01F    N  INFO    09 Z,A  ALL
  001      209/01F    N  INFO    10      NONE                          
  001      219/01F    N  INFO    11 Z,A  NONE                          
  001      21F/01F    N  INFO    12      NONE                          
  IEF031I SYSGEN VALUES TAKEN FOR JES                                    
  IEE101A READY                                                          
  IEE029I Q=(,F),SWPRM=(U),JLPRM=(U)                                      
 *00 IEF068A  VOLUME DLIBA1 REQUIRES FORMATTING. REPLY 'FORMATV' OR 'RESET'
 </pre>
 
 Reply with R 00,FORMATV 
 
 You will see
 
 <pre>
 IEE600I REPLY TO 00 IS 'FORMATV'                    
 *01 IEC107D E 150,DLIBA1,MASTER,SCHEDULR,SYS1.SYSPOOL 
 </pre>
 
 Reply with R 1,U
 
 The following incantation will hit your eyeballs
 
 <pre>
   IEE600I REPLY TO 01 IS 'U'
 *02 IEC107D E 150,DLIBA1,MASTER,SCHEDULR,SYS1.SYSWADS
 </pre>
 
 Reply with R 2,U
 
 This will format the spool and job areas, and a command to monitor jobs will be automatically be issued, as well as initiator P0 will be launched:
 
 <pre>
  IEE600I REPLY TO 02 IS 'U'                
  IEE052I MN      JOBNAMES,T
  IEE052I S        INITSWA.P0
  IEE048I INITIALIZATION COMPLETED          
  IEF403I INITSWA  STARTED TIME=18.20.58 P00 
  IEF005I PARTITION WAITING FOR WORK  P00
 </pre>
 
 Now the system can only run one job at a time. To allow 2 jobs to execute we need to change the SYS1.PARMLIB and the SYS2.PROCLIB. There is TSO or any interactive facility here (that's why I run this under VM/370 exclusively...).So we need a small batch job to make the change. Use this JCL by Kevin Leonard:
 
 <pre>
  //UPDATES  JOB 1,SOFTWARE,CLASS=A,MSGCLASS=A
  //*
  //* 2020/12/12 @kl updates to VS1 6.0 starter system
  //*                SYS1.PARMLIB/SYS1.PROCLIB
  //*
  //PARMLIB EXEC PGM=IEBUPDTE,PARM=NEW
  //SYSPRINT DD  SYSOUT=A
  //SYSUT2  DD  DISP=SHR,DSN=SYS1.PARMLIB,UNIT=3330,VOL=SER=DLIBA1
  //SYSIN    DD  *
  ./ ADD NAME=NIP01
  HARDCPY=,DEVSTAT=ALL,PAGE=(V=DLIBA1,BLK=4096)                          00001000
  ./ ADD NAME=DFN01
  P0=(3072K,LAST),END                                                    00001000
  ./ ENDUP
  /*
  //*
  //PARMLIB EXEC PGM=IEBUPDTE,PARM=NEW
  //SYSPRINT DD  SYSOUT=A
  //SYSUT2  DD  DISP=SHR,DSN=SYS1.PROCLIB,UNIT=3330,VOL=SER=DLIBA1
  //SYSIN    DD  DATA
  ./ ADD NAME=INITSWA
  //INITSWA PROC SWA=1600                                                00001000
  //IEFPROC EXEC PGM=IEFIIC,PARM='SWA=&SWA'                              00002000
  ./ ENDUP
  /*
  //
  </pre>
  
  Put the above JCL into a file on your Linux or windoze machine (or whatever) and read it into the system thru the card reader at address 00C like this:
  
  devinit 00c yourJCLfile.txt
  
  To read in the job cards (remember, this is a card reader.  The devinit only places the card deck into the card reader...), do this:
  
  sf ,00C
  
  Turn up the volume on your computer and you will hear the cards flying thru the reader (just kidding). 
  
  Since this job will attempt to open up two date-protected data sets (SYS1.PARMLIB and SYS1.PROCLIB, naturally), you will get a warning like this
  
  <pre>
   IEF403I UPDATES  STARTED TIME=18.34.58 P00              
 *04 IEC107D E 150,DLIBA1,UPDATES,PARMLIB,SYS1.PARMLIB P00
 </pre>
 
 Tell the system yeah, yeah, I know what I am doing by tellit it:
 
 R 4,U
 
 It is now happy and says 
 
 <pre>
  IEE600I REPLY TO 04 IS 'U'                              
*05 IEC107D E 150,DLIBA1,UPDATES,PARMLIB,SYS1.PROCLIB P00 
</pre>

 Same thing again, just do R 05,U
 
 <pre>
 IEE600I REPLY TO 05 IS 'U'                              
 IEF404I UPDATES  ENDED  TIME=18.36.50 P00              
 IEF005I PARTITION WAITING FOR WORK  P00
 </pre>
 
 That's it, first step done. Print out the very interesting job output (looks so 70s!) by issuing a start writer command with
 
 SF ,00E
 
 Now, shut down the system and IPL again. 
 
 <br><br><b>Second Step:</b>

 Right ater IPL, press ENTER to all the messages from NIP

 <pre>
 
  IEA760A SPECIFY VIRTUAL STORAGE SIZE          
  </pre>
  
  Press ENTER and you see magically
  
  <pre>
  IEA761I PAGE=(U=130,BLK=1024)                                    
  IEA791I  DEVICE 130 NOT READY
  IEA761I PAGE=(U=148,BLK=1024)                                    
  IEA791I  DEVICE 148 NOT READY
  IEA761I PAGE=(U=150,BLK=1024)                                    
  IEA761I PAGE=(U=158,BLK=1024)                                    
  IEA791I  DEVICE 158 NOT READY
  IEA761I PAGE=(U=1C0,BLK=1024)                                    
  IEA791I  DEVICE 1C0 NOT READY
  IEA761I PAGE=(U=1D0,BLK=1024)                                    
  IEA791I  DEVICE 1D0 NOT READY
  IEE054I DATE=92.366,CLOCK=18.39.46                                  
  IEE054I DATE=92.366,CLOCK=18.39.46,GMT                              
  IEA101A SPECIFY SYSTEM AND/OR SET PARAMETERS FOR RELEASE 06.0E OS/VS1
  </pre>
  
  Press ENTER again. You see out of nowhere
  
  <pre>
                                                                      
  IEA103I DATASET SYS1.DUMP NOT FOUND BY LOCATE
  IEA135A SPECIFY SYS1.DUMP TAPE UNIT ADDRESS OR NO
  </pre>
  
  Now, that's a dry request... (or NO)...whatever press ENTER. This will appear 
  
  <pre>
  IEA208I SYS1.DUMP FUNCTION INOPERATIVE                              
  IEA106I IEAAPF00 NOT FOUND IN SYS1.PARMLIB 
  *00 IEC107D E 150,DLIBA1,MASTER,SCHEDULR,SYS1.SYSWADS 
  </pre>
  
  Reply with R 0,U
  
  OS/VS1 will start the P0 initiator and wait for jobs to be submitted. 
  
  <pre>
  IEE600I REPLY TO 00 IS 'U'                
  IEE048I INITIALIZATION COMPLETED          
  IEF403I INITSWA  STARTED TIME=18.42.55 P00 
  IEF005I PARTITION WAITING FOR WORK  P00
  </pre>
  
  Thassit!! OS/VS1 at your command. Start a reader or writer whenever you need one (see above commands SF). You only start the writers to printer once at the beginning of your IPL, afterwards al print output will go to the printer automagically. 
  
  
  
  Now, remember there are no compilers, linkers or anything in the system. You will have to go and install those from the Jay Mosely compiler tapes. But that's all simple and easy. 
  
  If you run this system under VM/370 you will be able to edit jobs and submit them from within the VM/370 full screen editor, but, hey, up to you. 
  
  Documentation
  =============
  
  Kevin Leonard, our amazing JES3, OS/VS1 and OS/VS2 expert extraordinnaire, makes some manuals available here: 
  http://www.j76.org/vs1/documentation.html
  
  
  If you have any problems with this procedure, then post an issue here on github. 
  
  
  
 
