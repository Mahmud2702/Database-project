# Database-project
use temp1;
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255),
    role VARCHAR(50),
    registration_date DATE,
    affiliation VARCHAR(100)
);


CREATE TABLE species (
    species_id INT AUTO_INCREMENT PRIMARY KEY,
    scientific_name VARCHAR(100),
    common_name VARCHAR(100)
);

CREATE TABLE genes (
    gene_id INT AUTO_INCREMENT PRIMARY KEY,
    gene_symbol VARCHAR(50),
    gene_name VARCHAR(100),
    species_id INT,
    FOREIGN KEY (species_id) REFERENCES species(species_id) ON DELETE CASCADE
);

CREATE TABLE mutations (
    mutation_id INT AUTO_INCREMENT PRIMARY KEY,
    gene_id INT,
    user_id INT,
    mutation_type VARCHAR(50),
    nucleotide_change VARCHAR(50),
    amino_acid_change VARCHAR(50),
    chromosome VARCHAR(10),
    position INT,
    reference_allele VARCHAR(50),
    alternate_allele VARCHAR(50),
    start_position INT,
    end_position INT,
    date_submitted DATE,
    FOREIGN KEY (gene_id) REFERENCES genes(gene_id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

CREATE TABLE disorders (
    disorder_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    description TEXT,
    category VARCHAR(100)
);

CREATE TABLE mutation_disorder_link (
    mutation_id INT,
    disorder_id INT,
    PRIMARY KEY (mutation_id, disorder_id),
    FOREIGN KEY (mutation_id) REFERENCES mutations(mutation_id) ON DELETE CASCADE,
    FOREIGN KEY (disorder_id) REFERENCES disorders(disorder_id) ON DELETE CASCADE
);


CREATE TABLE predictions (
    prediction_id INT AUTO_INCREMENT PRIMARY KEY,
    mutation_id INT,
    tool_name VARCHAR(50),
    score FLOAT,
    interpretation VARCHAR(50),
    FOREIGN KEY (mutation_id) REFERENCES mutations(mutation_id) ON DELETE CASCADE
);


CREATE TABLE homologous_mutations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    mutation_id INT,
    homologous_to_id INT,
    homology_type VARCHAR(50),
    note TEXT,
    FOREIGN KEY (mutation_id) REFERENCES mutations(mutation_id) ON DELETE CASCADE,
    FOREIGN KEY (homologous_to_id) REFERENCES mutations(mutation_id) ON DELETE CASCADE,
    CHECK (mutation_id <> homologous_to_id)
);


INSERT INTO users (user_id, name, email, password, role, registration_date, affiliation) VALUES
(1, 'Nazmul', 'nazmul@genome.org', 'hashed_pw1', 'Researcher', '2023-02-10', 'Genome Research Lab'),
(2, 'Swadhin', 'swadhin@medcenter.com', 'hashed_pw2', 'Clinician', '2023-03-15', 'MedCenter'),
(3, 'Tonmoy', 'tonmoy@cancerinstitute.org', 'hashed_pw3', 'Geneticist', '2023-04-20', 'Cancer Institute'),
(4, 'Siam', 'siam@unibiotech.edu', 'hashed_pw4', 'Professor', '2023-05-12', 'UniBiotech'),
(5, 'Evan', 'evan@biocore.org', 'hashed_pw5', 'Reviewer', '2023-06-18', 'BioCore'),
(6, 'Farhan', 'farhan@neurogen.org', 'hashed_pw6', 'Researcher', '2023-07-25', 'NeuroGen Labs'),
(7, 'Foysal', 'foysal@nhs.uk', 'hashed_pw7', 'Clinician', '2023-08-30', 'NHS Trust'),
(8, 'Tamim', 'tamim@genomics.edu', 'hashed_pw8', 'Scientist', '2023-09-05', 'Genomics Institute'),
(9, 'Ronaldo', 'CRronaldo7@oncology.org', 'hashed_pw9', 'Oncologist', '2023-10-11', 'Oncology Network'),
(10, 'Messi', 'messi10@bioinformatics.ai', 'hashed_pw10', 'Bioinformatician', '2023-11-21', 'AI Bioinformatics Hub');



INSERT INTO species (species_id, scientific_name, common_name) VALUES
(1, 'Homo sapiens', 'Human'),
(2, 'Mus musculus', 'Mouse'),
(3, 'Rattus norvegicus', 'Rat'),
(4, 'Danio rerio', 'Zebrafish'),
(5, 'Drosophila melanogaster', 'Fruit Fly'),
(6, 'Caenorhabditis elegans', 'Nematode'),
(7, 'Arabidopsis thaliana', 'Thale Cress'),
(8, 'Saccharomyces cerevisiae', 'Yeast'),
(9, 'Gallus gallus', 'Chicken'),
(10, 'Canis lupus familiaris', 'Dog');


INSERT INTO genes (gene_id, gene_symbol, gene_name, species_id) VALUES
(1, 'BRCA1', 'Breast cancer type 1 susceptibility protein', 1),
(2, 'TP53', 'Tumor protein p53', 1),
(3, 'CFTR', 'Cystic fibrosis transmembrane conductance regulator', 1),
(4, 'APOE', 'Apolipoprotein E', 1),
(5, 'EGFR', 'Epidermal growth factor receptor', 1),
(6, 'KRAS', 'Kirsten rat sarcoma viral oncogene homolog', 1),
(7, 'MLH1', 'MutL homolog 1', 1),
(8, 'MYC', 'MYC proto-oncogene', 1),
(9, 'PTEN', 'Phosphatase and tensin homolog', 1),
(10, 'VEGFA', 'Vascular endothelial growth factor A', 1);


INSERT INTO mutations (mutation_id, gene_id, user_id, mutation_type, nucleotide_change, amino_acid_change, chromosome, position, reference_allele, alternate_allele, start_position, end_position, date_submitted) VALUES
(1, 1, 1, 'Frameshift', 'c.68_69delAG', 'p.Glu23Valfs', '17', 43045629, 'AG', '-', 43045629, 43045630, '2023-02-11'),
(2, 2, 2, 'Nonsense', 'c.637C>T', 'p.Arg213*', '17', 7579472, 'C', 'T', 7579472, 7579472, '2023-03-18'),
(3, 3, 3, 'Deletion', 'c.1521_1523delCTT', 'p.Phe508del', '7', 117199644, 'CTT', '-', 117199644, 117199646, '2023-04-22'),
(4, 4, 4, 'Missense', 'c.388T>C', 'p.Cys130Arg', '19', 44908684, 'T', 'C', 44908684, 44908684, '2023-05-14'),
(5, 5, 5, 'Missense', 'c.2369C>T', 'p.Thr790Met', '7', 55259515, 'C', 'T', 55259515, 55259515, '2023-06-20'),
(6, 6, 6, 'Missense', 'c.35G>A', 'p.Gly12Asp', '12', 25398284, 'G', 'A', 25398284, 25398284, '2023-07-27'),
(7, 7, 7, 'Splice Site', 'c.790+1G>A', 'Unknown', '3', 37034841, 'G', 'A', 37034841, 37034841, '2023-08-31'),
(8, 8, 8, 'Amplification', 'NA', 'Overexpressed', '8', 128748357, 'NA', 'NA', 128748357, 128748357, '2023-09-06'),
(9, 9, 9, 'Deletion', 'c.388C>T', 'p.Arg130Ter', '10', 89623195, 'C', 'T', 89623195, 89623195, '2023-10-12'),
(10, 10, 10, 'Gain-of-function', 'c.2578C>T', 'p.Pro860Leu', '6', 43745678, 'C', 'T', 43745678, 43745678, '2023-11-22');


INSERT INTO disorders (disorder_id, name, description, category) VALUES
(1, 'Hereditary Breast and Ovarian Cancer', 'Associated with BRCA1 and BRCA2 mutations.', 'Cancer'),
(2, 'Li-Fraumeni Syndrome', 'A rare inherited cancer syndrome linked to TP53 mutations.', 'Genetic Syndrome'),
(3, 'Cystic Fibrosis', 'A recessive genetic disorder affecting lungs and pancreas.', 'Respiratory'),
(4, 'Alzheimer’s Disease', 'Neurodegenerative disease associated with APOE ε4.', 'Neurological'),
(5, 'Non-Small Cell Lung Cancer', 'Cancer often involving EGFR mutations.', 'Cancer'),
(6, 'Colorectal Cancer', 'Linked to MLH1 mismatch repair gene defects.', 'Cancer'),
(7, 'Multiple Endocrine Neoplasia', 'Involves mutations in RET or KRAS genes.', 'Endocrine'),
(8, 'Prostate Cancer', 'Linked to PTEN deletions and BRCA mutations.', 'Cancer'),
(9, 'Glioblastoma', 'Often involves MYC amplification and TP53 mutations.', 'Neurological'),
(10, 'Macular Degeneration', 'Vascular disorder linked to VEGFA variants.', 'Ophthalmologic');

INSERT INTO mutation_disorder_link (mutation_id, disorder_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 7),
(7, 6),
(8, 9),
(9, 8),
(10, 10);

INSERT INTO predictions (prediction_id, mutation_id, tool_name, score, interpretation) VALUES
(1, 1, 'PolyPhen-2', 0.99, 'Probably Damaging'),
(2, 2, 'SIFT', 0.01, 'Deleterious'),
(3, 3, 'MutationTaster', 1.00, 'Disease Causing'),
(4, 4, 'REVEL', 0.87, 'Pathogenic'),
(5, 5, 'FATHMM', 0.91, 'Likely Pathogenic'),
(6, 6, 'CADD', 25.6, 'High Impact'),
(7, 7, 'SpliceAI', 0.94, 'Disruptive'),
(8, 8, 'OncoKB', 0.85, 'Amplification'),
(9, 9, 'ClinPred', 0.90, 'Likely Pathogenic'),
(10, 10, 'MetaLR', 0.76, 'Possibly Pathogenic');


INSERT INTO homologous_mutations (id, mutation_id, homologous_to_id, homology_type, note) VALUES
(1, 1, 3, 'Orthologous', 'Similar BRCA1 deletion found in mouse models.'),
(2, 2, 9, 'Paralogous', 'TP53 and PTEN show loss-of-function in cancers.'),
(3, 3, 1, 'Functional', 'CFTR and BRCA1 deletion both lead to loss of function.'),
(4, 4, 6, 'Pathway-Level', 'KRAS and APOE affect different parts of PI3K pathway.'),
(5, 5, 10, 'Domain-Based', 'EGFR and VEGFA mutations affect tyrosine kinase activity.'),
(6, 6, 5, 'Species-Conserved', 'KRAS codon 12 conserved in human and rat.'),
(7, 7, 2, 'Splice-Conserved', 'TP53 and MLH1 both share conserved splice motifs.'),
(8, 8, 4, 'Amplification', 'MYC and APOE overexpressed in cancer/AD.'),
(9, 9, 7, 'Deletion-Based', 'PTEN and MLH1 commonly deleted in prostate and colon cancer.'),
(10, 10, 8, 'Functional', 'VEGFA gain-of-function parallels MYC overexpression.');
