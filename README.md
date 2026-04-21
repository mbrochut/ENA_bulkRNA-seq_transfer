# ENA Bulk RNA-seq Upload Guide

This guide describes the steps required to configure a Linux environment and upload bulk RNA-seq data to the European Nucleotide Archive (ENA). It is based on a real workflow and highlights practical issues and solutions.

---

## 1. Registering Samples and Metadata on ENA

Before uploading any files, you must register your project, study, and samples on ENA.

### 1.1 Create an ENA Account

* Go to: [https://www.ebi.ac.uk/ena/submit/webin/login](https://www.ebi.ac.uk/ena/submit/webin/login)
* Create a Webin account
* Save your **Webin username and password** (you will need them later for uploads). Also the username will be given automatically via the registration form.

> ⚠️ Important: For the next steps, study and sample registration, you can check the [ENA documentation](https://ena-docs.readthedocs.io/en/latest/index.html)

### 1.2 Register a Study
* Log into the Webin portal
* Clic on Register Study on **Study (Project)**
* Provide:

  * Release date (when the data will be submitted publicaly) you can change it at any time.
  * Study Name
  * short descriptive study title (what is the difference, I don't know)
  * Abstract
* After submission, you will receive a **Study accession (e.g., PRJEBXXXX)**

### 1.3 Register Samples
The best way to register sample is
* Click on Register Samples in **Samples**
* Download speadsheet to register samples
* Find the best template to you data or go to Other Checklist then ENA default sample checklist.
* Add optional Fields (you can do it later)
* Next
* Download TSV Template.

Then you need to fill the TSV template with the sample information that you want. You can always add a column if needed.

#### Example Sample TSV

```tsv
sample_alias	title	tax_id	scientific_name	description
sample_1	Sample 1	9606	Homo sapiens	Control sample
sample_2	Sample 2	9606	Homo sapiens	Treated sample
```

#### Notes:

* `sample_alias`: unique identifier for each sample
> ⚠️ It will be the id used later to identify your RUN !
* `sample_title`: This will be the name that you will **see** in the Webin and not the `sample_alias`
* `tax_id`: NCBI taxonomy ID (e.g., 9606 for human or 10090 for mouse)
* `scientific_name`: must match taxonomy (Mus musculus for example)

#### Submission

* Upload the TSV via Webin on Register Samples in **Samples** and Upload filled spreadsheet to register samples. If there is problem in your TSV they will give you an error message.
* After submission, each sample will receive a **Sample accession (e.g., ERSXXXX)**

---

## 2. Installation of Dependencies

ENA uploads require several tools. Using outdated versions will cause failures, so follow these steps carefully.

### 2.1 Install Java (Latest Version Required)

System package managers (apt, yum) usually provide outdated versions.

#### Recommended method:

1. Download from: [https://openjdk.org/](https://openjdk.org/)
2. Choose the **Linux tar.gz version**
3. Extract it:

```bash
tar -xzf openjdk-*.tar.gz
```

4. You don't necesserly need to export the path of java you just need to know where it's located and you will provide this information later:

```bash
path/to/tarFile/jdk-*/bin
```

---

### 2.2 Download webin-cli

1. Go to: [https://github.com/enasequence/webin-cli/releases](https://github.com/enasequence/webin-cli/releases)
2. Download the latest `.jar` file
3. Again, you just ned to know the path of where you have download the webin-cli-9.0.3.jar


---

### 2.3 Install Ruby (>= 3.1)

Required for `aspera-cli`.

System package managers often provide older versions, so use **rbenv**.

#### Install rbenv
To install rbenv you can follow the doc [here](https://github.com/rbenv/rbenv).

normally it's really strateforward with

Centos:
```bash
sudo yum install rbenv
```
or Ubuntu
```bash
sudo apt install rbenv
```

#### Install Ruby

```bash
rbenv install 3.1.0
rbenv global 3.1.0
```

#### Verify installation

```bash
gem -v
ruby -v
```

If Ruby is not detected correctly, ensure your PATH includes:

```bash
export PATH="$HOME/.rbenv/shims:$HOME/.rbenv/bin:$PATH"
```

For fish shell:

```fish
fish_add_path $HOME/.rbenv/shims
fish_add_path $HOME/.rbenv/bin
```

---

### 2.4 Install Aspera CLI

Used for fast and reliable file transfer (strongly recommended).
you can check the doc [here](https://github.com/IBM/aspera-CLI)

#### Install

```bash
gem install aspera-cli
ascli config transferd install
```

* The second command installs the **FASP transfer engine (ascp)**

#### Configure PATH

You must ensure `ascp` is accessible, otherwise `webin-cli` will not use it.

Example:

```bash
export PATH="$HOME/.aspera/sdk:$PATH"
```

Adjust the path depending on your installation location.

---

### 2.5 Final Check

Make sure all tools are available:

```bash
java -version
gem -v
ascli --version
```

If everything works, your environment is ready for ENA uploads.

---

## 3. Upload Process

*To be completed.*

This section will describe:

* Preparing metadata for runs and experiments
* Using `webin-cli` for validation and submission
* Uploading FASTQ files with Aspera
* Handling common errors

---

## Notes and Tips

* Using Aspera (`ascp`) significantly reduces transfer errors compared to FTP
* Always validate metadata before attempting upload
* Keep track of all accession numbers (Study, Sample, Run)
* Store scripts and commands used for reproducibility
