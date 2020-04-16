# Earth Cube Resource Registry Ontology

This repository contains files for development and maintenance of an ontology for describing EarthCube information resources documented in the EarthCube Resource Registry, extending the work done for dataset description. The project team started ontology development using the [ontology starter kit](https://github.com/INCATools/ontology-starter-kit), which implements conventions used by the [Open Biological and Biomedical Ontology (OBO) Foundry](http://obofoundry.org/). Strict adherence to the conventions there became problematic and was not always followed. The [Ontology, and associated property vocabularies](http://cor.esipfed.org/ont#/so/eccro) are registered in the [ESIP Community Ontology Registry](http://cor.esipfed.org/ont#/) (COR). 

RDF vocabularies used in instance documents:
- schema.org: "http://schema.org/"
- geolink earthcube project: "http://schema.geolink.org/1.0/base/main#"
- vivo: "http://vivoweb.org/ontology/core#"
- dcat v2: "http://www.w3.org/ns/dcat#"
- Namespace for elements defined in this ontology: "http://purl.obolibrary.org/obo/ECRRO_"
- Dublin Core Terms: "http://purl.org/dc/terms/" .
		
The ontology definition (/src/ontology/ecrro-edit.ttl) is an OWL v2 ontology, and uses elements from 	
- schema.org: "http://schema.org/"
- obo: "http://purl.obolibrary.org/obo/"
- Dublin Core: "http://purl.org/dc/elements/1.1/"
- Dublin Core Terms: "http://purl.org/dc/terms/"
	
Vocabulary used for property values are imported from the ESIP COR with URI prefix "http://cor.esipfed.org/ont/earthcube/"

See https://www.earthcube.org/resource_registry for additional project information

## What's in this repository

the repository root contains 
 - README.md -- this file  
 - project.yaml -- OBO foundary project configuration file
 - issue_template.md -- a template to be used by GitHUB for generating new issue reports
 - CONTRIBUTING.md -- instructions for submitting contributions to the project, from the OBO foundry framework package
 - CODE_OF_CONDUCT.md -- statement of conduct expectations for project participants, 


### Directory structure for the repository

#### docs
[Ontology documentation provided by Widico](https://earthcubearchitecture-ecresourcereg.github.io/ecrro/index-en.html) is maintained in this directory. The content is from the Widico project and in general will not need to be modified.

#### Documentation
Some schema diagrams for the ontology information model, using UML, and a powerpoint slide outlining the workflow using the resource registry to link from data discovery to applications using the data.

#### src 
Source code for the ontology.  Most of the content here is imported from the [ontology starter kit](https://github.com/INCATools/ontology-starter-kit); files dated 4/30/2019 are from that original repository set up, and for the most part ar not used. The critical content is in the **src** directory. The ontology is in the file **src/ecrro-edit.ttl**. 

#### Examples 
Test instances are in the 
[Information Model GitHub repository Examples folder](https://github.com/earthcubearchitecture-ecresourcereg/infomodel/tree/master/examples). This were constructed manually to test the ontology framework 

# Contributors

This project is an open project, and contributions are welcome from any individual.  All contributors to this project are bound by a [code of conduct](CODE_OF_CONDUCT.md).  Please review and follow this code of conduct as part of your contribution.

### Current contributors:
  * Ruth Duerr [![orcid](https://img.shields.io/badge/orcid-0000--0003--4808--4736-brightgreen.svg)](https://orcid.org/0000-0003-4808-4736)
  * Stephen Richard [![orcid](https://img.shields.io/badge/orcid-0000--0001--6041--5302-brightgreen.svg)](https://orcid.org/0000-0001-6041-5302)
  * Ilya Zaslavsky [![orcid](https://img.shields.io/badge/orcid-0000--0003--4191--8275-brightgreen.svg)](https://orcid.org/0000-0003-4191-8275)

### Tips for Contributing

Guidance for contributing is provided in [the 'CONTRIBUTING' document](CONTRIBUTING.md). Issues and bug reports are always welcome, using the issue tracker. You will need a GitHub account to post new issues.  Code clean-up, and feature additions can be proposed through pull requests, either via branches from project members, or from [project forks](https://help.github.com/en/github/getting-started-with-github/fork-a-repo) for others.

All EarthCube products must have a clear and explicit statement of license terms. 


### Stable release versions

The latest version of the ontology can always be found at:

http://purl.obolibrary.org/obo/ecrro.owl

[Older releases](https://github.com/earthcubearchitecture-ecresourcereg/ecrro/releases) are available in this repository

### Editors' version

Editors of this ontology should use the edit version, [src/ontology/ecrro-edit.ttl](src/ontology/ecrro-edit.ttl)

## Contact

Please use this GitHub repository's [Issue tracker](https://github.com/earthcubearchitecture-ecresourcereg/ecrro/issues) to request new terms/classes or report errors or specific concerns related to the ontology.

## Acknowledgements

This ontology repository was created using the [ontology starter kit](https://github.com/INCATools/ontology-starter-kit)
