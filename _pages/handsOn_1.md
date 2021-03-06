---
title: "Hands On"
layout: archive
permalink: /handsOn_1/
---  

# Kmer analysis: running jellyfish <a name="where-are-we?"></a> 

Right, so today you have heard about how to analyse kmer composition of your genome sequenced reads! Now you are going to put your 'Hands On' data and will, yourself, count and analyse kmers.
You have to chose between the species: (i) *Vanessa atalanta*, (ii) *Pieris rapae* and (iii) *Notonda dromedarius* to work on until the end of the week. Once you have chosen it, create a folder where you will run your analysis. My suggestion would be to create a folder with your species name and inside it, a series of other folders to structure your analysis. For example:

v_atalanta/

 v_atalanta/kmers/
  
 v_atalanta/assembly/
  
 The above basically means you have created a folder called 'v_atanta' (if you choose vanessa atalanta as your working species) and inside it you have created two other folders side by side called 'kmers' and 'assembly'. If you would like to do that, then do:
 
 ```console  
mkdir <species_folder>
cd <species_folder>
mkdir kmers
mkdir assembly
```  
 
Great, now you need to go and copy the working data for your species. The data will be inside /home/ubuntu/Share/Data

```console  
cd /home/ubuntu/Share/Data
ls -ltr
```  
Do you see the 3 species folders? If you do, then go inside your species folder and inside 'kmers' and copy the file that contains \*600.fasta on its name to your kmer folder. One example:


```console  
cp /home/ubuntu/Share/Data/v_atalanta/kmers/ilVanAtal1.600.fasta <Path_to_your_folders>/v_atalanta/kmer/
ls -ltr
```  

### Attention :grey_exclamation: 

Each species data will have a code on it's name representing each of the 4 species we will work on during this week, this is the code they have at the Darwin Tree of Life Project. The codes are:

ilVanAtal1 - *Vanessa atalanta*

ilPieRapa1 - *Pieris rapae*

ilNotDrom1 - *Notodonta dromedarius*

iHemFuc1 - *Hemaris fuciformis* (we are not going to use this species in the current analysis)


Keep these codes in mind as the files will most likely to be named after them.

Now that you have the reads in place, let's activate the conda environment `eukaryotic_genome_assembly`, where most of the softwares that we'll use over this workshop have already been installed by your instructors:  

```bash  
conda activate eukaryotic_genome_assembly
```

If the activation has worked, your should see a `(eukaryotic_genome_assembly)` before your prompt. Something like this:  
```bash  
(eukaryotic_genome_assembly) userX@IP-address:working_directory$
```

After activating the conda environment, try calling Jellyfish:

```console  
jellyfish
``` 

Do you see the help message? Great! (If not, call Joāo!)

Jellyfish has many steps. The first one we want to run is the *count* to count our genome reads kmers. The kmer size we are going to use is 31. 

1-) Jellyfish count

So here comes the command:

```console  
jellyfish count -C -m 31 -s 1000 -t 1 -o <species>.jf <species>.fasta
``` 


### Attention :grey_exclamation: 

In the command line above you see \<species\>.fasta. This needs to be replaced by the fasta file of the species you have chosen. This is just a generic way to point out that a fasta file must be imputed in that position in the command line above. The same with the \<species\>.jf. If your species is Vanessa atalanta, you can choose to have the output (-o) called v_atalanta.jf and this will be your output name.

### More

Please try:

```console  
jellyfish count --help 
```

on your command line to understand what are the parameters you have imputed in the previous line. Help messages are a useful way for you to understand what you are running, and to see if you would like to add any other parameter for your specific analysis case. Also remember that beyond this course, the internet is always on your side. If you google ‘jellyfish user guide’, for example, you will find [JellyfishUserGuide]( http://www.genome.umd.edu/docs/JellyfishUserGuide.pdf)

### Back

2-) Jellyfish histo

Now that you have counted your 31-letters kmers, we want to transform it to a histogram file so you can plot it. Then you have your second command:


```console  
jellyfish histo <species>.jf > <species>.histo

```

### Attention :grey_exclamation: 

Note in the command above we have used the symbol “>” before the output. This is a command line symbol that will redirect your output to a file instead of printing it to the screen.

Note that the input for your command **jellyfish histo** is the output from your previous command, that was **jellyfish count**. Now you have the necessary result to plot a histogram on genomescope and have a look at the distribution of your genome kmers. 

# BUT STOP!

...

Before you plot this result, I want you to plot a genomescope plot for another file FIRST!! 

Inside the 'assembly' folder for your species, you are going to find two files called:

```console  
<species>.ccs.total.fasta.gz
<species>.total.histo
```

Download the file \<species\>.total.histo to your local machine (as you learned yesterday), go to the [Genomescope](http://qb.cshl.edu/genomescope/) page and upload the file there. You should change the **Description** to the name of your species, and the **kmer** to 31. Then plot.

Save the image of both versions of the plot - normal and log scale - somewhere in your computer.

Cool, you have plotted the kmer distribution of the \<species\>.total.histo file, and genomescope has calculated for your (i) the estimated genome size, (ii) the heterozygosity and (iii) the percentage of repeats of your species genome. 

The histogram you have just plotted is for a jellyfish count of the total PacBio HiFi reads sequenced to assemble your species. I have generated it for you because it takes time (but to generate it outside this course you would run the same commands you ran for the subsample set).

Now, I would like to you generate statistics for the \*.total.fasta reads. I have a script for you to do that. It's called asmstats.

Before running asmstats, you need to add the directory where the script is located (`/home/ubuntu/softwares/scripts`) to your environment variable:  

```bash  
export PATH=$PATH:/home/ubuntu/softwares/scripts
```  

Then check if the directory has been added to your environment variable:  

```bash  
echo $PATH
```

If you can see the `/home/ubuntu/softwares/scripts` in the output of the previous command, you should be ready to run the asmstats script:

```console  
asmstats <species>.ccs.total.fasta.gz > <species>.total.fasta.stats
```

Note: We are calling your output file \<species\>.total.fasta.stats

Once the command finishes to run, have a look at the output:

```console  
less <species>.total.fasta.stats
```

Also: use this tutorial to plot your reads length distribution: [Plot reads](https://github.com/eukaryotic-genome-assembly/eukaryotic-genome-assembly.github.io/blob/master/_pages/handsOn_plotReadLength.md)

You have now generated the kmer plot, reads statistics and reads plot distribution for your complete PacBio HiFi read set. Which conclusions can you now draw from these results?


a- What is the expected genome size?

b- What is the estimated heterozygosity?

c- What is the estimated repeat content?

d- Taking into consideration the estimated genome size, and the statistics of the total reads you have just generated, how much reads coverage you have to assemble this genome?


Now, 

I want you to go back to the *.histo* file you have generated today, download it to your local machine and plot it on genomescope. Also, I want you to run asmstats on the fasta file (the 600 subset) you imputed to jellyfish. 

e- What genomescope tells you when you try to plot a kmer histogram for just a handful of sequences? 

f- Looking at the asmstats result for this smaller file, how much coverage of the genome you have in this file giving the estimated genome size?

Great. Now let’s look at it all together: considering the kmer plot of the total file and its statistics, we can have a good look at the kmer composition of the DNA in the genome. How does it look? Discuss with your partners and then later in the big group.  


