---
layout: page
category: cook
title: "How to set up"
order: 0
---
#### 1. Download IgBlast program

IgBlast program can be downloaded from <https://ftp.ncbi.nih.gov/blast/executables/igblast/release/LATEST>.  We provide pre-compiled programs for Linux, Windows, MacOs as well as source code for you to build on your own platform. 

Note, for versions prior to 1.13.0 only, you also need to download the old internal_data and optional_file folders from <https://ftp.ncbi.nih.gov/blast/executables/igblast/release/>.  We strongly encourage you to get the latest IgBlast version with built-in internal_data and optional_file.

#### 2. Get blast database for germline V, D, and J gene sequences.  

IgBlast allows you to search any germline databases of your choice (using -germline_db_V, -germline_db_J and -germline_db_D options).

The NCBI mouse germline gene databases (i.e., mouse_gl_V, etc.) are supplied on our ftp site (<https://ftp.ncbi.nih.gov/blast/executables/igblast/release/database/>.  Also see <https://www.ncbi.nlm.nih.gov/igblast/> about database details).
  
To search IMGT germline sequences, you need to download them from IMGT web site 
([http://www.imgt.org/vquest/refseqh.html#VQUEST](http://www.imgt.org/vquest/refseqh.html#VQUEST) ).  You need to download all V, D and J sequences for whatever organisms you are interested in.  Combine all V, all D and all J sequences, respectively, into separate files (i.e., one file for all V sequences, one for all D sequences and one file all for J sequences).  After you have downloaded the sequences, invoke our utility tool edit_imgt_file.pl (in the bin directory in the IgBlast release package) to process these sequences (i.e., to change the long IMGT definition lines to germline gene names only).  For example:

```
bin/edit_imgt_file.pl imgt_file > my_seq_file
```

Then you can use NCBI's makeblastdb tool (also in the bin directory ) to make the blast database from the above output file.  For example:

```
bin/makeblastdb -parse_seqids -dbtype nucl -in my_seq_file
```

Now you can use my_seq_file as blast database file for IgBlast.

#### Other notes on setting up IgBlast 
The internal_data directory contains data internal to igblast program only and users should NEVER add, delete, move, copy or edit any files in this directory.  Igblastn program expects the internal_data directory under current directory (i.e., where you run igblast 
program) or a path pointed to by IGDATA environmental variable (avoid using space in your path name for 
Windows system). Note that this directory does NOT contain any germline gene databases you should search (see above 
for how you can obtain a germline gene database).

The optional_file directory contains files that indicate germline J gene coding frame start position (position is 0-based), the J gene type, and the CDR3 end position for each sequence in the germline J sequence database (Fields are tab-delimited).  The suppliled annotation information is only for NCBI or IMGT  germline gene sequence database (including gene names as well as the sequences).   If you search your own database and if it contains different sequences or sequence identifiers, then you need to edit the corresponding file accordingly (Enter -1 if the frame information is unknownor) or you won't get proper frame status or CDR3 information (other results will still be shown correctly).  You need to use -auxiliary_data option to specify your file. You can directly supply a path to this file or put it under a path pointed to by IGDATA environmental variable.

#### Procedure to set up IgBLAST for custom organism

IgBLAST internally only supports organisms including human, mouse, rat, rabbit and rhesus_monkey.  However, starting with version 1.16.0, IgBLAST also supports custom organism, although you need to prepare additonal files as follows.

1.	 Make the germline V gene annotation file for your own organism.  The purpose of this file is to provide information such as CDR/FWR regions for your germline V genes (see internal_data/human/human.ndm.imgt for an example).  This file must be put under the internal_data/my_organism folder (for example  internal_data/sheep).  For annotations using the IMGT numbering system, the file must be named my_organism.ndm.imgt for igblastn  (for example sheep.ndm.imgt) or my_organism.pdm.imgt for igblastp (for example sheep.pdm.imgt).   If you use the Kabat numbering system, the file must be named my_organism.pdm.kabat or my_organism.pdm.kabat.  Note that you do not need to make annotations for all alleles of a V gene.  You only need to annotate one allele.


2.	Make a blast sequence database for germline V genes that correspond to what you have annotated above.  This database needs to be named my_organism_V (for example sheep_V). Make sure you use the -parse_seqids flag when using makeblastdb.  The blast database files need to be put under internal_data/my_organism folder (for example  internal_data/sheep).  Note that this database is intended only as internal data for IgBLAST and does not need to be updated unless there is a new germline V gene (not new allele).  Typically the germline V gene database you want to search (i.e., specified by -germline_db_V parameter) is a different one (for example, the one that contains all alleles).


3.	If you also want CDR3/FWR4 information, you need to supply a file that has information such as CDR3 stop for germline J genes (see optional_file/human_gl.aux for an example).  This file can have any name and can be put anywhere as long as you supply it to the -auxiliary_data parameter when running IgBLAST(for example -auxiliary_data my_foler/my_file).

To run IgBLAST for your organism, please make sure you specify the -organism my_organsim parameter.  
