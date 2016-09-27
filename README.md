# PathOS
PathOS is a decision support tool to manage, analyse and report on high througput DNA sequencing variants

## Introduction
Clinical diagnostics is being transformed by the technology capable of analysing patient DNA at the nucleotide level.
Translating the data from this technology into clinically useful information requires decision support software that can analyse the data from sequencers and allow clinical scientists to interpret the DNA variantions.
High throughput sequencing generates many technical artefacts from the chemical processing in the sequencing, these artefacts must be identified and filtered out of the data before further analysis.
This curation process requires identifiying and annotating DNA changes, SNPs (single nucleotide polymorphisms), indels (insertions and deletions), CNVs (copy number variants) and SVs (structural variants) within a sample of patient DNA (either blood or tumour).
Once annotated, mutations are matched with internal and external databases to identify known pathogenic (disease causing) or actionable mutations (variants with an approriate drug).
The resulting few variants are then rendered into a clinical diagnostic report suitable for the treating clinician incorporating clinical evidence and relevent publications.

PathOS carries out these tasks within a hospital laboratory setting where many patients must be reported on in a reliable, consistent and efficient manner.

## Technology Platform
PathOS take advantage of many open-source and public Java libraries to implement an enterprise-grade application suitable for hospital use and secure storage of patient medical data. It interfaces to laboratory LIMS systems for input of patient demographic details and sample and assay registration data. An HL7 interface is currently being developed to interface with other hospital records systems.
The web application is implemented in Java, Javascript, Groovy and Grails deployable on any server supporting java servlet containers such as Tomcat or Jboss.
This allows for deployment in a wide range of environments.
Access to the system is controlled by the Spring Security Library which optionally uses an organisation's LDAP server for authentication or the internal database for authorisation and role assignment.
Web traffic is uses by Google Analytics to monitor user activity for workflow and user interface refinement.
The backend database is implemented with MariaDB, a MySQL compatible relational database, which stores the variant annotation cache and persistent java objects via Spring and Hibernate. The code base is managed in Atlassian Bitbucket and Git.
Build management uses Gradle to build system modules and create shared artefacts such as JARs, WARs and TAR files. Internally, Atlassian Bamboo is used to perform builds of system modules triggered by developer commits to the code repository.
The PathOS search engine is implemented in Apache Lucene. This is a powerful search framework allowing customised search capabilities over any text field in the PathOS domain model.

## Installation
PathOS has been deployed in a number of clinical environments. The simplest way to use PathOS is by accessing a cloud instance which can be customsied (see below).
If you'd like to build the system from scratch, this repository can be built using the pathos_deploy.sh bash script at the top level. This script runs on Linux or OSX but will need to be adapted for Windows environments.
There are a number of dependencies including the following
- Java JDK 1.7 from http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
- Grails (we use 2.3.7 at time of writing) from https://github.com/grails/grails-core/releases/download/v2.3.7/grails-2.3.7.zip
- Gradle (we use 1.10 at time of writing)
- Git & a git ssh key with access to PathOS git repository
- MySql or MariaDB
- Tomcat

## Contact
Ken Doig, Bioinformatics, Cancer Research Department, Data Scientist, Molecular Pathology Department
Peter MacCallum Cancer Centre
Victorian Comprehensive Cancer Centre Building
305 Grattan Street
Melbourne Victoria 3000 Australia
Ph: +61 411 225 178 Mail: ken.doig@petermac.org

## Demonstration Platform
A cloud instance on PathOS is maintained on the University of Melbourne Nectar cloud at http://115.146.87.30:8746/PathOS
Please get in touch with the authors for a login account.