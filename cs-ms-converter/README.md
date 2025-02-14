# Mass Spectroscopy File Converter Helmchart-Deployment

## Introduction:

The integration of the latest machines and technologies, combined with a diverse array of existing systems, expands the range of data formats and types, thereby introducing new challenges. To effectively navigate these complexities, it is crucial to implement robust governance mechanisms that ensure seamless conversion of diverse data formats into standardized forms.<br />
<br />
The converter service is built on the Common Workflow Language (CWL), selected for its exceptional interoperability, portability, and reproducibility. The source code is housed in a GitLab repository, complete with a CI/CD pipeline that not only automates the generation of conversion and validation summary reports but also fosters community collaboration. These reports offer a detailed overview of the conversion and validation process, ensuring transparency and efficiency across various file formats.<br />
<br />
The repository includes all the necessary code to run a converter, which is built on Flask, adheres to the OpenAPI specification, and provides a REST API for seamless integration with other NFDI4Chem services. The entire system is also packaged in a Docker image for easy deployment, and with a Helm chart available to streamline deployment in Kubernetes environments. <br />
<br />
This repository is specifically designed to facilitate the conversion of mass spectrometry files from various formats into the mzML format via **Helm Chart Deployment**
<br />


## Overview

<details>
  <summary>Tab 1: Kubernetes Deployment</summary>
The deployment of Kubernetes instance is done via [Helm chart](https://helm.sh/)

1. Clone the repository `git clone https://git.rwth-aachen.de/linsherpa/msconverter-helmchart.git`
1. Provide necessary changes on the values.yaml such as name of `app, namespace, persistent volume claims, APIConnection`, etc
1. Command for deploying Helm Chart:
```
helm install -f values.yaml < name-of-the-helm-app > .
```

</details>

## MS-Converter Server:
If the server has been deployed correctly following the instructions, then the following image would be seen.

[Mass Spectrometry File Converter](images/msconverter_open_api_specification.png)

It consists of three functions:

#### a. msconvert:Conversion
The aim of conversion is to convert a format (in this case a Mass Spectrometry File) into a standardized (mzML) format. <br />
The converter is based on [proteowizard image](https://github.com/ProteoWizard/container) <br />
Input Files:
It works for three types of input.
1. Folder with `.D` or `.d` extention: <br /> 
The folder needs to be converted to `.tar` extention with maintaining the name `< name of folder >.d.tar` <br /> 
2. File with `.RAW` or `.raw` extention: <br />
The File can be directly used  for conversion. <br /> 
3. File that requires another secondary file i.e. `.wiff and .wiff.scan` extention.<br />
Both of the files need to be compressed together into `.tar` extention. <br /> 

#### b. FileInfo:Validation
 The input file for it is file with extention `.mzML`. 
 The validation is based on [FileInfo command from Openms](https://www.openms.org/documentation/html/TOPP_FileInfo.html)
 The image used for validation is [biocontainers/openms](https://hub.docker.com/r/biocontainers/openms/tags) <br />


#### c. FileConvert:Conversion
The input file for it is file with extention `.mzML`. 
The main use of this tool is to convert data from external sources to the formats used by OpenMS/TOPP.
The validation is based on [Fileconverter command from Openms](https://www.openms.org/documentation/html/TOPP_FileConverter.html).
The image used for validation is the same [biocontainers/openms](https://hub.docker.com/r/biocontainers/openms/tags) <br />



## Acknowledgement:
Funded by the Deutsche Forschungsgesellschaft (DFG, German Research Foundation) under the National Resaerch Data Infrastructure – NFDI/1 – Project number 441958208
