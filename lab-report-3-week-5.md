# Week 5 Lab Report: Command-line Options

For this week's lab, we will be exploring the various options for the command `grep`.

The command `grep` will take a *pattern* (a string of characters) and a *file* and will search for and output any lines within the *file* that have a match for the *pattern*.

## `grep` Command-line Options

Three examples of command-line options of `grep` that I found to be potentially useful were `-c`, `-l`, and `-n`.

* `-c`: outputs count of matching lines instead of the lines themselves
* `-l`: instead of outputting the lines that the pattern is found in, it will output the file that the line(s) are contained in
* `-n`: gives a line number as well as a matching line

---

## `grep -c`

`-c`: outputs count of matching lines instead of the lines themselves

Examples:

## 1. Counting appearances of pattern "neuron" in lines of a specific file
```
$ grep -c "neuron" biomed/1471-2202-2-1.txt
23
```
Here, we used `grep -c` to return a count of the appearances of pattern "neuron" in the lines of technical/biomed/1471-2202-2-1.txt. This could be useful if we wanted to find the frequency of usage of a word, or even a phrase.

## 2. Counting appearances of pattern "while" in lines of a specific file
```
$ grep -c "while" biomed/1471-2202-2-1.txt
2 
```
Here, we used `grep -c` to return a count of the appearances of pattern "while" in the lines of technical/biomed/1471-2202-2-1.txt. This could potentially be useful if we used this in a program file in which we would like to know how many while loops there are.

## 3. Counting appearances of pattern "[" in lines of a specific file
```
grep -c "\[" biomed/1471-2202-2-1.txt
64
```
Here, we used `grep -c` to return a count of the appearances of pattern "[" in the lines of technical/biomed/1471-2202-2-1.txt. I believe that the square braces are used to indicate a footnote or reference point or something of the sort, so in the context of the file, it is useful in that it tells us how many times we are making use of the square brace reference points.

---

## `grep -l`

`-l`: outputs the file name that the valid line(s) are contained within, instead of outputting the lines themselves

Examples:

## 1. Searching for files in which pattern "Pentagon" is found
```
$ grep -l "Pentagon" 911report/*.txt
911report/chapter-1.txt
911report/chapter-10.txt
911report/chapter-11.txt
911report/chapter-12.txt
911report/chapter-13.1.txt
911report/chapter-13.2.txt
911report/chapter-13.4.txt
911report/chapter-13.5.txt
911report/chapter-3.txt
911report/chapter-5.txt
911report/chapter-6.txt
911report/chapter-7.txt
911report/chapter-9.txt
```
Here, we used `grep -l` to return all text files within technical/911report that contained the pattern "Pentagon". This is useful since if we were looking for any text files containing information about the Pentagon, we have reduced our search size to less files than we started with.

## 2. Searching for files in which pattern "class" is found
```
grep -l "class" government/*/*.txt
government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
... (lots of files!)
government/Post_Rate_Comm/Mitchell_spyros-first-class.txt
government/Post_Rate_Comm/Redacted_Study.txt
government/Post_Rate_Comm/ReportToCongress2002WEB.txt
```
Here, we used `grep -l` to return all text files within technical/government that contained the pattern "class". This could potentially prove to be useful as if we were to use this on a directory full of java files, we could potentially find files in which classes are created.

## 3. Searching for files in which pattern "abstract" is found
```
$ grep -l "abstract" plos/*.txt
plos/journal.pbio.0020228.txt
plos/journal.pbio.0020297.txt
plos/journal.pbio.0020337.txt
plos/journal.pbio.0020350.txt
plos/journal.pbio.0020394.txt
plos/journal.pbio.0020439.txt
plos/journal.pbio.0030105.txt
plos/pmed.0010052.txt
```
Here, we used `grep -l` to return all text files within technical/plos that contained the pattern "abstract". This could potentially be used to search for research papers, and could be cross referenced with other search results for other common research paper keywords. 

---

## `grep -n`

`-n`: gives a line number as well as a matching line

Examples:

## 1. Searching for lines (with line numbers) within a specific file in which pattern "photosynthesis" is used.
```
$ grep -n "photosynthesis" biomed/gb-2002-3-9-research0045.txt
406:          photosynthesis genes encoded in the nuclear genome. Thus,
657:          Two types of C4 photosynthesis pathways in
670:          decarboxylation of OAA in PEPCK-type C4 photosynthesis [
679:          photosynthesis in maize. Maize leaves have PEPCK activity
```
Here, we used `grep -n` to return all lines and their line numbers in which pattern "photosynthesis" is found within the file technical/biomed/gb-2002-3-9-research0045.txt. This is useful as if I needed to access this database and in particular it's findings surrounding something like "photosynthesis", I could do so and be shown where exactly in the file they appear.

## 2. Searching for lines (with line numbers) within a specific file in which pattern "switch" is used.
```
$ grep -n "switch" biomed/gb-2002-3-7-research0035.txt
574:        then switch their function from cross-tolerization to
844:          template-switching PCR. The first round combined reverse
847:          and template-switch primer
859:          the template-switch primer. aRNA (3.5-5 μg) obtained
```
Here, we used `grep -n` to return all lines and their line numbers in which pattern "switch" is found within the file technical/biomed/gb-2002-3-7-research0035.txt. This could be useful if we were to use this in a C++ file and were looking for anywhere where a switch structure is used.

## 3. Searching for lines (with line numbers) within a specific file in which pattern "summary" is used.
```
$ grep -n "summary" biomed/rr191.txt
492:        In summary, our studies are suggestive that in lung
```
Here, we used `grep -n` to return all lines and their line numbers in which pattern "summary" is found within the file technical/biomed/rr191.txt. This could be useful if we just wanted to find a quick summary of the findings of a paper or research.