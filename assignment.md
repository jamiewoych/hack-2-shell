# 1. Get the data files

Download the following data files from the internet using the curl command: http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. Use the less or zless commands to look at each file. Describe what these commands do. Finally, use the head command to print the first 5 lines of the file iris-data-dirty.csv.

First I looked up information about the curl command, and found that you can use the command to download files from a url with -O option to save it with the same name.
When attempting to read the files with the less command, I found that the files weren't downloading properly, since they were empty. After some searching, I found I needed to add the -L option to allow redirects from the url.
Then I used the man command to look up the documentation for the less and zless command. The less command allows you to read files easily, while the zless command allows you to read zipped files. 
The head command returns the first 10 lines by default, so I used the -n option to specify the number of lines to return. 
```
curl -O -L http://eaton-lab.org/pdsb/test.fastq.gz 
curl -O -L http://eaton-lab.org/pdsb/iris-data-dirty.csv 
zless test.fastq.gz
less iris-data-dirty.csv
head -n 5 iris-data-dirty.csv
5.1,3.5,1.4,0.2,Iris-setosa
4.9,3.0,1.4,0.2,Iris-setosa
4.7,3.2,1.3,0.2,Iris-setosa
4.6,3.1,1.5,0.2,Iris-setosa
5.0,3.6,1.4,0.2,Iris-setosa
```

# 2. Clean the data
Use grep, uniq, and sed for this question. Check that all of the species names are spelled correctly in the file iris-data-dirty.csv. Also check for missing values stored as NA. Create a new file where mispelled names are replaced with the correct values, and lines with NA are excluded, and save it as iris-data-clean.csv. Use cut, sort and uniq to list the number of data values there are for each species in the new cleaned data file. Describe your work.

I read the file with the less function again to check if the species names were spelled correctly. I saw two instances where the species names were spelled 'setsa' and 'versicolour'. I looked up the documentation for the grep, uniq, and sed commands. I listed all the lines with alterations necessary using the grep command. In order to replace the spelling errors, I used the sed command to substitute the errors in the document with the correct spelling. I also used the sed command to delete any instances when 'NA' was present in the file, using the -i option to to save in place each time I made alterations. I then used the cp command to copy the data to a new file with the proper name. 
Then I used the cut, sort and uniq commands to cut the lines by the fifth field, delineated by a comma. This isolated the species names, which I then sorted, and listed the number of repetitions of each species name

```
grep 'setsa' iris-data-dirty.csv
grep 'versicolour' iris-data-dirty.csv
grep 'NA' iris-data-dirty.csv
sed -i "" 's/versicolour/versicolor/g' iris-data-dirty.csv
sed -i "" 's/setsa/setosa/g' iris-data-dirty.csv
sed -i "" '/NA/d' iris-data-dirty.csv
cp iris-data-dirty.csv iris-data-clean.csv
cut -d',' -f5 iris-data-clean.csv | sort | uniq -c
   1 
  50 Iris-setosa
  48 Iris-versicolor
  50 Iris-virginica

```
# 3. Find a sequence

Find how many lines in the data file test.fastq.gz start with "TGCAG" and end with "GAG". Describe your work.

Since the file is compressed, I had to unzip the file with gunzip with standard output instead of writing a new file using -c option. 
Then I used the grep command to search for patterns where the line started with "TGCAG" denoted by ^, and ended with "GAG" denoted by $, separated by any characters denoted by .*
I then used the word count command to list the number of lines this is true for

```
gunzip -c test.fastq.gz | grep -E "^TGCAG.*GAG$" | wc -l
44
```

# 4. Find a sequence chunk
Using grep and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.


I decompressed the file again with gunzip, and then used grep to search for lines that include the sequence "AAAACCCC", with options -B (before) and -A (after) to also print the line before and 2 lines after each sequence hit

```
gunzip -c test.fastq.gz | grep -B 1 -A 2 "AAAACCCC"

@32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
+32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
IIIIIIIIIIIIIIIIIIHIHIIIIIIIIGIIIGIIIIIIHIIIIIIIHIIIIHIIIIIIEHIHHIIIIICIHI
--
@33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC
+33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
IIIIIIIIIIIIIIIIIIIIDHIIHHIIIIIEIBGBGGGIIHEHHHIEBBHHIEGGDGIGGHAEFDBFBDDB?D
```

