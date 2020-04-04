# Clustering Chemical Libraries

## Motivation
Chemical Libraries grow large very quickly.  When looking for new chemical matter, it's necessary to compare your current libraries to new matter which is typically done by comparing similarity with the Tanimoto Coefficient.  By doing this, one can find chemical matter that is different from your current library; but is close enough in similarity that it may potentially provide active lead for known targets.

At best, the job can be split but can still take a very long time.  One solution often sought is to look for a smaller set that represents your current stock for use in the Tanimoto based similarity search.  The basic idea used is to employ a clustering algorithm such as K-Means.  Several bioinformatics companies offer out of the box solutions which advertise being able to do millions of compounds such as Daylight or ChemAxon’s ‘MadFast Similarity Search’ with multi-threaded implementation and in-memory storage.

For those that don’t have access to expensive platforms, budget restricted or just learning, you can start with KNIME.

## Getting Started

Download the ones you wish to use or alter and change the necessary variables and nodes.

## Clustering

This example uses a slice of data ([PIK3CA_IC50_short](https://github.com/mgarard/KNIME/blob/master/Chemistry/Clustering%20Chemical%20Libraries/PIK3CA_IC50_short.csv)) of ‘small molecule-activity for kinase research’ from [(Sharma R, Schürer SC and Muskal SM. Dataset 1 in: High quality, small molecule-activity datasets for kinase research. F1000Research 2016, 5(Chem Inf Sci):1366](https://doi.org/10.5256/f1000research.8950.d124591)).  You may see more of this data in later uploads.

The process starts by loading the file and using a modified version of my [Dynamic File Name Creation]( https://github.com/mgarard/KNIME/tree/master/Dynamic%20File%20Name%20Creation).  Opening the file requires the ‘smiles’ data type to be set since it’s stored as a ‘string’.  Once loaded, the data is converted into a RDKIT object with the ‘RDKit From Molecule’.  This node requires a ‘molecule object’ which can be from a SD, SDF, mol, smiles, etc.

One aspect that I often see missed is the stripping of salts which is done with the ‘RDKit Salt Stripper’. We don’t want an oxalate salt to ruin a comparison to another molecule. Then the fingerprint is generated with the RDKit Fingerprint.  This has various options within it but if it does not have the fingerprint you want. You may chose to generate it elsewhere, save it as a string in a file and when it’s imported, the datatype can be set to ‘bit vector’ on the load.

Once you have the fingerprint data, it’s goes to ‘Distance Matrix Calculation’.  This has several options such as Euclidean, Manhattan, Cosine...  In this case, I’ve selected ‘Tanimoto’.  I convert the molecule back to smiles ( as a string data type ) for saving later and then the distance calculations are fed into the ‘k-Medoids’.

In ‘k-Medoids’, you can set the number of desired clusters, chunk size and constrain the number of iterations if you wish.  The top return port returns the data with each molecule binned to the row number of the center molecule for the cluster.  The bottom port returns the center molecule row number with a list of distances to other molecules in the cluster and number of molecules in the cluster.

The weekness of k-Medoids is that it's limited to 65,500 molecules.  It makes short work of this number but libraries often exceed this number.  The data then needs to be row split or partioned.  I like to use "Partioning" with random sampling at some percentage.  This may be able to be looped by placing a loop start before the partion, partioning by set number of rows and looping the second split back to aggregate the results.

Then the data is saved from various points to capture the flow.  The result that I obtained are included.

![Clustering Chemical Libraries](https://github.com/mgarard/KNIME/blob/master/Chemistry/Clustering%20Chemical%20Libraries/library_clustering.svg)
[Download Workflow](https://github.com/mgarard/KNIME/blob/master/Chemistry/Clustering%20Chemical%20Libraries/library_clustering.knwf)

## Taking it futher

1.	Clusters can be clustered.  Why is that important?  (see reading)
    A.	Maybe you clustered a set down to 5 clusters and that did not represent your data well and getting a larger diverse sets from cluster would help represent that library better than the cluster centers.  For example, from one cluster, you could find 5 more most diverse molecules within that cluster.
    B.	Maybe you want to screen more of your library against a target and want to know where to start.  You can add a known active to a set and see what cluster it falls into.  The cluster that contains the known active might give you the best results.
2.	Fingerprints can be combined.  Do you know which fingerprint is best?
    a.	One strategy has been to combine fingerprints
    b.	Clusters may differ between fingerprints
    c.	Do the zeros matter?  Hempel’s Raven has an answer.  (see reading) PCA can be use to find the elements of the fingerprint which is most important.


## Reading
[Database Clustering with a Combination of Fingerprints and Maximum Common Structure Methods]( https://pubs.acs.org/doi/abs/10.1021/ci050011h#)

[Euclidean chemical spaces from molecular fingerprints: Hamming distance and Hempel's ravens.]( https://www.ncbi.nlm.nih.gov/pubmed/25475496)

