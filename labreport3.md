# Part 1 - Bugs
## The Original Code
```
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }
```
## Failure-Inducing Input
```
    @Test
    public void testMerge(){
        List<String> list1 = new ArrayList<String>(); 
        List<String> list2 = new ArrayList<String>();
        list1.add("a");
        list1.add("c");
        list2.add("b");
        list2.add("d");
        List mergedList = ListExamples.merge(list1, list2);
        assertEquals("a", mergedList.get(0));
    }
```
![Image](lr3buggyoutput.png)
## Successful Input
```
    @Test
    public void testMergeSucceed(){
        List<String> list1 = new ArrayList<String>(); 
        List<String> list2 = new ArrayList<String>();
        list1.add("a");
        list1.add("c");
        list2.add("b");
        List mergedList = ListExamples.merge(list1, list2);
        assertEquals("b", mergedList.get(1));
    }
```
![Image](lr3successoutput.png)
## Changes
```
// This is the buggy section before changes
while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
```
```
// This is the buggy section after changes
while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index2 += 1; // variable updated is now index2 instead of index1
    }
```
This change fixes the issue because before, `index2` would always be less than `list2.size()`, and therefore the `while` loop would never end. With the changes, `index2` is now being updated properly, and the `while` loop now has a way to end.

# Part 2 - Researching Commands
I chose to research the `grep` command.
For all examples, I only used [this](https://man7.org/linux/man-pages/man1/grep.1.html) page.
## `-e`
**Example One**
```
// the commands
find technical/biomed > lr-files.txt
grep -e "1468" lr-files.txt > e-results-one.txt
```
```
// the final output
technical/biomed/1468-6708-3-10.txt
technical/biomed/1468-6708-3-4.txt
technical/biomed/1468-6708-3-7.txt
technical/biomed/1468-6708-3-3.txt
technical/biomed/1468-6708-3-1.txt

```

In this block of commands, the system first searches for all of the files in the `biomed` directory, then it finds all of the files that have `"1468"` in the title and puts them in the `e-results-one.txt` file. This kind of usage might be useful if you are searching for files in a directory that is sorted by date, and you are looking for files from a specific date.

**Example Two**
```
// the commands
cat technical/biomed/gb-2002-4-1-r1.txt > cat-one.txt
grep -e "genome" cat-one.txt > e-results-two.txt
```
```
// the final output
        In May 2002, two new mouse genome assemblies were
        released. One was the second version of the mouse genome
        MGSCv3 [ 2]). Both these draft mouse genomes were obtained
        using a whole-genome shotgun (WGS) strategy, but using
        220,000 contigs and covers 2.475 Gb of the mouse genome. By
        gaps in these chromosomes occupy 4.1% of genome in Cel2 and
        scaffolds/supercontigs of both the genome assemblies are
          it is highly recommended that the two mouse genome
          Comparison between the human genome golden path and
          The human genome project is well into the finishing
          mouse and human genomes. Comparing the human genome with
          the mouse genome can greatly help our understanding of
          both genomes. We used the BLASTN program [ 6] to compare
          the December 2001 golden path freeze of the human genome,
          7]), to mask the repeats in all the three genomes. With a
          MGSCv3 genome assemblies and 1,737,297 CSEs between the
          human and Cel2. Each CSE includes one human genome
          segment and one matched mouse genome segment. The
          covered in the human or mouse genomes in the following
          human genome than CSEs from MGSCv3 assembly. Although the
          could be identified in the human genome from MGSCv3
          identity between the human and mouse genomes. Very short
          mouse genomes than long CSEs because they are more likely
          We compared the human genome regions covered by CSEs
          (Mb) (approximately 3% of the whole human genome) are
          than MGSCv3 in the whole mouse genome.
          inconsistency of CSE locations in the human genome from
            locations of CSEs in the human genome were compared
            the CSE in the human genome is in an intron region
            rat genome that matches the human location
            whole human genome, we found 6,259 NCBI-annotated novel
            RNA genes (from golden path April 2001 human genome
            genome location of this CSE, we discovered the
            genome hit multiple times in the other genome. We used
            duplications in the human genome relative to the mouse
            genome. The human and mouse regions in some of these
            type of CSEs are located in the genomes with the same
            collect a pair of human genome segments and one mouse
            genome segment. With length constraint of these
            segments against the newest human genome assembly (NCBI
            5. As the known repeats in both genomes were masked
            in the human genome may point to either true expansion
            and mouse genomes.
            functional regions in the genome, more than half of all
            accessed through the genome browser link at [ 18].
          Cel2 and MGSCv3 mouse genome assemblies were finished on
          CSE to search the whole human genome with BLAT. If this
          same as the percentage in the whole genome. The second
          approximately the same as in the whole genome. Under
        of one chromosome in the whole genome are available as
        of one chromosome in the whole genome

```
In this block of commands, the system concatenates the contents of a file (in this case `technical/biomed/gb-2002-4-1-r1.txt`) into a new file, then it looks for all instances of the word `"genome"` in the file. This may be useful if you are looking for specific information in a large text file.

## `-i`
**Example One**
```
// the command
grep -e "genome" cat-one.txt -i > i-results-one.txt
```
```
// the final output
        In May 2002, two new mouse genome assemblies were
        released. One was the second version of the mouse genome
        from the public Mouse Genome Sequencing Consortium (denoted
        MGSCv3 [ 2]). Both these draft mouse genomes were obtained
        using a whole-genome shotgun (WGS) strategy, but using
        220,000 contigs and covers 2.475 Gb of the mouse genome. By
        gaps in these chromosomes occupy 4.1% of genome in Cel2 and
        scaffolds/supercontigs of both the genome assemblies are
          it is highly recommended that the two mouse genome
          Comparison between the human genome golden path and
          The human genome project is well into the finishing
          mouse and human genomes. Comparing the human genome with
          the mouse genome can greatly help our understanding of
          both genomes. We used the BLASTN program [ 6] to compare
          the December 2001 golden path freeze of the human genome,
          7]), to mask the repeats in all the three genomes. With a
          MGSCv3 genome assemblies and 1,737,297 CSEs between the
          human and Cel2. Each CSE includes one human genome
          segment and one matched mouse genome segment. The
          covered in the human or mouse genomes in the following
          human genome than CSEs from MGSCv3 assembly. Although the
          could be identified in the human genome from MGSCv3
          identity between the human and mouse genomes. Very short
          mouse genomes than long CSEs because they are more likely
          We compared the human genome regions covered by CSEs
          (Mb) (approximately 3% of the whole human genome) are
          than MGSCv3 in the whole mouse genome.
          inconsistency of CSE locations in the human genome from
            locations of CSEs in the human genome were compared
            the CSE in the human genome is in an intron region
            rat genome that matches the human location
            novel gene is predicted by GenomeScan [ 14] in NCBI
            whole human genome, we found 6,259 NCBI-annotated novel
            RNA genes (from golden path April 2001 human genome
            genome location of this CSE, we discovered the
            genome hit multiple times in the other genome. We used
            duplications in the human genome relative to the mouse
            genome. The human and mouse regions in some of these
            type of CSEs are located in the genomes with the same
            collect a pair of human genome segments and one mouse
            genome segment. With length constraint of these
            segments against the newest human genome assembly (NCBI
            5. As the known repeats in both genomes were masked
            in the human genome may point to either true expansion
            and mouse genomes.
            functional regions in the genome, more than half of all
            accessed through the genome browser link at [ 18].
          Cel2 and MGSCv3 mouse genome assemblies were finished on
          CSE to search the whole human genome with BLAT. If this
          same as the percentage in the whole genome. The second
          approximately the same as in the whole genome. Under
        of one chromosome in the whole genome are available as
        of one chromosome in the whole genome

```

With this command option, the system looks for all instances of the word `"genome"` in the `cat-one.txt` file without case-sensitivity. This is useful for finding all instances of a certain word in a file that may have varied capitalization of that word.

**Example Two**
```
// the command
grep -e "BioMed" lr-files.txt -i > i-results-two.txt
```
```
// the final output
technical/biomed
technical/biomed/1472-6807-2-2.txt
technical/biomed/1471-2350-4-3.txt
technical/biomed/1471-2156-2-3.txt
technical/biomed/1471-2156-3-11.txt
technical/biomed/1471-2121-3-10.txt
technical/biomed/1471-2172-3-4.txt
technical/biomed/gb-2002-4-1-r2.txt
technical/biomed/gb-2003-4-6-r41.txt
technical/biomed/1471-2466-1-1.txt
technical/biomed/1471-2199-2-10.txt
technical/biomed/1471-2202-2-9.txt
technical/biomed/cc991.txt
technical/biomed/1471-2369-3-9.txt
technical/biomed/bcr620.txt
technical/biomed/1476-069X-2-4.txt
technical/biomed/1472-6750-3-11.txt
technical/biomed/1471-2164-2-9.txt
technical/biomed/1471-2091-2-10.txt
technical/biomed/gb-2001-2-4-research0010.txt
technical/biomed/gb-2003-4-4-r24.txt
technical/biomed/1471-213X-2-1.txt
technical/biomed/1472-6882-3-3.txt
technical/biomed/1471-2407-2-3.txt
technical/biomed/ar331.txt
technical/biomed/ar319.txt
technical/biomed/1471-2156-4-5.txt
technical/biomed/1471-2431-2-1.txt
technical/biomed/1476-4598-2-22.txt
technical/biomed/1471-2180-2-22.txt
technical/biomed/1471-2334-3-9.txt
technical/biomed/1471-2091-2-11.txt
technical/biomed/gb-2001-2-4-research0011.txt
technical/biomed/1471-2202-4-12.txt
technical/biomed/rr73.txt
technical/biomed/1471-2164-2-8.txt
technical/biomed/1471-2148-2-12.txt
technical/biomed/bcr635.txt
technical/biomed/1468-6708-3-10.txt
technical/biomed/gb-2003-4-5-r34.txt
technical/biomed/1471-2202-2-8.txt
technical/biomed/1471-2121-3-11.txt
technical/biomed/1471-2156-3-10.txt
technical/biomed/1471-2458-3-20.txt
technical/biomed/1471-2350-4-2.txt
technical/biomed/1472-6807-2-3.txt
technical/biomed/1472-6807-2-1.txt
technical/biomed/1476-4598-1-8.txt
technical/biomed/1477-7525-1-9.txt
technical/biomed/ar79.txt
technical/biomed/1476-0711-2-7.txt
technical/biomed/1472-6947-3-8.txt
technical/biomed/1471-2121-3-13.txt
technical/biomed/gb-2002-4-1-r1.txt
technical/biomed/1471-2407-3-18.txt
technical/biomed/1471-2229-2-3.txt
technical/biomed/1471-2334-1-9.txt
technical/biomed/gb-2002-3-9-research0043.txt
technical/biomed/1471-2415-3-5.txt
technical/biomed/1471-2334-1-21.txt
technical/biomed/gb-2001-2-7-research0025.txt
technical/biomed/ar130.txt
technical/biomed/1476-069X-2-7.txt
technical/biomed/1472-6890-2-5.txt
technical/biomed/ar118.txt
technical/biomed/gb-2002-3-7-research0032.txt
technical/biomed/1471-2253-2-5.txt
technical/biomed/1471-2210-1-10.txt
technical/biomed/1471-2091-2-13.txt
technical/biomed/1471-2180-2-20.txt
technical/biomed/1471-2202-3-19.txt
technical/biomed/1471-2202-4-10.txt
technical/biomed/1472-6963-2-10.txt
technical/biomed/1476-4598-2-20.txt
technical/biomed/1471-2156-4-6.txt
technical/biomed/1471-2458-3-5.txt
technical/biomed/1472-6769-1-4.txt
technical/biomed/gb-2003-4-4-r26.txt
technical/biomed/1472-6882-3-1.txt
technical/biomed/cc4.txt
technical/biomed/1471-2180-2-35.txt
technical/biomed/1471-2202-4-11.txt
technical/biomed/gb-2001-2-4-research0012.txt
technical/biomed/1471-2091-2-12.txt
technical/biomed/1471-2253-2-4.txt
technical/biomed/gb-2001-2-7-research0024.txt
technical/biomed/1471-2415-3-4.txt
technical/biomed/1471-2199-2-12.txt
technical/biomed/1471-2121-3-12.txt
technical/biomed/1471-2148-1-8.txt
technical/biomed/gb-2001-2-3-research0008.txt
technical/biomed/1471-2156-2-1.txt
technical/biomed/1471-2466-3-1.txt
technical/biomed/bcr568.txt
technical/biomed/gb-2003-4-7-r46.txt
technical/biomed/1475-2875-2-14.txt
technical/biomed/1471-2288-2-4.txt
technical/biomed/1472-6785-1-3.txt
technical/biomed/ar93.txt
technical/biomed/1472-6831-2-2.txt
technical/biomed/bcr583.txt
technical/biomed/cc367.txt
technical/biomed/1477-7827-1-17.txt
technical/biomed/1477-7827-1-13.txt
technical/biomed/1472-6807-2-4.txt
technical/biomed/1471-2156-3-17.txt
technical/biomed/1475-2875-2-10.txt
technical/biomed/gb-2003-4-7-r42.txt
technical/biomed/1471-2156-2-5.txt
technical/biomed/ar68.txt
technical/biomed/1471-2172-3-2.txt
technical/biomed/1471-2121-3-16.txt
technical/biomed/1472-6750-1-12.txt
technical/biomed/1472-6904-2-7.txt
technical/biomed/1472-6882-1-7.txt
technical/biomed/1471-2334-1-24.txt
technical/biomed/1471-2377-2-4.txt
technical/biomed/gb-2002-3-9-research0046.txt
technical/biomed/rr74.txt
technical/biomed/gb-2002-3-7-research0037.txt
technical/biomed/gb-2001-2-8-research0027.txt
technical/biomed/1476-069X-2-2.txt
technical/biomed/1471-2148-2-15.txt
technical/biomed/1472-6874-2-1.txt
technical/biomed/1471-2210-1-8.txt
technical/biomed/1471-2091-3-8.txt
technical/biomed/1472-6793-2-8.txt
technical/biomed/1471-213X-2-7.txt
technical/biomed/1471-2202-3-20.txt
technical/biomed/1471-2091-2-16.txt
technical/biomed/1476-4598-2-25.txt
technical/biomed/1471-230X-2-21.txt
technical/biomed/1476-4598-2-24.txt
technical/biomed/1471-2350-2-2.txt
technical/biomed/gb-2001-2-11-research0046.txt
technical/biomed/1472-6769-1-1.txt
technical/biomed/ar120.txt
technical/biomed/1471-2148-2-14.txt
technical/biomed/1471-2407-1-19.txt
technical/biomed/gb-2001-2-8-research0032.txt
technical/biomed/gb-2002-3-7-research0036.txt
technical/biomed/1471-2415-3-1.txt
technical/biomed/gb-2003-4-5-r32.txt
technical/biomed/1472-6750-1-13.txt
technical/biomed/1472-6920-1-3.txt
technical/biomed/1471-2474-2-1.txt
technical/biomed/1476-0711-2-3.txt
technical/biomed/rr171.txt
technical/biomed/1471-2156-3-16.txt
technical/biomed/gb-2003-4-7-r43.txt
technical/biomed/1472-6807-2-5.txt
technical/biomed/1471-2350-4-4.txt
technical/biomed/1471-2172-1-1.txt
technical/biomed/ar297.txt
technical/biomed/1471-2350-4-6.txt
technical/biomed/rr167.txt
technical/biomed/1471-2474-2-3.txt
technical/biomed/1471-2121-3-15.txt
technical/biomed/1471-2172-3-1.txt
technical/biomed/1472-6904-2-4.txt
technical/biomed/1472-6750-1-11.txt
technical/biomed/gb-2003-4-5-r30.txt
technical/biomed/gb-2002-3-9-research0051.txt
technical/biomed/1471-2415-3-3.txt
technical/biomed/gb-2002-3-9-research0045.txt
technical/biomed/gb-2001-2-8-research0030.txt
technical/biomed/bcr631.txt
technical/biomed/1472-6769-1-3.txt
technical/biomed/cc3.txt
technical/biomed/1471-2180-2-32.txt
technical/biomed/1471-2202-4-16.txt
technical/biomed/1471-2180-2-26.txt
technical/biomed/1471-2431-2-4.txt
technical/biomed/1471-2458-3-2.txt
technical/biomed/1475-9276-1-3.txt
technical/biomed/ar321.txt
technical/biomed/1471-230X-2-23.txt
technical/biomed/ar309.txt
technical/biomed/gb-2001-2-4-research0014.txt
technical/biomed/1477-7819-1-10.txt
technical/biomed/gb-2001-2-11-research0045.txt
technical/biomed/1471-2202-4-17.txt
technical/biomed/1472-6769-1-2.txt
technical/biomed/bcr618.txt
technical/biomed/gb-2002-3-7-research0035.txt
technical/biomed/gb-2001-2-8-research0031.txt
technical/biomed/1471-2148-2-17.txt
technical/biomed/1471-2229-2-4.txt
technical/biomed/1471-2350-3-12.txt
technical/biomed/gb-2002-3-9-research0044.txt
technical/biomed/1471-2377-2-6.txt
technical/biomed/1471-2474-4-4.txt
technical/biomed/1472-6904-2-5.txt
technical/biomed/1472-6823-3-1.txt
technical/biomed/1471-2474-2-2.txt
technical/biomed/rr166.txt
technical/biomed/rr172.txt
technical/biomed/1471-2156-2-7.txt
technical/biomed/1472-6785-1-5.txt
technical/biomed/bcr284.txt
technical/biomed/gb-2002-3-2-research0008.txt
technical/biomed/gb-2002-3-11-research0059.txt
technical/biomed/cc2190.txt
technical/biomed/gb-2002-3-11-research0065.txt
technical/biomed/1471-213X-3-2.txt
technical/biomed/1471-2148-3-18.txt
technical/biomed/1471-2229-3-3.txt
technical/biomed/1471-2172-4-1.txt
technical/biomed/ar795.txt
technical/biomed/1471-2164-3-15.txt
technical/biomed/cc1843.txt
technical/biomed/1471-2164-3-29.txt
technical/biomed/1471-2458-2-16.txt
technical/biomed/1475-925X-2-10.txt
technical/biomed/1472-6807-3-1.txt
technical/biomed/gb-2003-4-9-r58.txt
technical/biomed/1471-2105-4-27.txt
technical/biomed/1471-2105-3-12.txt
technical/biomed/gb-2003-4-2-r16.txt
technical/biomed/ar408.txt
technical/biomed/ar409.txt
technical/biomed/1471-2407-2-11.txt
technical/biomed/gb-2001-2-10-research0041.txt
technical/biomed/1471-2288-3-4.txt
technical/biomed/1471-2105-4-26.txt
technical/biomed/1471-230X-1-5.txt
technical/biomed/gb-2002-3-8-research0040.txt
technical/biomed/1475-925X-2-11.txt
technical/biomed/gb-2001-2-9-research0035.txt
technical/biomed/1471-2172-3-12.txt
technical/biomed/gb-2003-4-6-r37.txt
technical/biomed/1472-6750-1-8.txt
technical/biomed/1471-244X-2-9.txt
technical/biomed/gb-2002-3-12-research0085.txt
technical/biomed/1471-213X-1-1.txt
technical/biomed/1471-2164-4-21.txt
technical/biomed/cc1856.txt
technical/biomed/1471-2180-3-15.txt
technical/biomed/1471-2164-3-28.txt
technical/biomed/cc105.txt
technical/biomed/1471-2202-2-10.txt
technical/biomed/1471-2180-1-8.txt
technical/biomed/1471-2431-3-3.txt
technical/biomed/1471-2369-3-10.txt
technical/biomed/1471-213X-3-3.txt
technical/biomed/cc1498.txt
technical/biomed/1471-2377-1-2.txt
technical/biomed/1471-2350-3-7.txt
technical/biomed/gb-2002-3-2-research0009.txt
technical/biomed/bcr285.txt
technical/biomed/gb-2002-3-6-software0001.txt
technical/biomed/1475-2867-3-12.txt
technical/biomed/1471-2229-1-2.txt
technical/biomed/1471-2407-3-3.txt
technical/biomed/1472-6890-1-4.txt
technical/biomed/1471-2172-4-2.txt
technical/biomed/1471-2164-3-16.txt
technical/biomed/1471-2091-3-18.txt
technical/biomed/1471-2202-2-12.txt
technical/biomed/1471-2164-4-23.txt
technical/biomed/1471-213X-1-3.txt
technical/biomed/gb-2002-3-12-research0087.txt
technical/biomed/1471-2091-3-30.txt
technical/biomed/gb-2002-3-12-research0078.txt
technical/biomed/1471-2164-3-9.txt
technical/biomed/1471-2172-3-10.txt
technical/biomed/1471-2172-2-4.txt
technical/biomed/1471-2180-1-12.txt
technical/biomed/gb-2001-2-6-research0018.txt
technical/biomed/gb-2001-2-9-research0037.txt
technical/biomed/1471-2105-2-9.txt
technical/biomed/1472-6904-3-1.txt
technical/biomed/1472-6793-2-16.txt
technical/biomed/1472-6807-3-2.txt
technical/biomed/1476-072X-2-4.txt
technical/biomed/1471-2121-2-18.txt
technical/biomed/1471-2148-2-8.txt
technical/biomed/1471-2105-4-24.txt
technical/biomed/1471-2156-3-3.txt
technical/biomed/1471-2466-2-3.txt
technical/biomed/1471-230X-3-5.txt
technical/biomed/1471-2407-2-12.txt
technical/biomed/gb-2003-4-2-r14.txt
technical/biomed/1472-6904-1-2.txt
technical/biomed/cc1538.txt
technical/biomed/1471-2105-4-25.txt
technical/biomed/1471-2105-3-38.txt
technical/biomed/ar422.txt
technical/biomed/gb-2001-2-10-research0042.txt
technical/biomed/1471-2105-4-31.txt
technical/biomed/1472-6831-3-1.txt
technical/biomed/ar387.txt
technical/biomed/1471-230X-1-6.txt
technical/biomed/1472-6793-2-17.txt
technical/biomed/1471-2156-2-18.txt
technical/biomed/1471-2105-2-8.txt
technical/biomed/1475-925X-2-12.txt
technical/biomed/1478-7954-1-3.txt
technical/biomed/1472-6807-1-1.txt
technical/biomed/cc1882.txt
technical/biomed/1471-2164-3-8.txt
technical/biomed/1471-2180-3-9.txt
technical/biomed/gb-2002-3-12-research0079.txt
technical/biomed/1471-2091-3-31.txt
technical/biomed/gb-2002-3-12-research0086.txt
technical/biomed/1471-2164-4-22.txt
technical/biomed/1471-213X-1-2.txt
technical/biomed/1471-2202-3-8.txt
technical/biomed/1471-2458-2-6.txt
technical/biomed/1477-7827-1-48.txt
technical/biomed/1471-2229-1-3.txt
technical/biomed/1471-213X-3-4.txt
technical/biomed/cc300.txt
technical/biomed/1476-069X-1-3.txt
technical/biomed/1471-2253-1-1.txt
technical/biomed/1471-2431-3-4.txt
technical/biomed/1471-2210-2-9.txt
technical/biomed/gb-2002-3-12-research0082.txt
technical/biomed/1471-213X-1-6.txt
technical/biomed/1471-2164-4-26.txt
technical/biomed/1471-2164-3-13.txt
technical/biomed/1471-2202-2-17.txt
technical/biomed/1472-6874-3-2.txt
technical/biomed/ar778.txt
technical/biomed/ar750.txt
technical/biomed/1471-2474-3-3.txt
technical/biomed/1472-6785-2-6.txt
technical/biomed/1471-2490-3-2.txt
technical/biomed/1477-7827-1-9.txt
technical/biomed/gb-2001-2-6-research0021.txt
technical/biomed/ar624.txt
technical/biomed/1471-2121-2-21.txt
technical/biomed/1472-6920-2-3.txt
technical/biomed/ar383.txt
technical/biomed/1471-2105-3-14.txt
technical/biomed/1471-2407-2-16.txt
technical/biomed/1471-2105-3-28.txt
technical/biomed/gb-2003-4-2-r11.txt
technical/biomed/cc1529.txt
technical/biomed/1471-2407-2-17.txt
technical/biomed/1472-6815-2-3.txt
technical/biomed/ar619.txt
technical/biomed/1471-2458-2-11.txt
technical/biomed/gb-2001-2-6-research0020.txt
technical/biomed/1472-6785-2-7.txt
technical/biomed/1471-2180-1-16.txt
technical/biomed/ar745.txt
technical/biomed/1472-6890-3-2.txt
technical/biomed/cc103.txt
technical/biomed/1471-2202-2-16.txt
technical/biomed/ar792.txt
technical/biomed/gb-2002-3-12-research0083.txt
technical/biomed/1471-2180-3-13.txt
technical/biomed/1471-2210-2-8.txt
technical/biomed/1472-6793-1-8.txt
technical/biomed/1471-2458-2-3.txt
technical/biomed/1472-6750-2-21.txt
technical/biomed/1471-2431-3-5.txt
technical/biomed/1472-6882-2-10.txt
technical/biomed/1471-2350-3-1.txt
technical/biomed/gb-2002-3-11-research0062.txt
technical/biomed/1472-6882-2-5.txt
technical/biomed/gb-2002-3-11-research0060.txt
technical/biomed/1471-213X-3-7.txt
technical/biomed/cc303.txt
technical/biomed/cc1477.txt
technical/biomed/1471-2407-3-5.txt
technical/biomed/1471-2377-3-4.txt
technical/biomed/1471-2180-3-11.txt
technical/biomed/gb-2002-3-12-research0081.txt
technical/biomed/cc1852.txt
technical/biomed/1471-2164-4-25.txt
technical/biomed/1471-2091-3-22.txt
technical/biomed/1471-2202-2-14.txt
technical/biomed/1471-2164-3-10.txt
technical/biomed/1471-2164-4-19.txt
technical/biomed/cc713.txt
technical/biomed/1471-2172-3-16.txt
technical/biomed/1471-2180-1-28.txt
technical/biomed/1471-230X-1-10.txt
technical/biomed/1471-2121-2-22.txt
technical/biomed/1471-2334-2-29.txt
technical/biomed/1471-2121-3-8.txt
technical/biomed/1472-6963-1-8.txt
technical/biomed/1471-2296-3-19.txt
technical/biomed/1471-2407-2-15.txt
technical/biomed/1471-2105-3-17.txt
technical/biomed/1471-230X-3-3.txt
technical/biomed/ar430.txt
technical/biomed/1471-2156-3-4.txt
technical/biomed/1471-2466-2-4.txt
technical/biomed/1471-2296-3-18.txt
technical/biomed/1471-2105-3-16.txt
technical/biomed/1472-6920-2-1.txt
technical/biomed/1476-072X-2-3.txt
technical/biomed/gb-2003-4-9-r60.txt
technical/biomed/ar140.txt
technical/biomed/gb-2003-4-3-r17.txt
technical/biomed/1472-6793-2-11.txt
technical/biomed/1472-6823-2-2.txt
technical/biomed/1471-2180-1-29.txt
technical/biomed/1471-2172-2-3.txt
technical/biomed/1471-2407-1-6.txt
technical/biomed/1471-2091-2-9.txt
technical/biomed/1471-2202-2-15.txt
technical/biomed/1471-2091-3-23.txt
technical/biomed/1471-2164-4-24.txt
technical/biomed/1471-213X-1-4.txt
technical/biomed/gb-2002-3-12-research0080.txt
technical/biomed/1471-2180-3-10.txt
technical/biomed/cc1476.txt
technical/biomed/1471-2407-3-4.txt
technical/biomed/1471-2431-3-6.txt
technical/biomed/bcr294.txt
technical/biomed/gb-2002-3-11-research0061.txt
technical/biomed/1477-7827-1-43.txt
technical/biomed/1475-2832-1-1.txt
technical/biomed/1471-2199-3-12.txt
technical/biomed/1471-2202-1-1.txt
technical/biomed/1472-6750-2-13.txt
technical/biomed/cc2172.txt
technical/biomed/1472-6793-1-6.txt
technical/biomed/1471-2210-2-6.txt
technical/biomed/1471-213X-1-9.txt
technical/biomed/1471-2164-3-34.txt
technical/biomed/1471-2164-4-15.txt
technical/biomed/1471-2202-3-3.txt
technical/biomed/1471-2202-2-18.txt
technical/biomed/cc2358.txt
technical/biomed/1472-6793-3-4.txt
technical/biomed/gb-2002-3-12-research0072.txt
technical/biomed/1472-6963-3-11.txt
technical/biomed/1472-6963-3-6.txt
technical/biomed/1476-4598-2-3.txt
technical/biomed/1477-7827-1-6.txt
technical/biomed/1475-4924-1-10.txt
technical/biomed/1472-6874-2-13.txt
technical/biomed/1471-2369-4-5.txt
technical/biomed/1471-2121-3-4.txt
technical/biomed/1471-2121-2-12.txt
technical/biomed/1471-2148-2-2.txt
technical/biomed/1475-925X-2-6.txt
technical/biomed/1471-2148-1-14.txt
technical/biomed/1471-2407-2-19.txt
technical/biomed/1471-2210-2-14.txt
technical/biomed/1475-2867-3-4.txt
technical/biomed/ar429.txt
technical/biomed/1471-2407-2-31.txt
technical/biomed/1471-2105-3-26.txt
technical/biomed/1477-7525-1-12.txt
technical/biomed/1471-2407-2-18.txt
technical/biomed/1471-2105-4-13.txt
technical/biomed/gb-2003-4-1-r5.txt
technical/biomed/1471-2334-2-24.txt
technical/biomed/1471-2318-3-2.txt
technical/biomed/1471-2156-2-12.txt
technical/biomed/1471-2180-1-31.txt
technical/biomed/1476-4598-2-2.txt
technical/biomed/1472-684X-2-1.txt
technical/biomed/1471-5945-3-3.txt
technical/biomed/1472-6963-3-7.txt
technical/biomed/1475-2891-2-1.txt
technical/biomed/1471-2091-2-5.txt
technical/biomed/1472-6793-3-5.txt
technical/biomed/1475-4924-1-5.txt
technical/biomed/1471-2202-2-19.txt
technical/biomed/1471-2091-3-13.txt
technical/biomed/1471-2164-4-14.txt
technical/biomed/1471-2164-3-35.txt
technical/biomed/1471-2164-4-28.txt
technical/biomed/cc2167.txt
technical/biomed/bcr273.txt
technical/biomed/1477-7827-1-54.txt
technical/biomed/1471-2334-2-1.txt
technical/biomed/1471-2199-3-11.txt
technical/biomed/1472-6750-2-10.txt
technical/biomed/1471-2210-2-5.txt
technical/biomed/cc2171.txt
technical/biomed/1471-2164-3-23.txt
technical/biomed/1471-2164-4-16.txt
technical/biomed/ar774.txt
technical/biomed/1476-511X-1-2.txt
technical/biomed/1472-6963-3-12.txt
technical/biomed/1471-2091-2-7.txt
technical/biomed/gb-2002-3-12-research0071.txt
technical/biomed/1471-2180-1-33.txt
technical/biomed/gb-2000-1-1-research002.txt
technical/biomed/gb-2001-3-1-research0005.txt
technical/biomed/bcr45.txt
technical/biomed/1471-2091-4-1.txt
technical/biomed/gb-2003-4-1-r7.txt
technical/biomed/1471-2334-2-26.txt
technical/biomed/1471-2121-2-11.txt
technical/biomed/1471-5945-1-3.txt
technical/biomed/1471-2105-3-18.txt
technical/biomed/1471-2261-3-5.txt
technical/biomed/1471-2105-3-24.txt
technical/biomed/1476-5918-1-2.txt
technical/biomed/1477-7525-1-10.txt
technical/biomed/gb-2002-3-5-research0024.txt
technical/biomed/1471-2105-3-30.txt
technical/biomed/1471-2407-2-33.txt
technical/biomed/gb-2002-3-5-research0025.txt
technical/biomed/1471-2261-3-4.txt
technical/biomed/1471-2199-3-3.txt
technical/biomed/1471-2121-2-10.txt
technical/biomed/1471-2121-3-6.txt
technical/biomed/1471-2334-2-27.txt
technical/biomed/gb-2001-3-1-research0004.txt
technical/biomed/1471-2105-2-1.txt
technical/biomed/1471-2261-1-6.txt
technical/biomed/gb-2003-4-3-r18.txt
technical/biomed/ar615.txt
technical/biomed/ar601.txt
technical/biomed/1476-4598-2-1.txt
technical/biomed/1472-684X-2-2.txt
technical/biomed/1471-2180-1-26.txt
technical/biomed/1471-2458-2-21.txt
technical/biomed/1472-6793-3-6.txt
technical/biomed/1472-6963-3-13.txt
technical/biomed/1471-2164-3-1.txt
technical/biomed/1471-2202-3-1.txt
technical/biomed/1471-2210-2-4.txt
technical/biomed/1471-2199-3-10.txt
technical/biomed/1471-2350-2-12.txt
technical/biomed/1471-2350-3-9.txt
technical/biomed/1475-9268-1-1.txt
technical/biomed/gb-2001-2-2-research0004.txt
technical/biomed/cc2160.txt
technical/biomed/1472-6750-3-4.txt
technical/biomed/1471-2202-3-5.txt
technical/biomed/1471-2164-4-13.txt
technical/biomed/1471-2091-3-14.txt
technical/biomed/1471-5945-2-13.txt
technical/biomed/1471-2164-3-32.txt
technical/biomed/1471-2164-3-26.txt
technical/biomed/1471-2288-2-11.txt
technical/biomed/1471-2180-3-4.txt
technical/biomed/1472-6750-1-6.txt
technical/biomed/gb-2003-4-6-r39.txt
technical/biomed/1471-2458-2-25.txt
technical/biomed/gb-2003-4-3-r20.txt
technical/biomed/gb-2003-4-9-r57.txt
technical/biomed/1471-2121-3-2.txt
technical/biomed/1471-2407-2-23.txt
technical/biomed/1471-2105-4-28.txt
technical/biomed/1472-6947-2-7.txt
technical/biomed/gb-2002-3-5-research0021.txt
technical/biomed/ar407.txt
technical/biomed/1471-2199-3-7.txt
technical/biomed/1475-2867-3-2.txt
technical/biomed/1475-925X-2-1.txt
technical/biomed/1475-2867-3-3.txt
technical/biomed/1471-2105-3-34.txt
technical/biomed/gb-2002-3-5-research0020.txt
technical/biomed/1471-2407-2-22.txt
technical/biomed/1471-2121-2-15.txt
technical/biomed/1471-2148-2-5.txt
technical/biomed/cc1044.txt
technical/biomed/1471-2091-4-5.txt
technical/biomed/1471-2288-1-9.txt
technical/biomed/rr37.txt
technical/biomed/gb-2001-3-1-research0001.txt
technical/biomed/1472-6963-3-1.txt
technical/biomed/1471-2172-2-9.txt
technical/biomed/1471-2458-2-18.txt
technical/biomed/1471-2180-3-5.txt
technical/biomed/1471-2288-2-10.txt
technical/biomed/1471-2164-3-4.txt
technical/biomed/1472-6793-3-3.txt
technical/biomed/gb-2002-3-12-research0075.txt
technical/biomed/1471-2164-3-27.txt
technical/biomed/1471-2164-3-33.txt
technical/biomed/1471-2091-3-15.txt
technical/biomed/1471-2202-3-4.txt
technical/biomed/1472-6750-2-14.txt
technical/biomed/1471-2180-1-7.txt
technical/biomed/cc1497.txt
technical/biomed/1471-2334-2-5.txt
technical/biomed/1471-2199-3-17.txt
technical/biomed/1471-2350-2-11.txt
technical/biomed/1471-2334-2-7.txt
technical/biomed/cc1495.txt
technical/biomed/1475-9268-1-2.txt
technical/biomed/1477-7827-1-46.txt
technical/biomed/1471-2091-3-17.txt
technical/biomed/ar799.txt
technical/biomed/1471-2164-3-19.txt
technical/biomed/gb-2002-3-12-research0088.txt
technical/biomed/1471-2164-3-31.txt
technical/biomed/gb-2002-3-12-research0077.txt
technical/biomed/1472-6963-3-14.txt
technical/biomed/1475-2875-1-14.txt
technical/biomed/1471-2164-3-6.txt
technical/biomed/gb-2003-4-8-r51.txt
technical/biomed/1475-2875-2-4.txt
technical/biomed/1472-6963-1-11.txt
technical/biomed/ar612.txt
technical/biomed/1472-6793-2-19.txt
technical/biomed/1471-230X-1-8.txt
technical/biomed/1471-2148-2-7.txt
technical/biomed/gb-2002-3-5-research0022.txt
technical/biomed/1471-2288-3-9.txt
technical/biomed/1471-2105-3-22.txt
technical/biomed/1472-6947-2-4.txt
technical/biomed/bcr317.txt
technical/biomed/1475-925X-2-3.txt
technical/biomed/bcr303.txt
technical/biomed/1471-2105-3-23.txt
technical/biomed/1471-2288-3-8.txt
technical/biomed/bcr458.txt
technical/biomed/1471-2105-3-37.txt
technical/biomed/gb-2002-3-5-research0023.txt
technical/biomed/1472-6793-2-18.txt
technical/biomed/1471-2156-2-17.txt
technical/biomed/1471-2369-4-1.txt
technical/biomed/ar149.txt
technical/biomed/gb-2002-3-6-research0029.txt
technical/biomed/1471-2121-1-2.txt
technical/biomed/1471-2180-1-34.txt
technical/biomed/gb-2003-4-8-r50.txt
technical/biomed/1471-2164-3-7.txt
technical/biomed/1471-2164-3-30.txt
technical/biomed/1471-2202-2-20.txt
technical/biomed/1471-2164-3-24.txt
technical/biomed/1471-2202-3-7.txt
technical/biomed/1471-2091-3-16.txt
technical/biomed/1471-2164-3-18.txt
technical/biomed/1472-6750-3-6.txt
technical/biomed/1472-6793-1-2.txt
technical/biomed/1471-2334-2-6.txt
technical/biomed/1471-213X-1-15.txt
technical/biomed/cc350.txt
technical/biomed/1471-2148-3-1.txt
technical/biomed/bcr588.txt
technical/biomed/1471-2199-2-2.txt
technical/biomed/1475-2867-2-7.txt
technical/biomed/1468-6708-3-4.txt
technical/biomed/1471-2121-3-25.txt
technical/biomed/1471-2334-3-12.txt
technical/biomed/1471-2202-4-6.txt
technical/biomed/1472-6882-1-10.txt
technical/biomed/1471-2121-3-19.txt
technical/biomed/1471-2164-4-6.txt
technical/biomed/1471-2229-2-9.txt
technical/biomed/gb-2002-3-9-research0049.txt
technical/biomed/1471-2334-1-17.txt
technical/biomed/1471-2180-2-1.txt
technical/biomed/cc973.txt
technical/biomed/1471-2210-1-7.txt
technical/biomed/1471-2326-2-4.txt
technical/biomed/1471-2180-2-16.txt
technical/biomed/1471-2121-4-1.txt
technical/biomed/1471-213X-2-8.txt
technical/biomed/1472-6793-1-12.txt
technical/biomed/1471-2199-4-4.txt
technical/biomed/1471-2199-4-5.txt
technical/biomed/1471-2369-3-1.txt
technical/biomed/1471-2164-2-1.txt
technical/biomed/1471-2202-2-1.txt
technical/biomed/gb-2002-3-9-research0048.txt
technical/biomed/1471-2229-2-8.txt
technical/biomed/1471-2474-4-8.txt
technical/biomed/1471-2121-3-18.txt
technical/biomed/1472-6882-1-11.txt
technical/biomed/1471-2334-3-13.txt
technical/biomed/cvm-2-1-038.txt
technical/biomed/1471-2121-3-30.txt
technical/biomed/1471-2156-4-10.txt
technical/biomed/1471-2199-2-3.txt
technical/biomed/1476-4598-1-3.txt
technical/biomed/1471-2172-2-10.txt
technical/biomed/1471-2121-2-6.txt
technical/biomed/1477-7827-1-21.txt
technical/biomed/1477-7827-1-23.txt
technical/biomed/1475-2891-1-2.txt
technical/biomed/1468-6708-3-7.txt
technical/biomed/1471-2199-2-1.txt
technical/biomed/1471-2105-1-1.txt
technical/biomed/gb-2002-3-10-research0052.txt
technical/biomed/1471-2202-4-5.txt
technical/biomed/1471-2334-3-11.txt
technical/biomed/1471-2164-4-5.txt
technical/biomed/cvm-2-6-278.txt
technical/biomed/cvm-2-4-180.txt
technical/biomed/1471-2105-3-3.txt
technical/biomed/gb-2003-4-2-r9.txt
technical/biomed/1475-2883-2-11.txt
technical/biomed/1475-2867-2-10.txt
technical/biomed/1471-2202-2-3.txt
technical/biomed/bcr602.txt
technical/biomed/1471-2180-2-2.txt
technical/biomed/gb-2001-2-12-research0055.txt
technical/biomed/1478-1336-1-4.txt
technical/biomed/1472-6793-2-4.txt
technical/biomed/1471-2210-1-4.txt
technical/biomed/1471-2091-3-4.txt
technical/biomed/1471-2121-4-2.txt
technical/biomed/gb-2002-3-4-research0019.txt
technical/biomed/1471-2202-3-10.txt
technical/biomed/1471-2180-2-29.txt
technical/biomed/1472-6750-2-2.txt
technical/biomed/1476-511X-2-3.txt
technical/biomed/cc1547.txt
technical/biomed/1472-6793-1-11.txt
technical/biomed/1471-2407-2-9.txt
technical/biomed/1471-2407-2-8.txt
technical/biomed/1476-4598-2-28.txt
technical/biomed/1476-511X-2-2.txt
technical/biomed/1471-2202-3-11.txt
technical/biomed/1471-2121-4-3.txt
technical/biomed/gb-2002-3-4-research0018.txt
technical/biomed/1471-2261-2-11.txt
technical/biomed/1472-6793-2-5.txt
technical/biomed/1471-2164-2-2.txt
technical/biomed/gb-2001-2-12-research0054.txt
technical/biomed/ar104.txt
technical/biomed/1471-2407-1-15.txt
technical/biomed/1471-2202-2-2.txt
technical/biomed/gb-2003-4-2-r8.txt
technical/biomed/1472-6947-1-2.txt
technical/biomed/1471-2105-3-2.txt
technical/biomed/1471-2229-2-11.txt
technical/biomed/1471-2164-4-4.txt
technical/biomed/cvm-2-6-286.txt
technical/biomed/1471-2148-1-1.txt
technical/biomed/1471-2334-3-10.txt
technical/biomed/1472-6882-1-12.txt
technical/biomed/gb-2002-3-10-research0053.txt
technical/biomed/1471-2156-2-8.txt
technical/biomed/rr196.txt
technical/biomed/1471-2148-3-3.txt
technical/biomed/1472-6807-2-9.txt
technical/biomed/1477-7827-1-36.txt
technical/biomed/1471-2148-3-7.txt
technical/biomed/1471-213X-1-13.txt
technical/biomed/1471-2121-2-1.txt
technical/biomed/gb-2002-3-3-research0012.txt
technical/biomed/1471-2156-3-22.txt
technical/biomed/bcr571.txt
technical/biomed/1471-2199-2-4.txt
technical/biomed/gb-2000-1-2-research0003.txt
technical/biomed/1472-6955-2-1.txt
technical/biomed/1471-2105-3-6.txt
technical/biomed/1471-2474-3-23.txt
technical/biomed/1472-6947-1-6.txt
technical/biomed/1471-2407-3-14.txt
technical/biomed/1471-2202-2-6.txt
technical/biomed/1475-2867-2-15.txt
technical/biomed/1472-6793-2-1.txt
technical/biomed/gb-2002-3-8-research0039.txt
technical/biomed/1471-2369-3-6.txt
technical/biomed/bcr607.txt
technical/biomed/1472-6874-2-8.txt
technical/biomed/1471-2180-2-7.txt
technical/biomed/1471-2164-2-6.txt
technical/biomed/1471-2180-2-38.txt
technical/biomed/1471-2210-3-3.txt
technical/biomed/1471-2431-2-11.txt
technical/biomed/1472-6793-1-15.txt
technical/biomed/1471-2458-3-9.txt
technical/biomed/1471-2121-4-6.txt
technical/biomed/1471-2202-3-14.txt
technical/biomed/1471-2164-2-7.txt
technical/biomed/gb-2001-2-12-research0051.txt
technical/biomed/gb-2002-3-8-research0038.txt
technical/biomed/1471-244X-3-5.txt
technical/biomed/1471-2202-2-7.txt
technical/biomed/1471-2334-1-10.txt
technical/biomed/1471-2407-3-15.txt
technical/biomed/1471-2121-3-22.txt
technical/biomed/1471-2334-3-15.txt
technical/biomed/1471-2148-1-4.txt
technical/biomed/1471-2199-2-5.txt
technical/biomed/1468-6708-3-3.txt
technical/biomed/bcr570.txt
technical/biomed/gb-2002-3-10-research0056.txt
technical/biomed/1472-6947-3-5.txt
technical/biomed/cc343.txt
technical/biomed/1471-213X-1-12.txt
technical/biomed/1471-2296-3-3.txt
technical/biomed/1477-7827-1-27.txt
technical/biomed/1476-4598-1-5.txt
technical/biomed/rr191.txt
technical/biomed/1471-2148-3-4.txt
technical/biomed/1471-2458-3-11.txt
technical/biomed/1475-2875-1-5.txt
technical/biomed/1477-7827-1-31.txt
technical/biomed/1471-213X-1-10.txt
technical/biomed/gb-2002-3-3-research0011.txt
technical/biomed/gb-2002-3-10-research0054.txt
technical/biomed/1468-6708-3-1.txt
technical/biomed/1471-2148-1-6.txt
technical/biomed/1471-2202-4-3.txt
technical/biomed/1472-6947-1-5.txt
technical/biomed/1471-2202-2-5.txt
technical/biomed/1476-9433-1-2.txt
technical/biomed/1471-2210-1-2.txt
technical/biomed/1471-2458-1-9.txt
technical/biomed/1472-6793-2-2.txt
technical/biomed/gb-2001-2-12-research0053.txt
technical/biomed/1478-1336-1-2.txt
technical/biomed/1471-2202-3-16.txt
technical/biomed/1471-2180-2-13.txt
technical/biomed/1471-2121-4-4.txt
technical/biomed/gb-2003-4-4-r28.txt
technical/biomed/1471-230X-2-17.txt
technical/biomed/1477-5956-1-1.txt
technical/biomed/1471-2156-4-9.txt
technical/biomed/1471-2431-2-12.txt
technical/biomed/ar328.txt
technical/biomed/1471-2210-3-1.txt
technical/biomed/1471-2121-4-5.txt
technical/biomed/1471-2350-2-8.txt
technical/biomed/1471-2202-3-17.txt
technical/biomed/1471-2407-1-13.txt
technical/biomed/bcr605.txt
technical/biomed/1476-069X-2-9.txt
technical/biomed/1478-1336-1-3.txt
technical/biomed/1471-2164-2-4.txt
technical/biomed/1471-2210-1-3.txt
technical/biomed/1476-9433-1-3.txt
technical/biomed/1471-2334-1-13.txt
technical/biomed/1471-2407-3-16.txt
technical/biomed/1471-2164-4-2.txt
technical/biomed/cvm-2-4-187.txt
technical/biomed/1471-2105-3-4.txt
technical/biomed/1471-2121-3-21.txt
technical/biomed/1471-2202-4-2.txt
technical/biomed/1471-2172-3-9.txt
technical/biomed/gb-2001-2-3-research0007.txt
technical/biomed/1471-2199-2-6.txt
technical/biomed/bcr567.txt
technical/biomed/gb-2002-3-10-research0055.txt
technical/biomed/1471-2121-2-3.txt
technical/biomed/1471-213X-1-11.txt
technical/biomed/1472-684X-1-5.txt
technical/biomed/1476-4598-1-6.txt

```

This example better showcases how the `-i` command option finds all instances of a word without caring about capitalization. This may be handy if you have multiple files or folders named the same thing but with different capitalization, but you want to find all instances of that word in the file or folder titles.

## `-v`
**Example One**
```
// the command
grep -e "genome" cat-one.txt -v > v-results-one.txt
```
```
// the final output

  
    
      
        Background
        assembly from Celera Genomics, created by using both
        private and public sequence information (denoted Cel2 [
        1]), and the other was the third version of the assembly
        from the public Mouse Genome Sequencing Consortium (denoted
        different mouse strains and distinct sequence-assembly
        algorithms.
        Assembled by direct overlapping sequence fragments, Cel2
        has about 260,000 contigs with a total size of 2.51 × 10
        9base-pairs (2.51 gigabases (Gb)), whereas MGSCv3 has about
        incorporating pair-end sequence information, Cel2 and
        MGSCv3 contain around 47,000 scaffolds and around 43,000
        supercontigs, respectively. After using physical map
        information, such as sequence-tagged sites (STSs), 20 mouse
        chromosomes were constructed together with an 'unassigned'
        chromosome called chromosome UA in Cel2 and chromosome Un
        in MGSCv3. The total sizes of these chromosomes in Cel2 and
        MGSCv3 are about 2.62 Gb and 2.59 Gb, respectively. The
        4.5% in MGSCv3. The average size of the contigs, gaps and
        comparable (data not shown).
      
      
        Results
        
          Comparison of the two mouse assemblies
          To compare the coverage and accuracy of Cel2 and
          MGSCv3, we used BLAT [ 3] to compare 8,434 mouse mRNA
          sequences in the RefSeq database of the National Center
          for Biotechnology Information (NCBI) ([ 4], 12 April,
          2002) with these assemblies. As a basis for comparison,
          we determined the numbers of mRNAs that have more than
          50%, 80%, 90% and 97% of base-pairs that match in both
          Cel2 and MGSCv3, in one assembly only, or in neither
          assembly (Figure 1). Although most mRNAs could be matched
          with both assemblies, there were some mRNAs that could be
          matched well in only one assembly. We also found that
          more mRNAs had higher percentage matches in Cel2 than in
          MGSCv3 (that is, > 97%). As a further test, we
          especially investigated how well long mRNAs can be
          matched to each assembly. The 10 longest mRNA sequences
          are all matched well with both assemblies, except for the
          
          piccolo (Pico) gene (coding for a
          presynaptic cytomatrix protein): paradoxically, it is
          matched in chromosome 12 in Cel2 and in chromosome 5 in
          MGSCv3.
          mRNA sequences can only be used to check the quality
          of assembly in the gene regions, and, for this, the
          accuracy can be determined only within exons. Therefore,
          we also used 39 newly finished mouse bacterial artificial
          chromosomes (BACs) with known chromosome locations (data
          from 14 May to 23 May 2002 in NCBI daily updates) to test
          the coverage and accuracy of long continuous regions.
          Although all BACs matched at the correct chromosomal
          locations in both assemblies, MGSCv3 exhibited higher
          matching quality in these BAC regions. In MGSCv3, only
          two BACs had less than 90% coverage: AC087780 (74% in
          chromosome 1) and AC099773 (84% in chromosome 5) while
          four BACs in Cel2 had less than 90% coverage: AC090479
          (11% in chromosome 18), AC023789 (85% in chromosome 4),
          AC021063 (86% in chromosome 18), and AC087115 (89% in
          chromosome 8). For the BAC AC090479, which has only 11%
          coverage in Cel2, many of the matching genomic DNA
          fragments are in the UA chromosome at present. This is
          probably due to the fact that the BAC sequencing
          information that Cel2 used was released before July 2001.
          Therefore, MGSCv3 could have a better assembly in those
          regions where newer BAC information had been
          incorporated. Interestingly, these two assemblies show
          some complementary features. For example, BAC AC090479
          was only 11% covered in Cel2 but 99% covered by
          chromosome 18 in MGSCv3, whereas BAC AC087780 had 74%
          coverage in MGSCv3 but 94% coverage in chromosome 1 in
          Cel2.
          As well as using different assembly methods, Cel2 and
          MGSCv3 also used different mouse strains. This may also
          cause some slight differences when comparing mRNAs or
          BACs from different strains with Cel2 and MGSCv3.
          From the above analyses of mouse mRNAs and BACs, we
          find some features that are complementary between MGSCv3
          and Cel2. This complementarity partly originates from the
          fact that MGSCv3 used more BAC sequencing information,
          which produced higher sequence quality and coverage in
          the regions covered by sequenced BACs. However, the mRNA
          test indicates that the sequences in Cel2 are more
          accurate than sequences in MGSCv3 in gene regions. Thus,
          assemblies be used in an integrated fashion rather than
          separately.
        
        
          the two mouse assemblies
          stage [ 5], providing the opportunity to compare the
          which is also NCBI build 28, with each mouse assembly. We
          first used RepeatMasker (A. Smit, unpublished work, and [
          fixed expect value (E-value) of 1.0e-1 we found 1,860,560
          conserved sequence elements (CSEs) between the human and
          simplest cases, which we call 'univalent CSEs', are
          matches between unique human and mouse regions. However,
          there are some cases, which we call 'multivalent CSEs',
          where more than one mouse region matches the same human
          region, or conversely, more than one human region matches
          a mouse region. For all multivalent CSEs, we chose the
          longest matches, and added them to univalent CSEs to make
          a new set, called 'primary CSE set'.
          Table 1shows the numbers of CSEs and the base-pairs
          categories: all CSEs; human primary CSEs; mouse primary
          CSEs; and univalent CSEs. We can see that the CSEs from
          the Celera mouse assembly cover slightly more of the
          numbers of univalent CSEs from two mouse assemblies are
          almost the same (about 415,000), 31,000 univalent CSEs
          alone, while 31,194 were identified from Cel2. This
          indicates again that the majority of univalent CSEs are
          common between Cel2 and MGSCv3, yet some differences
          remain between these two assemblies. We then constructed
          an overall CSE set, which includes all CSEs found from
          both MGSCv3 and Cel2 (named as aCSE) and its human
          primary set (named as aCSE-hp). Figure 2aillustrates the
          length distribution of CSEs in set aCSE and aCSE-hp, and
          Figure 2bshows the plot of the percentage identity versus
          length of CSEs in set aCSE. Most of the CSEs show 80-95%
          CSEs overlap each other more frequently in the human or
          to happen by chance. At the E < 1.0e-1 level, the
          average length of CSEs is 109 bp in the aCSE set and 151
          bp in the aCSE-hp set. The shortest one is only 26 bp,
          and the longest one is 6,735 bp, which covers the human
          spastic ataxia of Charlevoix-Saguenay (sacsin) gene.
          from Cel2 and MGSCv3 and found that about 90 megabases
          covered by all CSEs. CSEs from Cel2 and MGSCv3 covered
          97% and 95% of this 90-Mb region, respectively. Among
          them, 92% of the base-pairs were covered by both CSE
          sets. Figure 3shows the distribution of CSE locations in
          each human chromosome. These results also suggest that
          the Celera mouse assembly has slightly higher coverage
          Cross-species sequence conservation information is
          widely used in current gene-prediction tools, such as
          TwinScan [ 8], SGP-1 [ 9] and SLAM (L. Pachter, personal
          communication). Lack of conservation information may
          decrease the prediction accuracy of these methods. The
          the two different mouse assemblies suggests that, for
          reliability, it is advisable to use both assemblies to
          search genes and to perform other functional
          analyses.
        
        
          Functional analysis of CSEs
          
            CSEs in the human RefSeq genes
            We used the human RefSeq gene annotation from golden
            path December 2001 database to explore further the
            possible functional implications of these CSEs. The
            with a total of 14,653 RefSeq gene structures. We found
            that 94.9% of these genes and 81% of all exons
            (139,694) are covered by the CSEs in aCSE-hp (data not
            shown). These CSEs also cover 57.2% of the base-pairs
            in the exon regions. Coding regions (CDS) have a higher
            degree of CSE coverage (77.7% at the nucleotide level)
            than 5' untranslated regions (5' UTRs) (24%) and 3'
            UTRs (18%). In addition, 35.3% of the CSEs are located
            within RefSeq gene regions. Of these, 19.3% are
            exon-related, 14.8% are intron-related, and 1.2% are
            'alternative exon'-related (that is, the location of
            according to one mRNA transcript, but in an exon region
            according to another mRNA transcript). Here, the
            relationship between a CSE and an exon or an intron is
            defined by the location of the middle point of the CSE.
            Clearly, most of the RefSeq genes have at least one CSE
            hit.
          
          
            CSEs in the known and predicted members of
            ETS-domain protein family
            To determine whether CSEs are found within regions
            encoding conserved protein motifs, we examined the
            relationship of CSE locations and the protein-domain of
            a large gene family (the ETS-domain family) found by
            our GeneFamilyScan software (GFScan [ 10], Table 2).
            For all known human members in this gene family, only
            one (TEL2) has no CSE hit. Additionally, all five newly
            identified potential human ETS-domain genes have CSE
            hits in the corresponding motif regions. Remarkably,
            for the novel gene corresponding to Ensembl [ 11]
            predicted transcript ENST00000299272, we found a mouse
            ETS-domain gene ( 
            Spi-C ), which shares 67% protein
            sequence identity with the human predicted transcript.
            Hence, the existence of CSEs within a protein-domain
            region can help to find other related novel genes.
          
          
            CSEs in intronic regions of known genes
            To investigate the possible function of CSEs in
            intronic regions, we examined a subset of intronic
            CSEs. The ten longest intronic CSEs (see Supplementary
            Table 1 in the additional data file) within known human
            genes, including RefSeq genes and other genes with
            known mRNA sequences, were further examined and
            compared with the human golden path annotation [ 12].
            Our analysis indicates that many long intron-related
            CSEs may actually be alternative exons. For example, we
            found that nine of these ten CSEs have spliced or
            unspliced human expressed sequence tag (EST) matches.
            Some of them also shared similarity with mouse mRNAs.
            There was only one CSE for which we could not get any
            functional information (CSE ID is chr1c_0024330, it is
            located at human chr1:246028930-246030945 in an intron
            region of a novel gene with mRNA accession number
            AL122093.)
          
          
            CSEs, genes and transcriptional activity in human
            chromosome 22
            To investigate whether the density of CSEs
            correlates with gene density and resulting
            transcriptional activity, we chose to examine
            chromosome 22, a well-finished and annotated
            chromosome, more closely. We calculated the density of
            base-pairs in the CSEs and Sanger Center annotated
            exons in chromosome 22 separately. The distributions of
            these densities are consistent across the length of the
            whole chromosome (Figure 4), except one region around
            21-22 Mb. The average length of the exons in this 1-Mb
            region is 247 bp, which is shorter than the average
            length of all exons in chromosome 22 (302 bp). Although
            the average CSE length is shorter than 247 bp, some
            very short conserved regions still cannot be covered by
            CSEs, which causes the difference of the base-pair
            densities between CSEs and exons in this region. To
            further investigate this discrepancy, we used data
            collected by Affymetrix. From Affymetrix
            oligonucleotide array data [ 13], we obtained the
            density of the base-pairs around all positive probes in
            this chromosome. The density distribution of base-pairs
            in CSEs is more consistent with that in exons than that
            in oligonucleotide probe regions for this chromosome.
            This indicates that the Affymetrix oligonucleotide
            array detected more transcripts that are less conserved
            compared to the known exons, especially those
            transcripts toward the end of the chromosome.
          
          
            CSEs between known genes
            Many long CSEs that are located in the intergenic
            regions between the known human genes were also
            scrutinized against ESTs and genes in other species.
            Although two of the five longest intergenic region CSEs
            were determined to be pseudogenes, two others appeared
            to be gene-related. CSE chr9p_0053719 is located in the
            17707984-17709816 region of human chromosome 9. In the
            corresponding mouse region, transcript mCT3199 was
            predicted in the Celera database and a zinc-finger
            protein basonuclin-like gene (XM_143875) was also
            annotated by NCBI. CSE chr6p_0056662 (human location:
            chr6: 51140014-51141656) also overlaps one annotated
            novel gene, dJ402H5.2 in NCBI. Of these five CSEs, only
            CSE chr6p_0077382 does not have any annotation
            information. However, we found a genomic region in the
            (chr6:105053471-105055327) with 94% identity, which is
            not much lower than 95% within this CSE. Even in the
            well-finished and annotated human chromosomes, such as
            20 and 22, some long intergenic region CSEs can still
            be found. Some of them are pseudogenes, while others
            appear to be related to novel genes. For example, in
            the human genomic region around CSE chr20p_0001494, a
            novel gene is predicted by GenomeScan [ 14] in NCBI
            (XM_104356), but is not otherwise confirmed. In the
            human gene models that are covered by at least one CSE.
            Although we do not know the clear functions for these
            novel gene-related CSEs, they obviously deserve
            immediate experimental verification.
          
          
            CSEs in promoter regions
            It is probable that many CSEs located in promoter
            regions contain 
            cis -regulatory elements. For
            example, transcription factor 8 (TCF8) can repress
            interleukin 2 (IL2) expression by binding to a negative
            regulatory element 100 bp upstream of the IL2
            transcription start site [ 15]. We found that this
            promoter region is hit by our CSE chr4p_0043880 (its
            human location is chr4: 124039778-124040406). In a
            separate study, we are systematically investigating how
            to incorporate CSE information into promoter
            analysis.
          
          
            CSEs in RNA genes
            We found that 439 CSEs are related to non-translated
            annotation), such as ribosomal RNA, microRNA, small
            nucleolar RNA (snoRNA), and the like. For example, Z30
            small nucleolar RNA [ 16] (accession number: AJ007733),
            located at human chr17: 34803858-34803954, is a
            methylation guide molecule for U6 snRNA. The whole
            genomic DNA region of Z30 is hit by our CSE
            chr17p_0010358 (human location: chr17:
            34803845-34803959). By checking the corresponding mouse
            corresponding mouse Z30 gene (accession number:
            AJ007734). Therefore, genomic sequence conservation
            between different species can effectively facilitate
            the discovery of RNA genes.
          
          
            CSEs and human genomic segment duplication
            Multivalent CSEs can be used to find genomic segment
            duplication because these CSEs that hit once in one
            CSEs of length greater than 100 bp found by BLAST with
            E value < 1.0e-10 from MGSCv3 to analyze human
            genomic sequence duplication. As an example, we used
            one type of multivalent CSEs, in which a mouse region
            can match two separate human regions, to find doublet
            order. In this case, we can link them together and
            segments (> 10 kb), we found 451 pairs of human
            segments and their corresponding 451 segments in the 19
            mouse autosomal chromosomes. We defined a pair of human
            segments as doublet duplication when the segment length
            difference is less than 10% of the shorter one. For all
            451 doublet duplications, the identity between two
            human segments varies from 37% to 100%, whereas the
            length of segments varies from 10 Kb to 257 Kb (the
            data are available in our ftp server [ 17] and
            Supplementary Table 2 in the additional data file).
            Because there may have been much artificial duplication
            in the human working draft sequences due to
            misassembly, we checked those 451 human duplication
            human build 30), which has only become available very
            recently. We found that only 79 duplications exist
            according to this updated assembly, as shown in Figure
            before we constructed CSEs, these doublet duplications
            regions or further assembly errors. As we have limited
            the length of the duplication regions to longer than 10
            kb, and the length difference between two duplicated
            regions to less than 10%, these pairs of human regions
            look more like direct duplications of genomic DNA than
            a pseudogene. But their true identity may require
            experimental verification. We are trying to use CSEs to
            find more expansion or contraction regions in the human
            Although many CSEs are related to different
            CSEs are still mysterious. Experimental approaches and
            further theoretical characterizations are needed to
            discover the function of these conserved elements. All
            the CSEs and their available functional information are
            accessible and searchable in our CSEdb, which may be
          
          
            Human gene number estimation by human-mouse
            comparison
            We analyzed the correlation between CSE number and
            chromosome length. We found that they are not
            correlated very well (see Supplementary Figure 1 in the
            additional data file). From Figure 3, we found that
            most human regions with high Ensembl gene density also
            have high CSE density, although some regions may lack
            this kind of correlation, probably because of the
            divergence between the human and mouse. Together with
            the statistics of the RefSeq genes, in which most
            RefSeq genes have at least one CSE hit (see RefSeq data
            above), we believe that it is more meaningful and
            accurate to estimate protein-coding gene number from
            CSEs instead of from chromosome length. Because human
            chromosomes 20, 21 and 22 have been finished and
            annotated, we used the number of CSEs and
            protein-coding genes in these three chromosomes and the
            number of total CSEs to estimate: first, the number of
            the human and mouse homologous genes that share at
            least one CSE; and second, the total number of the
            human genes. By our estimates, the total number of
            human protein-coding genes is 37,000, of which 30,000
            are related to mouse through a CSE. The estimated
            number of all human genes at different E-values is
            almost constant. The estimated homologous gene numbers
            increased with increasing E-value as expected (Figure
            6).
          
        
      
      
        Materials and methods
        
          BLAT search
          BLAT was used to match all mouse mRNA sequences and
          BAC sequences to both mouse assemblies with the default
          parameter setting. As the chromosome information is known
          for each BAC, we only calculated the coverage of BAC by
          the corresponding chromosome in order to check the
          accuracy of the assembly.
        
        
          BLAST search
          The BLAST comparison of the human golden path with
          an 80-CPU LINUX cluster in 4 days. NCBI BLAST was used
          with different E-values as the threshold (1.0e-10,
          1.0e-4, and 1.0e-1) in this project. We found that 1.0e-1
          appeared to be the best parameter for covering both
          gene-related and non-coding CSEs, because the average
          length of exons is about 150 bp and that of regulatory
          elements is much shorter. Another reason to choose 1.0e-1
          is that we can use only those more highly significant
          CSEs if needed. The CSE number is super-exponentially
          decreased when increasing the significance from 1.0e-1 to
          1.0e-10. The other options of BLAST use -b 50000 -v 50000
          - U T.
        
        
          Density of base-pairs in CSEs
          The density of base-pairs in CSEs or Sanger Center
          annotated exons is calculated by counting the number of
          base-pairs in CSEs or exons within every 5.7-Mb window.
          This 5.7-Mb window is slided 57 kb in each step.
          Oligonucleotide array data was downloaded from the
          University of California Santa Cruz golden path server.
          The probe whose score is higher than 80 in at least one
          cell line is regarded as the positive probe. The
          following 35 base-pairs of the positive probes are
          counted to calculate the base-pair density in a 57-kb
          window [ 13].
        
        
          Pseudogene test
          To test whether a human region in a CSE encoded a
          pseudogene, we used the human genomic DNA region of this
          continuous human region contained internal splice sites
          like a cDNA, we regarded this region as a potential
          pseudogene location.
        
        
          Gene number estimation
          Estimation of the number of human protein-coding genes
          and of human-mouse homologs is based on two assumptions.
          As the only training data at present are three finished
          human chromosomes, (chromosomes 20, 21 and 22), the first
          assumption is that the percentage of the gene-related
          CSEs in three finished chromosomes is approximately the
          assumption is that the average CSE number per gene
          calculated in the three finished chromosomes is
          these assumptions, we could estimate the total number of
          human-mouse homologous genes ( 
          nGENE 
          
            hm 
           ) from the total number of CSEs in a CSE-hp ( 
          nCSE 
          
            a-hp 
           ) set, the number of human primary CSEs in three
          finished chromosomes ( 
          nCSE 
          
            3chr-hp 
           ) and the total genes in these three chromosomes
          covered by CSEs ( 
          nGENE 
          
            c 
           ):
          
          And the total number of human genes ( ) could be
          estimated by:
          
          where 
          nGENE 
          
            3chr 
           is the total number of annotated genes in
          chromosomes 20, 21 and 22, which is equal to 1,595 (the
          overlapping genes were only counted once and non-coding
          genes are not counted) from the present data. Of these,
          1,291 are covered by CSEs and the total number of human
          primary CSEs within these three chromosomes is 25,578.
          Thus, we obtain the total number of human-mouse
          homologous genes as 590,632 × (1,291/25,578) = 29,811 at
          E-values less than 1.0e-1, and the total number of human
          genes is therefore estimated as 590,632 × (1,595/25,578)
          = 36,830.
          We also tried to use information from these three
          chromosomes to estimate gene number separately, and got
          the mean number 35,322 with a standard deviation of
          9,705.
        
        
          Availability
          Data concerning 6,259 novel genes are available from [
          19] and from the CSEdb browser at [ 18]. The human
          duplicated segments data is available from [ 20].
        
      
      
        Additional data files
        Supplementary tables listing the 10 longest
        intron-region CSEs and 451 mouse genomic segments and their
        matched human segment pairs, and a figure showing the
        correlation between portions of CSEs and chromosome length
        additional data filewith the online version of this
        paper.
        Additional data file 1
        Additional tables and figure
        Supplementary tables listing the 10 longest
        intron-region CSEs and 451 mouse genomic segments and their
        matched human segment pairs, and a figure showing the
        correlation between portions of CSEs and chromosome length
        Click here for additional data file
      
    
  

```

This example shows how the `-v` option can be used to omit all (case-sensitive) instances of a word from the file that `grep` generates. This command option would be useful in omitting unnecessary information from certain documents.

**Example Two**
```
// the command
grep -e "14" lr-files.txt -v > v-results-two.txt
```
```
// the final output
technical/biomed
technical/biomed/gb-2002-4-1-r2.txt
technical/biomed/gb-2003-4-6-r41.txt
technical/biomed/cc991.txt
technical/biomed/bcr620.txt
technical/biomed/gb-2001-2-4-research0010.txt
technical/biomed/gb-2003-4-4-r24.txt
technical/biomed/ar331.txt
technical/biomed/ar319.txt
technical/biomed/gb-2001-2-4-research0011.txt
technical/biomed/rr73.txt
technical/biomed/bcr635.txt
technical/biomed/gb-2003-4-5-r34.txt
technical/biomed/ar79.txt
technical/biomed/gb-2002-4-1-r1.txt
technical/biomed/gb-2002-3-9-research0043.txt
technical/biomed/gb-2001-2-7-research0025.txt
technical/biomed/ar130.txt
technical/biomed/ar118.txt
technical/biomed/gb-2002-3-7-research0032.txt
technical/biomed/gb-2003-4-4-r26.txt
technical/biomed/cc4.txt
technical/biomed/gb-2001-2-4-research0012.txt
technical/biomed/gb-2001-2-7-research0024.txt
technical/biomed/gb-2001-2-3-research0008.txt
technical/biomed/bcr568.txt
technical/biomed/gb-2003-4-7-r46.txt
technical/biomed/ar93.txt
technical/biomed/bcr583.txt
technical/biomed/cc367.txt
technical/biomed/gb-2003-4-7-r42.txt
technical/biomed/ar68.txt
technical/biomed/gb-2002-3-9-research0046.txt
technical/biomed/rr74.txt
technical/biomed/gb-2002-3-7-research0037.txt
technical/biomed/gb-2001-2-8-research0027.txt
technical/biomed/gb-2001-2-11-research0046.txt
technical/biomed/ar120.txt
technical/biomed/gb-2001-2-8-research0032.txt
technical/biomed/gb-2002-3-7-research0036.txt
technical/biomed/gb-2003-4-5-r32.txt
technical/biomed/rr171.txt
technical/biomed/gb-2003-4-7-r43.txt
technical/biomed/ar297.txt
technical/biomed/rr167.txt
technical/biomed/gb-2003-4-5-r30.txt
technical/biomed/gb-2002-3-9-research0051.txt
technical/biomed/gb-2002-3-9-research0045.txt
technical/biomed/gb-2001-2-8-research0030.txt
technical/biomed/bcr631.txt
technical/biomed/cc3.txt
technical/biomed/ar321.txt
technical/biomed/ar309.txt
technical/biomed/gb-2001-2-11-research0045.txt
technical/biomed/bcr618.txt
technical/biomed/gb-2002-3-7-research0035.txt
technical/biomed/gb-2001-2-8-research0031.txt
technical/biomed/gb-2002-3-9-research0044.txt
technical/biomed/rr166.txt
technical/biomed/rr172.txt
technical/biomed/bcr284.txt
technical/biomed/gb-2002-3-2-research0008.txt
technical/biomed/gb-2002-3-11-research0059.txt
technical/biomed/cc2190.txt
technical/biomed/gb-2002-3-11-research0065.txt
technical/biomed/ar795.txt
technical/biomed/cc1843.txt
technical/biomed/gb-2003-4-9-r58.txt
technical/biomed/gb-2003-4-2-r16.txt
technical/biomed/ar408.txt
technical/biomed/ar409.txt
technical/biomed/gb-2001-2-10-research0041.txt
technical/biomed/gb-2002-3-8-research0040.txt
technical/biomed/gb-2001-2-9-research0035.txt
technical/biomed/gb-2003-4-6-r37.txt
technical/biomed/gb-2002-3-12-research0085.txt
technical/biomed/cc1856.txt
technical/biomed/cc105.txt
technical/biomed/gb-2002-3-2-research0009.txt
technical/biomed/bcr285.txt
technical/biomed/gb-2002-3-6-software0001.txt
technical/biomed/gb-2002-3-12-research0087.txt
technical/biomed/gb-2002-3-12-research0078.txt
technical/biomed/gb-2001-2-6-research0018.txt
technical/biomed/gb-2001-2-9-research0037.txt
technical/biomed/cc1538.txt
technical/biomed/ar422.txt
technical/biomed/gb-2001-2-10-research0042.txt
technical/biomed/ar387.txt
technical/biomed/cc1882.txt
technical/biomed/gb-2002-3-12-research0079.txt
technical/biomed/gb-2002-3-12-research0086.txt
technical/biomed/cc300.txt
technical/biomed/gb-2002-3-12-research0082.txt
technical/biomed/ar778.txt
technical/biomed/ar750.txt
technical/biomed/gb-2001-2-6-research0021.txt
technical/biomed/ar624.txt
technical/biomed/ar383.txt
technical/biomed/gb-2003-4-2-r11.txt
technical/biomed/cc1529.txt
technical/biomed/ar619.txt
technical/biomed/gb-2001-2-6-research0020.txt
technical/biomed/ar745.txt
technical/biomed/cc103.txt
technical/biomed/ar792.txt
technical/biomed/gb-2002-3-12-research0083.txt
technical/biomed/gb-2002-3-11-research0062.txt
technical/biomed/gb-2002-3-11-research0060.txt
technical/biomed/cc303.txt
technical/biomed/gb-2002-3-12-research0081.txt
technical/biomed/cc1852.txt
technical/biomed/cc713.txt
technical/biomed/ar430.txt
technical/biomed/gb-2003-4-9-r60.txt
technical/biomed/gb-2003-4-3-r17.txt
technical/biomed/gb-2002-3-12-research0080.txt
technical/biomed/bcr294.txt
technical/biomed/gb-2002-3-11-research0061.txt
technical/biomed/cc2172.txt
technical/biomed/cc2358.txt
technical/biomed/gb-2002-3-12-research0072.txt
technical/biomed/ar429.txt
technical/biomed/gb-2003-4-1-r5.txt
technical/biomed/cc2167.txt
technical/biomed/bcr273.txt
technical/biomed/cc2171.txt
technical/biomed/ar774.txt
technical/biomed/gb-2002-3-12-research0071.txt
technical/biomed/gb-2000-1-1-research002.txt
technical/biomed/gb-2001-3-1-research0005.txt
technical/biomed/bcr45.txt
technical/biomed/gb-2003-4-1-r7.txt
technical/biomed/gb-2002-3-5-research0024.txt
technical/biomed/gb-2002-3-5-research0025.txt
technical/biomed/gb-2001-3-1-research0004.txt
technical/biomed/gb-2003-4-3-r18.txt
technical/biomed/ar615.txt
technical/biomed/ar601.txt
technical/biomed/gb-2001-2-2-research0004.txt
technical/biomed/cc2160.txt
technical/biomed/gb-2003-4-6-r39.txt
technical/biomed/gb-2003-4-3-r20.txt
technical/biomed/gb-2003-4-9-r57.txt
technical/biomed/gb-2002-3-5-research0021.txt
technical/biomed/ar407.txt
technical/biomed/gb-2002-3-5-research0020.txt
technical/biomed/cc1044.txt
technical/biomed/rr37.txt
technical/biomed/gb-2001-3-1-research0001.txt
technical/biomed/gb-2002-3-12-research0075.txt
technical/biomed/ar799.txt
technical/biomed/gb-2002-3-12-research0088.txt
technical/biomed/gb-2002-3-12-research0077.txt
technical/biomed/gb-2003-4-8-r51.txt
technical/biomed/ar612.txt
technical/biomed/gb-2002-3-5-research0022.txt
technical/biomed/bcr317.txt
technical/biomed/bcr303.txt
technical/biomed/bcr458.txt
technical/biomed/gb-2002-3-5-research0023.txt
technical/biomed/gb-2002-3-6-research0029.txt
technical/biomed/gb-2003-4-8-r50.txt
technical/biomed/cc350.txt
technical/biomed/bcr588.txt
technical/biomed/gb-2002-3-9-research0049.txt
technical/biomed/cc973.txt
technical/biomed/gb-2002-3-9-research0048.txt
technical/biomed/cvm-2-1-038.txt
technical/biomed/gb-2002-3-10-research0052.txt
technical/biomed/cvm-2-6-278.txt
technical/biomed/cvm-2-4-180.txt
technical/biomed/gb-2003-4-2-r9.txt
technical/biomed/bcr602.txt
technical/biomed/gb-2001-2-12-research0055.txt
technical/biomed/gb-2002-3-4-research0019.txt
technical/biomed/cc1547.txt
technical/biomed/gb-2002-3-4-research0018.txt
technical/biomed/gb-2001-2-12-research0054.txt
technical/biomed/ar104.txt
technical/biomed/gb-2003-4-2-r8.txt
technical/biomed/cvm-2-6-286.txt
technical/biomed/gb-2002-3-10-research0053.txt
technical/biomed/rr196.txt
technical/biomed/gb-2002-3-3-research0012.txt
technical/biomed/bcr571.txt
technical/biomed/gb-2000-1-2-research0003.txt
technical/biomed/gb-2002-3-8-research0039.txt
technical/biomed/bcr607.txt
technical/biomed/gb-2001-2-12-research0051.txt
technical/biomed/gb-2002-3-8-research0038.txt
technical/biomed/bcr570.txt
technical/biomed/gb-2002-3-10-research0056.txt
technical/biomed/cc343.txt
technical/biomed/rr191.txt
technical/biomed/gb-2002-3-3-research0011.txt
technical/biomed/gb-2002-3-10-research0054.txt
technical/biomed/gb-2001-2-12-research0053.txt
technical/biomed/gb-2003-4-4-r28.txt
technical/biomed/ar328.txt
technical/biomed/bcr605.txt
technical/biomed/cvm-2-4-187.txt
technical/biomed/gb-2001-2-3-research0007.txt
technical/biomed/bcr567.txt
technical/biomed/gb-2002-3-10-research0055.txt

```

This example shows how the `-v` command option can be useful when looking at file systems. This option could be handy when looking for files in a directory with lots of files named the same thing. By using the `-v` command, you can sort through the clutter of the other files to find the file you are looking for (granted the title of the file is fairly unique to the other files in the directory).

## `-w`
**Example One**
```
// the command
grep -e "genome" cat-one.txt -w > w-results-one.txt
```
```
// the final output
        In May 2002, two new mouse genome assemblies were
        released. One was the second version of the mouse genome
        using a whole-genome shotgun (WGS) strategy, but using
        220,000 contigs and covers 2.475 Gb of the mouse genome. By
        gaps in these chromosomes occupy 4.1% of genome in Cel2 and
        scaffolds/supercontigs of both the genome assemblies are
          it is highly recommended that the two mouse genome
          Comparison between the human genome golden path and
          The human genome project is well into the finishing
          mouse and human genomes. Comparing the human genome with
          the mouse genome can greatly help our understanding of
          the December 2001 golden path freeze of the human genome,
          MGSCv3 genome assemblies and 1,737,297 CSEs between the
          human and Cel2. Each CSE includes one human genome
          segment and one matched mouse genome segment. The
          human genome than CSEs from MGSCv3 assembly. Although the
          could be identified in the human genome from MGSCv3
          We compared the human genome regions covered by CSEs
          (Mb) (approximately 3% of the whole human genome) are
          than MGSCv3 in the whole mouse genome.
          inconsistency of CSE locations in the human genome from
            locations of CSEs in the human genome were compared
            the CSE in the human genome is in an intron region
            rat genome that matches the human location
            whole human genome, we found 6,259 NCBI-annotated novel
            RNA genes (from golden path April 2001 human genome
            genome location of this CSE, we discovered the
            genome hit multiple times in the other genome. We used
            duplications in the human genome relative to the mouse
            genome. The human and mouse regions in some of these
            collect a pair of human genome segments and one mouse
            genome segment. With length constraint of these
            segments against the newest human genome assembly (NCBI
            in the human genome may point to either true expansion
            functional regions in the genome, more than half of all
            accessed through the genome browser link at [ 18].
          Cel2 and MGSCv3 mouse genome assemblies were finished on
          CSE to search the whole human genome with BLAT. If this
          same as the percentage in the whole genome. The second
          approximately the same as in the whole genome. Under
        of one chromosome in the whole genome are available as
        of one chromosome in the whole genome

```

In this example, the `-w` option makes it so the new file contains lines that strictly have the given regular expression. This is different from using `-e` without `-w` because without `-w`, the file contains all instances of the regex and words containing the regex. This may be useful in cases where you're looking for lines that have a specific word in a file where there are many variations on the same word.

**Example Two**
```
// the command
grep -e "4" cat-one.txt -w > w-results-two.txt
```
```
// the final output
        gaps in these chromosomes occupy 4.1% of genome in Cel2 and
        4.5% in MGSCv3. The average size of the contigs, gaps and
          for Biotechnology Information (NCBI) ([ 4], 12 April,
          (11% in chromosome 18), AC023789 (85% in chromosome 4),
            whole chromosome (Figure 4), except one region around
          an 80-CPU LINUX cluster in 4 days. NCBI BLAST was used
          1.0e-4, and 1.0e-1) in this project. We found that 1.0e-1

```

This example shows how the `-w` option is useful for finding specific numbers. Without the option, the file would contain all instances of the number 4, including numbers like 40000 or 400. This may be useful for finding particular numbers or percentages in a long report.
