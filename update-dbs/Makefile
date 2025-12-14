

DIR := ../db/kingdom
DIR_HMM := ../db/hmm
DIR_B := $(DIR)/Bacteria
DIR_A := $(DIR)/Archaea
DIR_V := $(DIR)/Viruses
DIR_M := $(DIR)/Mitochondria

EVID := 2
UNIPROT := ./UniProt-to-sprot.pl --evidence $(EVID) 
RMDUP := seqkit rmdup --by-seq

CP := cp -vf

.PHONY: install all

ALL := AMR sprot_b sprot_v sprot_a HAMAP.hmm.gz

all: $(ALL)

install : $(ALL)
	$(CP) HAMAP.hmm.gz $(DIR_HMM)/
	$(CP) AMR $(DIR_B)/
	$(CP) sprot_b $(DIR_B)/sprot
	$(CP) sprot_a $(DIR_A)/sprot
	$(CP) sprot_v $(DIR_V)/sprot
#	$(CP) sprot_m $(DIR_M)/sprot

HAMAP.hmm.gz : HAMAP.hmm
	pigz -9 < $< > $@

HAMAP.hmm : hamap_rules.dat rules_index.dat hamap_seed_alignments.tar.gz
	../bin/prokka-hamap_to_hmm 

hamap_rules.dat :
	wget https://ftp.expasy.org/databases/hamap/hamap_rules.dat

rules_index.dat :
	wget https://ftp.expasy.org/databases/hamap/rules_index.dat
	
hamap_seed_alignments.tar.gz :
	wget https://ftp.expasy.org/databases/hamap/hamap_seed_alignments.tar.gz

AMR: AMRProt.fa
	./AMRprot-to-AMR.pl $< > $@

AMRProt.fa :
	wget https://ftp.ncbi.nlm.nih.gov/pathogen/Antimicrobial_resistance/AMRFinderPlus/database/latest/AMRProt.fa

sprot_b : uniprot_sprot_bacteria.dat.gz
	 zcat $< | $(UNIPROT) | $(RMDUP) > $@

sprot_a : uniprot_sprot_archaea.dat.gz
	 zcat $< | $(UNIPROT) | $(RMDUP) > $@

sprot_v : uniprot_sprot_viruses.dat.gz
	 zcat $< | $(UNIPROT) | $(RMDUP) > $@

uniprot_sprot_bacteria.dat.gz :
	wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/taxonomic_divisions/uniprot_sprot_bacteria.dat.gz

uniprot_sprot_archaea.dat.gz :
	wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/taxonomic_divisions/uniprot_sprot_archaea.dat.gz

uniprot_sprot_virsues.dat.gz :
	wget https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/taxonomic_divisions/uniprot_sprot_viruses.dat.gz
			
clean :
	rm -f $(ALL)
