# CBT587
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 587 is from Matt Gates and contains his modified copy     *   FILE 587
//*           of an old version of the VTOC command, which is now   *   FILE 587
//*           on File 112 of this tape.  This version of the VTOC   *   FILE 587
//*           command is circa 1990, and contains desirable         *   FILE 587
//*           improvements (as described below).  However the MVS   *   FILE 587
//*           operating system has passed this version by, and the  *   FILE 587
//*           source code is being presented here, awaiting the     *   FILE 587
//*           work of somebody to modernize it to the current       *   FILE 587
//*           version of the operating system, or to merge its      *   FILE 587
//*           very nice features into the File 112 version of the   *   FILE 587
//*           VTOC command.                                         *   FILE 587
//*                                                                 *   FILE 587
//*           Dave Cartwright has made this version of VTOC usable  *   FILE 587
//*           for MVS 3.8 (OS/VS2) running under Hercules.  See     *   FILE 587
//*           the Improvements Log below.                           *   FILE 587
//*                                                                 *   FILE 587
//*           email:  Please contact Sam Golob at                   *   FILE 587
//*                   sbgolob@cbttape.org                           *   FILE 587
//*                                                                 *   FILE 587
//*  - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -  *   FILE 587
//*                                                                 *   FILE 587
//*  Improvements Log:                                              *   FILE 587
//*                                                                 *   FILE 587
//*   09/02 - Dave Cartwright has gotten this version of VTOC       *   FILE 587
//*           to work for MVS 3.8 (OS/VS2) under Hercules.          *   FILE 587
//*           See member ASM370, which assembles member VTOC370.    *   FILE 587
//*                                                                 *   FILE 587
//*   09/02 - HELP member for this version was added by Matt Gates. *   FILE 587
//*                                                                 *   FILE 587
//*  - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -  *   FILE 587
//*                                                                 *   FILE 587
//*   A description of the current state of this program, and of    *   FILE 587
//*   the improvements contained therein, follows (in the words     *   FILE 587
//*   of the author):   (Please see the Improvements Log above.)    *   FILE 587
//*                                                                 *   FILE 587
//*      This is an older version of the VTOC command looking       *   FILE 587
//*      for some talented and worthy person who is willing to      *   FILE 587
//*      merge it with the current VTOC command.  I'm sorry I       *   FILE 587
//*      did not feed this back to the world when I did the         *   FILE 587
//*      work, but now I would like to make amends.                 *   FILE 587
//*                                                                 *   FILE 587
//*      Well here's the idea I had. Even though my version         *   FILE 587
//*      left off around 1990 there are still many improvements     *   FILE 587
//*      that could be incorporated into the existing VTOC          *   FILE 587
//*      command. I warn you it won't be easy; I had to scare       *   FILE 587
//*      up additional base registers in my time.                   *   FILE 587
//*                                                                 *   FILE 587
//*      Why would you want this stuff?                             *   FILE 587
//*                                                                 *   FILE 587
//*      1) I coded a VTOCMAP module which generates a mapping      *   FILE 587
//*      of the VTOC that can optionally be outputted to the        *   FILE 587
//*      VTOCOUT DD if you use the MAP operand, but the beauty      *   FILE 587
//*      was I always did a VTOC integrity check to show gaps       *   FILE 587
//*      or overlapping extents in the VTOCs I was reading and      *   FILE 587
//*      put out a message if there were errors even if you did     *   FILE 587
//*      not have the MAP operand.                                  *   FILE 587
//*                                                                 *   FILE 587
//*      2) I coded the "NOT" of many operands like NLEV            *   FILE 587
//*      (things of NOT this high level qualifier would show in     *   FILE 587
//*      the output), NCON (NOT containing), NEND (NOT ending).     *   FILE 587
//*                                                                 *   FILE 587
//*      3) Added  BEG and NBEG (beginning with and not             *   FILE 587
//*      beginning with like BEG(SYS) which shows SYSxxxxx as       *   FILE 587
//*      opposed to coding LE(SYS1 SYS2 SYS3 etc).                  *   FILE 587
//*                                                                 *   FILE 587
//*      4) Minor allowed > < = ^= etc instead of GT, LT, EQ,       *   FILE 587
//*      and NE on LIM and ANDx operands.                           *   FILE 587
//*                                                                 *   FILE 587
//*      5) Increased ANDx and ORx to allow many more               *   FILE 587
//*      conditionals than the original VTOC command had.           *   FILE 587
//*                                                                 *   FILE 587
//*      6) Allowed * on checking for dates, where * means          *   FILE 587
//*      current date.  I.E. LIM(CDATE LT *) meaning I want         *   FILE 587
//*      things created prior to today.                             *   FILE 587
//*                                                                 *   FILE 587
//*      7) Did something to allow KEYLE (key length) for ISAM      *   FILE 587
//*      I think.                                                   *   FILE 587
//*                                                                 *   FILE 587
//*      8) Added LOWLEVEL and NLOWLEVEL to allow check of          *   FILE 587
//*      entire low level qualifier as opposed to END which         *   FILE 587
//*      only checks that the last few characters match.            *   FILE 587
//*                                                                 *   FILE 587
//*      9) Put volume ID in error messages for better              *   FILE 587
//*      knowledge of which pack had a problem.                     *   FILE 587
//*                                                                 *   FILE 587
//*      10) Turn off catalog search when extents are zero.         *   FILE 587
//*                                                                 *   FILE 587
//*      11) Provide ability to show whether last open of a         *   FILE 587
//*      dataset was for update or not.                             *   FILE 587
//*                                                                 *   FILE 587
//*      12) Allow OPTCD as a LIM value, was originally done to     *   FILE 587
//*      spot use of OPTCD EQ W which I didn't want people          *   FILE 587
//*      using.                                                     *   FILE 587
//*                                                                 *   FILE 587
//*      All modifications are well documented within the code,     *   FILE 587
//*      line by line and in a modifications list up front in       *   FILE 587
//*      the modules.                                               *   FILE 587
//*                                                                 *   FILE 587
//*      Definitely as this code currently stands it only knew      *   FILE 587
//*      about UCB addresses that were 000-FFF; it knew not of      *   FILE 587
//*      0000-FFFF UCB addresses.                                   *   FILE 587
//*                                                                 *   FILE 587
```
