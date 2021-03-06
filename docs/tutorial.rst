ProteoClade 5 Minute Demo
=========================
Want to see ProteoClade in action? This tutorial provides a quick run through of both targeted and *de novo* workflows to demonstrate the tool's features.

Prepare Data
^^^^^^^^^^^^

#. `Install ProteoClade <introduction.html#installation>`_

#. `Download some example data <https://wustl.box.com/s/m3pf3dilnbmjz43k1j3id7s7d57uf8co>`_. Extract these files to a folder. "targeted_humouse_example.txt" is a truncated and reformated MaxQuant search from a patient-derived xenograft data set, while "denovo_bacteria_example.csv" is a truncated *de novo* PEAKS search from an oral microbiome data set.

#. Navigate to the folder with the example data, open a Python 3 shell, and import ProteoClade::

    >>> from proteoclade import *


#. Download and assemble taxonomy information from the NCBI::

    >>> download_taxonomy()


Targeted Database Example
^^^^^^^^^^^^^^^^^^^^^^^^^

#. Download protein sequence information from UniProt::

    >>> download_uniprot((9606,'sr'),(10090,'sr'), download_folder = 'pdxseq')
	#Downloads human and mouse proteomes by Taxon ID.

#. Create a PCDB for patient-derived xenografts:

    >>> create_pcdb('humouse', 'pdxseq')

#. Annotate the targeted experiment. Make sure to replace the XXXXXX with the date/name of the PCTAXA file you generated in step 4 of "Prepare Data".::

    >>> annotate_peptides('targeted_humouse_example.txt', 'humouse.pcdb', 'XXXXXX.pctaxa', taxon_levels = ('species','phylum'))

#. Roll up peptide information to gene symbols::

    >>> roll_up('annotated_targeted_humouse_example.txt')

**Results**: "rollup_annotated_targeted_humouse_example.txt" now contains genes derived from species-specific peptides and their summed ion intensities.

*de novo* Example
^^^^^^^^^^^^^^^^^

#. Download protein sequence information from UniProt::

    >>> download_uniprot((1891914,'a'),(1283313,'a'), download_folder = 'denovoseq')
	#Downloads strep oralis and alloprevotella proteomes by Taxon ID.

#. Create a PCDB for patient-derived xenografts:

    >>> create_pcdb('bacteria', 'denovoseq')
	
#. Annotate the *de novo* experiment. Make sure to replace the XXXXXX with the date/name of the PCTAXA file you generated in step 4 of "Prepare Data".::

    >>> annotate_denovo('denovo_bacteria_example.csv', 'bacteria.pcdb', 'XXXXXX.pctaxa', taxon_levels = ('species','phylum'))

#. Roll up peptide information to gene symbols::

    >>> roll_up('annotated_denovo_matched_denovo_bacteria_example.csv')
	
**Results**: 'annotated_denovo_matched_denovo_bacteria_example.csv' contains species and phyla annotations for the *de novo* data set, while 'rollup_annotated_denovo_matched_denovo_bacteria_example.csv' contains peptides summed to gene symbols. Note that although this *de novo* data set does not contain quantitative information, spectral counts are provided in additional columns.