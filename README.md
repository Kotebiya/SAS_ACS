# SAS_ACS
SAS Code for Importing ACS PUMS from the Census Bureau

This is an example of the setup for importing the data directly from the zip files without extracting them.

* Make sure to change this to any number between 07 and 14. ;
%LET YEAR=07;
* Make sure to edit this to reflect the correct directory location on your system ;
%LET SOURCE=/folders/myshortcuts/DATA/ACS/20&YEAR./1YR/;

LIBNAME ACS&YEAR. "&SOURCE.";

FILENAME H SASZIPAM "&SOURCE.csv_hus.zip";
FILENAME P SASZIPAM "&SOURCE.csv_pus.zip";

DATA PART1;
	INFILE P(ss&YEAR.pusa.csv)
		FIRSTOBS=2
 		DSD DLM='2C0D'x
		LRECL=2000 MISSOVER ;
	%INCLUDE "&SOURCE.P_LENGTH.TXT";
	%INCLUDE "&SOURCE.P_INPUT.TXT";
	%INCLUDE "&SOURCE.P_LABEL.TXT";
RUN;

DATA PART2;
	INFILE P(ss&YEAR.pusb.csv)
		FIRSTOBS=2
 		DSD DLM='2C0D'x
		LRECL=2000 MISSOVER ;
	%INCLUDE "&SOURCE.P_LENGTH.TXT";
	%INCLUDE "&SOURCE.P_INPUT.TXT";
	%INCLUDE "&SOURCE.P_LABEL.TXT";
RUN;

DATA ACS&YEAR..P;
	SET PART1 PART2;
RUN;

DATA HART1;
	INFILE H(ss&YEAR.husa.csv)
		FIRSTOBS=2
 		DSD DLM='2C0D'x
		LRECL=2000 MISSOVER ;
	%INCLUDE "&SOURCE.H_LENGTH.TXT";
	%INCLUDE "&SOURCE.H_INPUT.TXT";
	%INCLUDE "&SOURCE.H_LABEL.TXT";
RUN;

DATA HART2;
	INFILE H(ss&YEAR.husb.csv)
		FIRSTOBS=2
 		DSD DLM='2C0D'x
		LRECL=2000 MISSOVER ;
	%INCLUDE "&SOURCE.H_LENGTH.TXT";
	%INCLUDE "&SOURCE.H_INPUT.TXT";
	%INCLUDE "&SOURCE.H_LABEL.TXT";
RUN;

DATA ACS&YEAR..H;
	SET HART1 HART2;
RUN;
