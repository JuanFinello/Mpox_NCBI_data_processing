# Mpox NCBI data processing

Takes a list of mpox accessions from NCBI and turns the records into a curated,
submission-ready dataset: sequences in FASTA, metadata in a tabular template, with the
fields that databases require checked rather than assumed.

## What the notebook does

1. **Download.** Fetches the GenBank record for every accession in `sequences.csv` through
   Entrez, and writes them to a single `.gb` file.
2. **Parse.** Reads the GenBank records into a dataframe: isolate, collection date,
   country, host, authors, submitting lab, and the sequence itself.
3. **Check.** Flags records with missing or malformed metadata, so that an incomplete
   record is visible rather than silently carried forward.
4. **Split.** Writes the metadata to TSV and the sequences to FASTA, keyed by isolate.
5. **Reshape.** Normalises dates, fills the fields the destination template requires, and
   produces the metadata table in the expected format.

The interesting part is step 3. Sequence records are only as useful as the metadata around
them, and a collection date or a country that is missing, ambiguous or written in a local
format is the difference between a usable record and a dead one.

## Stack

Python · pandas · Biopython (SeqIO, Entrez) · openpyxl
