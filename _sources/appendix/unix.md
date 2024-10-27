# Chapter A1.1: Unix cheat sheet

## Commonly used command line tools

* `cd <destination>`: Navigate to a directory. Examples:
  * `cd /home/mgymrek/gwas/` navigate to this directory
  * `cd` If no destination specified, go to the home directory
  * `cd ~/gwas` ~ is a shorthand for the home directory
* `pwd` or `PWD`: print the path of the current working directory
* `ls`: list the contents of the current directory. Additional helpful options:
  * `-l` lists contents in "long" format with info about file size, owner of the contents, time last modified, etc.
  * `-lh` lists things in "human readable" format. e.g. uses sufixes to denote kilobytes (K), megabytes (M), etc. rather than listing sizes in bytes.
  * `-t` sort by time modified.
  * `-r` reverse the order of the sort
  * These can be combined e.g. with `ls -ltrh`. Many other options to `ls` exist but the ones above are very common.
* `head <file>`: print the first several lines of a file to the terminal
  * `head -n 20 <file>`: print the first 20 lines
* `less -S`: allows you to scroll through a file either vertically or horizontally on the terminal
* `mkdir <destination>`: make a new directory. Additional helpful option:
  * `mkdir -p /path/to/folder`: Creates intermediate directories (e.g. `/path`, `/path/to`) if required
* `cp <src> <dest>`: Copy contents from one path to another. Additional helpful option:
  * `cp -r <src> <dest>`: Recursive copy, which copies all subdirectories of `<src>`.
* `mv <src> <dest>`: Move contents from one path to another
* `wc -l <file>`: count the number of lines in a file
* `cat <file>`: print the contents of a file 
  * `cat <file1> <file2> <file3>`: concatenates the contents of multiple files
* `zcat`: similar to `cat` but works on zipped files. e.g.
  * `zcat myfile.vcf.gz`: will print contents of the gzipped VCF file
  * Note on Mac you need to do: `zcat < myfile.vcf.gz`
* `bgzip <file>`: Bgzip a file, Additional helpful option:
  * `cat myfile.vcf | bgzip -c > myfile.vcf.gz`: the `-c` option writes contents to stdout.
* `grep <searchpattern> <file>`: Searches contents of a file
  * `grep mymatch myfile.txt` prints lines of `myfile.txt` that contain `mymatch`.
  * `grep -n mymatch myfile.txt` also print the line number where each match was found
  * `grep -w chr2 myfile.vcf` the `-w` option requires *word* matches. In this example, chr22 and chr2 would both match chr2, but the `-w` option only returns lines with `chr2`.
  * `grep -v chr2 myfile.vcf` the `-v` option returns lines that *do not* match the search string
  * `grep chr2 *`: search for chr2 in all files in the current directory
* `awk` is actually an entire scripting language but can be used for many one-liner queries. Examples:
  * `awk '($2=="yes")' myfile.txt`: return lines of `myfile.txt` for which column 2 is equal to `yes`
  * `awk -F"," '($2=="yes")' myfile.txt`: same as above but specify that columns are delimited by columns
  * `awk '{print $2 "\t" $3}' myfile.txt`: output the second and third columns of `myfile.txt`, separated by a tab
  * `awk '($2=="1") {print $4} myfile.txt'`: output the fourth column of `myfile.txt`, but only for lines where the second column is equal to 1.
  * You can also pipe files as input to awk. e.g. `cat myfile.txt | awk -F"," '($2=="yes")'`

## Command line operators

* `>`: direct contents of stdout to a file. e.g.:
  * `head -n 10 myfile.txt > myfile_top10.txt`
* `2>`: direct contents of stderr to a file. e.g.:
  * `./myscript.sh 2> err.log`
* `|`: Use stdout of one command as stdin to another. e.g.:
  * `cat myfile.txt | head -n 10 > myfile_top10.txt`  
* You can string together multi-line commands with `\`. e.g.:

```
cat myfile.txt | \
  head -n 20 > myfile_top20.txt
```