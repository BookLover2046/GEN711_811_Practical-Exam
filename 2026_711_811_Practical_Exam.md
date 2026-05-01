

Exam Instructions:
- Make sure to paste all the commands that you use below each of the tasks.
- Some tasks require you to copy your output here. All output that you paste is less than 10 lines.
- Do not spend too much time on a single prompt. It's important to get to all prompts.
- If you get stuck and do not know how to proceed: specifically ask for the answer.
- Help will get you to the next step, but you will lose points for that prompt.
- Questions on clarification of the question does not result in the loss of points.
- Replace MYNAME with your first and last name.
- Your exam must be pushed to your github before 5pm to get credit. You can push early, and push again at the end.

Hints:
- Remember, if you want to re-run a command, but change it slightly to try to fix it, hit the up arrow and edit the last command that you ran.
- When typing out commands, filenames, or paths, hit the tab button a couple of times to see if it autocompletes for you.
- The two paired-end fastq files for this exam live in the `Data/` directory of the repo you cloned:
  - `WELMw0518231_S614_L002_R1_001.fastq.gz`
  - `WELMw0518231_S614_L002_R2_001.fastq.gz`

---

1. Use an absolute path to change your current working directory to the `GEN711_811_Practical-Exam` directory that you just cloned (2 points).

cd /home/users/hjm1079/GEN711_811_Practical-Exam

2. From `GEN711_811_Practical-Exam`, make the following directory structure in a single command: `analysis/untrimmed_fastq` (2 points, -1 point if you need to use 2 commands for this. Hint: There is a flag/option that lets you create nested directories all at once.)

mkdir analysis | mkdir untrimmed_fastq | mv untrimmed_fastq analysis

3. Copy the two fastq files in the `Data/` directory directly into your `analysis/untrimmed_fastq` directory **without changing your current directory**. (2 points, partial credit if you need to change directories first. Multiple correct answers)

cp Data/WELMw0518231_S614_L002_R1_001.fastq.gz analysis/untrimmed_fastq/
cp Data/WELMw0518231_S614_L002_R2_001.fastq.gz analysis/untrimmed_fastq/

4. List all the hidden files in this repo. Paste the command and the output below (2 points)

ls -a | grep "^\\."
.
..
.git
.gitignore

5. Use a relative path to change your current working directory to the `analysis/untrimmed_fastq` directory. (2 points)

cd analysis/untrimmed_fastq/

6. These are paired-end Illumina 16S rRNA amplicon FASTQ files. To confirm the files look ok, view one of them and paste the command and top 4 lines below. (4 points, Hint: These files are gzip-compressed. Multiple correct answers)

gunzip WELMw0518231_S614_L002_R1_001.fastq.gz
head -n4 WELMw0518231_S614_L002_R1_001.fastq 
@A01346:131:HTHT7DRX3:2:2101:10917:1047 1:N:0:AACACCAC+GCTGTAAG
GTACACACCGCCCGTCGCTCCTACCGCTTGAGTGATCCGGTGAATAATTTGGACTGCTGCAATGCTCAGTTACTGACATTGTGGTGGGAAATCTAGGGAACCTTATCACTTAGTGGAAGGAGACGGCGTAACAATGTTTCCGTAGGTGAACCTGCAGAAGTATCACTGTCTGTTATACACATCTCCGAGACCACGAGACAACACAACAACGCGTCTGCCCTGTGATTCTTGAAGAT
+
FFF,,FFFF::FFFF:FFFFFFFFFF,F,,F:::F,,F,FF,,:FFFFF:,FFFFFF,,:F,,:,:,F,FFF,FFF:,,FF::,FFF::FFFFFF,,FFF,FFFFFFFFF,F,,FFF:FFF:,,,,,,F:FF::,:FFFF,:,FF:FFFFFFFFFFFF::,,F,:FFFF,,,F,F,:F:FF,FFF,F:,,FFFFFF:,,F,,FF,,F,,:,,F:,FF,,,F,,,,F,:,,F,:,::

7. How large (file size) are the two fastq files? Use a single command with appropriate options to show the file sizes in a human-readable format (e.g., MB). Paste the command and output below. (2 points)

ls -lrth WELMw0518231_S614_L002_R1_001.fastq 
-rw-r--r--. 1 hjm1079 domain users 89M May  1 15:42 WELMw0518231_S614_L002_R1_001.fastq

89 MB

ls -lh WELMw0518231_S614_L002_R2_001.fastq 
-rw-r--r--. 1 hjm1079 domain users 90M May  1 15:42 WELMw0518231_S614_L002_R2_001.fastq

90 MB

8. For each fastq, how many quality score lines contain the `@` symbol? To answer this, use one line of piped bash commands for each fastq, and the output should be a single number per file. (2 points)

grep '@' WELMw0518231_S614_L002_R1_001.fastq | grep -v '@A01346' | wc -l
0
grep '@' WELMw0518231_S614_L002_R2_001.fastq | grep -v '@A01346' | wc -l
0

9. How many reads in `WELMw0518231_S614_L002_R1_001.fastq.gz` have **1 or more uncalled bases (`N`)** in the sequence? Count WITHOUT making a new file. (4 points)
Hint: grep, less, and cat all have versions that work on compressed files: zgrep, zless, and zcat
Hint: Do quality score lines contain 'N's?

zgrep 'N' WELMw0518231_S614_L002_R1_001.fastq.qz | grep -v '@A01346' | wc -l 
145

zgrep 'N' WELMw0518231_S614_L002_R2_001.fastq.gz | grep -v '@A01346' | wc -l 
10

10. Make a single **fasta** file from the two **fastqs** using only the reads identified in question 9 (reads containing at least one `N`) and their respective info lines. Name the new file `badreads.fasta` in the `untrimmed_fastq` directory. (4 points)

Hint: remember the difference between FASTA and FASTQ when making the new file

grep '@' WELMw0518231_S614_L002_R1_001.fastq | grep -v '@A01346' > WELMw0518231_S614_L002_R1_001.fasta
grep '@' WELMw0518231_S614_L002_R2_001.fastq | grep -v '@A01346' > WELMw0518231_S614_L002_R2_001.fasta
mkdir badreads.fasta
mv WELMw0518231_S614_L002_R1_001.fasta WELMw0518231_S614_L002_R2_001.fasta badreads.fasta

11. Activate the conda `genomics` environment that contains `fastqc` and confirm where `fastqc` is installed. Paste the command and its output below. (2 points)

conda activate genomics
which fastqc
/home/share/anaconda/envs/genomics/bin/fastqc

12. Run `fastqc` on `WELMw0518231_S614_L002_R1_001.fastq.gz`. Then, create a `results/fastqc_untrimmed_reads` directory and move both the `.zip` and `.html` output files into it — all without leaving your `untrimmed_fastq` directory. Paste all commands used. (4 points)

mkdir results/
mkdir fastqc_untrimmed_reads/
mv fastqc_untrimmed_reads/ results/
mv WELMw0518231_S614_L002_R1_001_fastqc.html WELMw0518231_S614_L002_R1_001_fastqc.zip results/fastqc_untrimmed_reads/

13. Without changing directories, what is the **100th line** of the file `WELMw0518231_S614_L002_R1_001.trim.fastq.gz` in your `untrimmed_fastq` directory? (2 points)
Hint: grep, less, and cat all have versions that work on compressed files: zgrep, zless, and zcat

head -n100 WELMw0518231_S614_L002_R1_001.fastq
FFFFFFFFFFF:FFFFFFFFFFFFFFFFFFFFFFFF::FF:FFFFFFFFFFFFFFFFFF:FFFFF:FFFFFF,FFF:FFFFFFFFFFFFFFFFFFFFFFF::FFFF:FFFFFFFFFFFFFFF:F,FFFFFFFFFFFFFFF:FFFF:FFFFFFFF:F:F::FF,,F:FFFFFFFFFFFFFFF,FFF,F::,F:FFFFF,FFFFFF,FFF,F,F:F,FF,FFF,,,,FF,,FF,,,,

14. Run `md5sum` on `WELMw0518231_S614_L002_R1_001.fastq.gz`. Then run it again, redirecting the output to a new file called `my_md5sums.txt`. Next, run `md5sum` on `WELMw0518231_S614_L002_R2_001.fastq.gz` and **append** it to `my_md5sums.txt`. Finally, append your name to the end of `my_md5sums.txt`. Paste all commands used. (4 points)

 md5sum WELMw0518231_S614_L002_R1_001.fastq 
  md5sum WELMw0518231_S614_L002_R1_001.fastq > my_md5sums.txt
  md5sum WELMw0518231_S614_L002_R2_001.fastq >> my_md5sums.txt
mv my_md5sums.txt Hannah_md5sums.txt

15. Push all of the new files that you created to your github repo using vscode's github side bar. (4 points)
---
