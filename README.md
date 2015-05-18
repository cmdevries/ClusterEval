# ClusterEval
Cluster Quality Evaluation Software

ClusterEval implements novel and standard measures for the evaluation of
cluster quality. This software has been used at the INEX XML Mining track [a]
and in the MediaEval Social Event Detection task [b].

[a] http://inex.otago.ac.nz/tracks/wiki-mine/wiki-mine.asp

[b] http://www.multimediaeval.org/mediaeval2013/sed2013/

Introduction
============

The goal of clustering is to place similar objects into groups or clusters and
place disimilar objects into different clusters. These objects can range from
written text as documents, to visual information as images, to sensor data from
spacial tracking sensors, to customer profiles as age, height, shopping
history, etc. Automated methods generate groupings using a variety of different
data representations and algorithms. The goal of extrinsic cluster evaluation
is to determine how well these groupings match with human constructed views of
clusters. Therefore, an automated approach produces a better quality clustering
when it better matches the human view of what constitutes a cluster. This
extrinsic human generated view of clusters is referred to as the ground truth.

There are many different approaches to evaluating the quality of a clustering.
Cluster evaluation can be either intrinsic or extrsinic. Intrinsic or internal
evaluations measure how well cluster optimises the representation it uses to
determine clusters. However, this software evaluates clusters extrinsically
using external information about clusters. In version 1.0 clusters are
evaluated using the classical classes-to-clusters approach, where a set of
classes or categories determined by humans are used to determine how well a
clustering matches this pre-defined classification. Future releases will
include extrinsic evaluation using ad hoc queries as presented by De Vries et.
al. [1].

Usage
=====

The program will display its usage when given no parameters. A more detailed
explanation of all the options, inputs and outputs are described in the
following sections. For example, the following command with display how to use
the program.

    $ ./cluster_eval.py

By default the program evaluates using the NMI measure. Included with this
program are ground truths from the INEX 2010 XML mining track. A multiple label
ground truth is contained in inex10\_multiple\_label.txt. It has been converted
to single label by taking only the first label for each document in
inex10\_single\_label.txt. Only the first 10,000 single label documents are
contained in inex10\_first\_10000\_single\_label.txt. A clustering generated
using TopSig document signatures [3] is contained in inex10\_clusters\_50.txt.
The following example evaluates this clustering using the multiple label ground
truth from the INEX 2010 XML Mining track.

    $ ./cluster_eval.py inex10_multiple_label.txt inex10_clusters_50.txt

File Formats
============

This programs takes ground truth sets of categories and clusterings in the same
format. Each of these inputs assign class labels, categories or clusters to an
object identifier. In the case of documents the object identifier is a document
identifier such as the Wikipedia document ID. For example, a Wikipedia document
with ID 19723050 [c] belongs to the three categories “Culture Musicalculture
People”. This is represented as a single line of space seperated strings as
illustrated in the following example. The first token is the Wikipedia document
ID and all remaining tokens are categories that this document belongs to.

[c] http://en.wikipedia.org/wiki?curid=19723050

    19723050 Culture Musicalculture People

All inputs follow the general format illustrated below. An input file consists of 1
or more lines in the following format. Each of the tokens are treated as a string.
Therefore, the object identifiers and labels do not strictly have to be numeric.
They can be any valid string.

    <object identifier> <label 1> <label 2> ... <label n>

An object identifier can be repeated on multiple lines. This results in a state
where all the labels will be assigned to the object identifier. The following
example results in the same internal state of the program as the first single line
example.

    19723050 Culture
    19723050 Musicalculture People

Cluster submissions follow the same format. The following example assigns
Wikipedia document ID 19723050 to cluster 5.

    19723050 5

Cluster submissions can also be multiple label. The following example assigns
Wikipedia document ID 19723050 to clusters 5 and 10.

    19723050 5 10

The ground truth or clustering or both can be multiple or single label. The
program reports which inputs are single and multiple label.

Measures
========

This program implements many standard evaluation measures. The default measure
if not specified to the program is the NMI measure. There are options for each
of the measures as described in the usage. For a complete description and
analysis of the measures please see De Vries et. al. [1].

*Purity* measures the fraction of each cluster that is the majority class label
in the ground truth. Purity ignores class labels that are not the majority. All
the following measures do not.

*Entropy* and *Negentropy* combine probabilities for each class label in the
ground truth that exist in a cluster using the entropy measure. The probability
for a class label is the fraction of the clusters that is the given class
label.

*F1* compares each pair of documents to and combines them using the harmonic
mean.

The *Old F1* measure transforms the clustering into a classification using the
majority class label from the ground truth. This measure was used in the 2009
and 2010 INEX XML Mining track.

*NMI* compares the ground truth and clustering in an information theoretic sense
that makes a trade-off between the number of clusters and quality.

*Divergence from a Random Baseline* augments any measure of cluster quality to
account for ineffective or pathological clusterings. It can differentiate
clusterings of no use, such as assigning each document to its own cluster. In
this case the purity score is at its maximum at 1 but when augmented using this
approach, it is adjusted to 0.

Credits
=======

This software has been written by Chris De Vries between 2009 and 2013, and
has been supported by the Queensland University of Technology Deputy Vice
Chancellor’s and Australian Postgraduate Award scholarships. Furthermore,
many people have helped in the formation and correction of ideas implemented
in this software.

Persons who have contributed:
- Shlomo Geva
- Sangeetha Kutty
- Richi Nayak
- Peer reviewers of related published material
- Participants at INEX and MediaEval

References
==========

[1] Christopher M De Vries, Shlomo Geva, and Andrew Trotman. Document
clustering evaluation: Divergence from a random baseline. Workship Information
Retrieval, WIR 2012, Dortmund, Germany, 2012.

[2] Christopher M De Vries, Richi Nayak, Sangeetha Kutty, Shlomo Geva, and
Andrea Tagarelli. Overview of the inex 2010 xml mining track: Clustering
and classification of xml documents. In Comparative Evaluation of Focused
Retrieval, pages 363–376. Springer, 2011.

[3] Shlomo Geva and Christopher M De Vries. Topsig: topology preserving document
signatures. In Proceedings of the 20th ACM international conference
on Information and knowledge management, pages 333–338. ACM, 2011.
