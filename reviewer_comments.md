* Referee: 1

** Comments for the Author
1.The manuscript need to be shorten considerably and proper flow to be maintained.
**** not sure if I aggree, but I will definitly try to imrove readability
RESPONSE:
We have considered shortening the manuscript, but have found that difficult.  As the method relies on two previously undocumented biological findings regarding the rDNAs (namely, the within-genome uniqueness and the across-genomes conservation), we are forced to sustantiate these claims prior to detailing our method. We have attempted to strealine the implementation and descriptions of the software components for better flow, and revised the paper throughout.

2.Reproduction of the method is difficult from the written method. Needs the method to be simplified.
**** added run script + config, added to conda
RESPONSE:  While the reviewer clearly made no effor to reproduce the written method, the pipeline has been reconfigured to be run with a single script instead of the three steps of preprocessing, operon prediction, and de fere novo assembly.  Further, it can now be installed using "conda", which sorts out all the dependencies.

3. It is known fact that flanking regions of rDNA are conserved in a genome, so any other novelty used in this strategy?
**** No, this is just not true.  Citation please.
RESPONSE:  We would respectfully ask that the reviewer would substantiate this claim with a citation, as this does not appear to be the case.  Further, we perform important charactarization of this finding across a range of bacterial species.  Apart from the novel software package we are reporting, we are the first (to our knowledge) to use this "known fact" to improve genome assembly.

4. The article is already available online: http://www.biorxiv.org/content/early/2017/07/14/159798 (doi.org/10.1101/159798) at bioRxiv. Therefore I am not sure if it violates the NAR policy.
**** if you agree to review, read the policies of the journal before making this 25% of your critique of the paper
RESPONSE:  I am surprised that the reviewers have no means of accessing NAR's policy, perhaps by performing a web search of the string "Nucleic Acid Research preprint".


* Referee: 2

** Comments for the Author
*** General Comments:
This manuscript describes a DNA sequence assembly pipeline, riboSeed, that addresses a key problem in de novo microbial genome assembly with short read sequences — repeat regions. riboSeed implements a so-called “de fere novo” method of sequence assembly by first recruiting reads to the ribosomal operons of a closely related (and already completed) prokaryotic genome. Reads that recruit to the ribosomal operons and flanking regions (1 Kbp to the left and right of each operon) are then co-assembled with that region. The resulting contigs from each region are then stitched together with 5 Kbp of ambiguous (N) bases creating a “pseudocontig”. The pseudocontig is then refined by iteratively recruiting reads and reassembling. Once (if) the pseudocontig is good enough it is then co-assembled with the raw reads as either a SPAdes “trusted” or “untrusted” contig, depending on the quality of the pseudocontig.

In simulated and real data the riboSeed de fere novo method out-performed the de novo assembly method. While that result is good the see, it isn’t necessarily surprising, as reference-based assemblies should always out-perform de novo assemblies in repeat regions.

RESPONSE: We agree, but  being able to improve asembly from only using only 1/300th of the reference (in the case of E. coli, for instance) in a hands-free approach, highlighting the relatively reference agnostic nature of the method might be considered bemusing at least, if not surprising.

Overall I think the manuscript is well-written, there are however a few tables and figures I would like to see revised and improved (see specific comments). Also, the first two subsections of the Results section seem to be more Materials and Methods than actually reporting results. I appreciate that the authors report other genome polishing tools for gap closure, but I wish some tool comparisons were done so I could understand how much better riboSeed is doing compared to the other tools. I think the concept of “de fere novo” is humorous and makes sense to me, but I don’t see that sticking in the community, as the term “reference-based” is already widely accepted for this methodology (granted this method is a bit more de novo than most reference-based methods).

REPONSE:  We thank the reviewer for their kind words! We felt (as still feel) that the novelty of characterizing the rDNA flanking regions within genomes and  within taxa warrented their place in the results section, as the proceeding findings hinge on the reliability of those claims. As for the tables, we are considering combining them as a single table with subsections;  effor was made to ensure that the column headings stayed the same across the experiments, as differing headers might mislead the reader.  "De fere novo" may never stick, but we believe in the importance of an actuarate ontological description of a process that would be miss-represented by calling it either "reference-based" or "reference-free" due to the diregard riboSeed has for roughly 99.7% of the reference.

I regret that I do not have sufficient time to download and use the riboSeed code on a test dataset, but I will commend the authors for putting together a clear and well-documented GitHub repository. As a casual observer I’m a bit intimidated by how many parameters and scripts there are in the code base, but from the perspective of a researcher who is desperate to close a bacterial genome I think I would be willing to forgo time spent tinkering with all of the parameters if it helped the assembly. riboSeed seems less automated than most reference-based assembly pipelines.

RESPONSE:  We appreciate the reviewers apraisal of the Ducmentation found on the GitHub repo. Additionally, we have made use of Readthedocs.io to host even more documentation.

Lastly, I have left several specific comments below, some of which are minor, others are a little more important. The largest conflicts I could find in the methods/implementation of riboSeed is:

(1) Likely the most important decision a riboSeed user has is “which reference should I use” and the authors don’t give a great deal of advice on that subject (though KGCAK is recommended). They do demonstrate that selecting the (obviously wrong) reference can lead to a poor assembly (Fig. 3), but I would like to see a more specific set of instructions for users to follow to get a good reference sequence (maybe using phylogeny?). Also, how easy is it to know that a genome has all rDNA regions assembled and known to be in the right context/order? Seems like there’s a need for tools/methods to verify that more easily (though the authors do mention 16Stimator).

RESPONSE:  This is an important critique and we thank the reviewe for raising it.  We have  noted a few suggestions in the Materials sand methods section, where our recommendation is as follows.  We tried to kreat


  have used Kraken construct a "suggestion" database consisting of a representative of each of the complete prokaryotic genomes found in NCBI.  The users can, after installing kraken, find the closest full genome n the database to their sequence, and use that as a reference.  We do not include the database in the repository as it would exceed the policies of Github.com.

(2) The use of the reference genome’s ribosomal operon as a “trusted contig” in the initial SPAdes assembly. It’s still unclear to me how SPAdes handles trusted (and untrusted) contigs and I’m also confused by why they recommend users not to feed in contigs of “related species”, which is what the pipeline does. Maybe they’re worried that with a related reference the assembly will drift away from the sample’s genome and toward that reference. That might not be an issue for riboSeed because the pseudocontigs are later re-assembled iteratively by only the reads. Would want to see how (or even if) using the reference genome as a trusted contig helps or hurts the results.

RESPONSE:  This, too, is a very valid critique. We have included in the supplementary materials data regarding the use of this parameter.  We hope that by showing the effect of using "trusted-contigs", users will feel sufficiently informed to chose the appropriate option.

RESPONSE, cont: The reasons for using reference as either "trusted or untrusted" by default include the difficulty spades has with assembling reads mapping to rDNAs de novo.  One of the issues is that of the coverage depth at the rDNA.  Becuase the mapping scheme allows multiple mappings, the non-polymorphic regions of the rDNA will have higher coverage than the regions that differentiate between rDNAs.  Because of this, coverage correction is not enabled for the subassemblies, as SPAdes often returns an error; at the advice of the authors, the parameters for subassembly spades runs are to use sinsgle-cell mode (as single cell data shares some of the coverage issues).



*** Specific Comments:

Pg. 1 Line 30 (abstract): Need to italicize “de novo”
**** cannot fix, due to NAR template
RESPONSE: We would respectfully request that the NAR editors make this edit upon acception to the journal, as the NAR template does not appear to allow italics in the abstract environment.

Pg. 1 Line 34 (intro): “Sequencing of the 16S ribosomal region is widely used to identify bacteria and explore […]” “Bacteria” should probably be “prokaryotes”.
**** changed
RESPONSE: Addressed

Table 1: Why are only two dates shown? They’re relatively close to one another, too. Might be better to include annual counts that go back to, say, 2007. This should demonstrate that the gap between the number of complete genomes and total genomes sequenced begins to rapidly increase after short read sequencing comes into play.
**** changed to a plot reflecting the data
RESPONSE: We have changed table one to be a small figure showing the counts of genome assembly levels between 2000 and 2017.

Fig. 1: I’m assuming when the pseudocontigs are being stitched together that the order of the rDNA operons will be correct. No mention of the order of the operons. Is that not important?
**** It isn't important, as we are only considering the flanking regions, and I noted this under the de fere novo assembly section.
RESPONSE: We have made a note in the riboSeed.py subsection confirming the reviewers assumption about the importance of the order of the pseudocontigs. .


Pg. 3 Line 48: Not sure what “default size 1 Kbp” is referring to. The flanking region? Need to be a bit more clear here.
**** added comment
RESPONSE:  The sentence now reads "Reads that map to each annotated rDNA and its flanking regions (where the flanking regions consist of 1kb upstream and 1kb downstream of the rDNA, by default) are ..."

Pg. 3 Line 51: Why use the reference rDNA as trusted contigs? Did you see beter results when testing with/without this method? It’s still unclear to me how the trusted contigs are utilized by SPAdes. I assume k-mers are generated by the trusted contigs and just thrown in the pool of available k-mers during de novo assembly. Interestingly the SPAdes manual says not to use contigs of related species: “--trusted-contigs: Reliable contigs of the same genome, which are likely to have no misassemblies and small rate of other errors (e.g. mismatches and indels). This option is not intended for contigs of the related species.” I'm not entirely certain why they have this disclaimer in the manual. I’d like to see how well riboSeed performs without the trusted contigs.
**** an excellent point!  this is a heuristic (see note on setting this parameter)
RESPONSE:  We again take the opportunity to thank the reviewer for the insightful comment.  The short answer is that performance is


Pg. 3 Line 55: This isn’t particularly important, but why a 5 Kbp stretch of N’s? Seems excessive. Wouldn’t a 500 bp stretch of N’s suffice? Also, be consistent, to this point you typically write “kbp” but here you write “kb”.
**** Another excellent point.  Reduce to 1kb maybe.  Also, wiki says kb, not kbs?
RESPONSE:  We have changed the spacer legnth to 1kb.  The reviews comment is noted, but the authors feel that using 1kb while being in effect no different from a 500bp spacer, helps readers understand purpose of the spacers to be exceedingly longer than spannable by a read from illumina or other short read technology.  Further, wikipedia seems to prefer the suffix "kb", and the manuscript has been changed to reflect that throughout.


Pg. 3 Line 52: Go ahead and specify “BLASTn” here.
**** done


Fig. 2: In part (b) what are “scannedScaffolds” in the title of the plot? I think you want to have more precise titles for both of these plots. Or just drop them and let the (a) and (b) labels explain what they are.
**** will dropped the labels

Pg. 4 Line 60: “softare” -> “software”
**** changed

Pg. 6 Line 20: “adoped” -> “adopted”
**** changed

Fig. 5: What is the y-axis? Number of rDNA’s assembled? Need to label this better. Also the y-axis limits are different between (a) and (b). Both should be ylim=c(0,7).
****  good point;  limits have to do woth total number of rDNAs, but still a good point

Table 3: What does “skipped assembly” mean? Also, I would recommend re-configuring this table (a table with only one row isn’t much of a table). Maybe row one is de novo and row two is de fere novo?
**** should I redfine?  This is addressed in the "Validating assembly acrouss rDNA regions section",  I like keepiung the table structure consistent, even if only one rwo is shown.  Perhaps we make this subtables?

Table 4: You display the SNP type in the table, but don’t discuss the significance of them. Is it worth reporting?
**** not sure what they mean by significance? if they mean statistical significance, than I don't think this table was understood.  Will try to clarify

Pg. 7 Line 47: What is USA200?
**** added the word lineage?

Pg. 7 Line 27: How low for GC content and why? If low-GC content is a challenge, wouldn’t high-GC content be a challenge as well because of the low-complexity?
**** arent low GC bits hard to sequence?  I thought that was the reaosn

Table 5: Would recommend re-configuring this table as well.
**** see above

Pg. 8 Line 22: “where the rDNA regions to act as […]” -> “where the rDNA regions act as […]”
**** addressed
