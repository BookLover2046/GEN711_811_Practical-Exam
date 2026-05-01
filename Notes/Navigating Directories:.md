Navigating Directories:
•	pwd: print working directory 
o	shows which directory you are working in 
•	cd: use followed by a file name to start working in that directory 
o	cd../: transfer to previous directory 
	cd../../../
o	cd $Home or cd ~: brings you to home directory 
Files:
•	cp: copy a pre-existing file into a specified file that is listed after the command 
o	to create backup file, add the original file name after cp and then type /backup followed by file name 
•	mkdir: create a new folder in a directory 
•	grep ‘’: searches for lines in a file with a specific character or term specified by the user 
o	Ex. grep ‘@’ FILE.NAME
•	grep -v: inverse grep that excludes all reads that contain a specified sequence
•	head: prints first several lines of a file into the terminal 
o	head n-4: displays first four lines of a specified file 
•	tail: prints last several lines of a file into the terminal
•	cat: prints entire files into the terminal
•	mv: move file into a different directory 
o	Can be used to change file name
	Ex. mv old_filename new_filename 
•	less: allows user to scroll through the entire file that is entered afterwards 
•	history: displays history of previous commands that were run
•	wc: word count
o	wc -l: number of lines in a file 
o	wc -w: number of words in a file 
o	wc -c: number of characters in a file  
•	-B1: print one line before a line with a series of specified characters in a file
o	Ex. grep -B1 NNNNN SRR098026.fastq
•	-A1: print one line after a line with a series of specified characters in a file

Converting Files from FASTQ to FASTA:
•	Print first lines of FASTQ in FASTA format
o	grep -A1 -h --no-group-separator ‘@SRR’ SRR098026.fastq | head 
•	Convert FASTQ to FASTA
o	grep -A1 -h --no-group-separator '@SRR' SRR098026.fastq > SRR098026.fasta
•	grep -A1 –no-group-separator ‘^+’ *.fastq | grep ‘@’ | wc -l
o	output is a single number that indicates have many quality score lines have an ‘@’ symbol 
o	can be reused to 
•	which fastqc 
o	tells you where fastqc is installed 
o	need to be in respective conda environment 

File Controls and Lines:
•	ls: list all files in a directory 
o	ls * ‘’: lists files in a directory that contain specified characters 
o	ls -F: lists options in a folder 
o	ls -lrth: provides more information about the directories and who owns it 
	Lists when directory was made 
	Human readable format 
o	ls -lh 
	human readable format 
o	ls /bin/c* | wc -l: counts the number of lines in the binaries folder that contains the letter c 
o	ls –all: displays all folders/directories including those that are hidden 
o	ls -- all .hidden/: use to open hidden files 
o	ls -a | grep “^\\.”: displays only the hidden file 
o	ls -d .*: display hidden file only 
o	ls -laF: lists all files that require additional permission to edit 
•	/bin: command that lists all binaries 
•	chmod -w: take away write privileges for a file
Conda Environments:
•	conda activate genomics: activates genomics conda environment 
•	conda activate trimmomatic-0.39: activate trimmomatic conda environment 
•	conda info --envs: lists all conda environments and where they are located 

Trimmomatic:
•	trimmomatic PE -threads 4 SRR_1056_1.fastq SRR_1056_2.fastq  \
            SRR_1056_1.trimmed.fastq SRR_1056_1un.trimmed.fastq \
            SRR_1056_2.trimmed.fastq SRR_1056_2un.trimmed.fastq \
            ILLUMINACLIP:SRR_adapters.fa SLIDINGWINDOW:4:20
o	PE: this will be take a paired-end file as an input 
o	-threads 4: to use four computing threads to run (this will speed up the run)
o	SRR_1056_1.fastq: first input file name 
o	SRR_1056_2.fastq: second input file name
o	SRR_1056_1.trimmed.fastq: the output file for surviving pairs from the `_1` file
o	SRR_1056_1un.trimmed.fastq: the output file for orphaned reads from the `_1` file
o	SRR_1056_2.trimmed.fastq: the output file for surviving pairs from the `_2` file
o	SRR_1056_2un.trimmed.fastq: the output file for orphaned reads from the `_2` file
o	ILLUMINACLIP:SRR_adapters.fa: to clip the Illumina adapters from the input file using the adapter sequences listed in ‘SRR_adapters.fa’
o	SLIDINGWINDOW:4:20: to use a sliding window of size 4 that will remove bases if their phred score is below 20
