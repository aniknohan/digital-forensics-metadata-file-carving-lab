# Environment Setup Evidence

This section documents the preparation of the Ubuntu virtual machine and verification of the forensic tools used throughout the Digital Forensics Metadata and File Carving Lab.

The environment was configured to support metadata examination, filesystem analysis, deleted-file identification, and file recovery.

---

## Step 1 — ExifTool Installation and Verification

ExifTool was installed on the Ubuntu virtual machine to support metadata extraction and analysis of digital evidence.

### Installation

The following command was used to install ExifTool:

```bash
sudo apt install libimage-exiftool-perl

The package manager confirmed that `libimage-exiftool-perl` was installed and up to date.

### Version Verification

```bash
exiftool -ver
```

**Installed version:** `12.76`

### Executable Verification

```bash
which exiftool
```

**Executable path:** `/usr/bin/exiftool`

### Evidence

![ExifTool installation and verification](01-exiftool-installation-and-verification.png)

### Analyst Note

ExifTool was successfully installed and verified on the Ubuntu virtual machine. It will be used later in this lab to extract and examine metadata from PDF and JPEG evidence files.

---

## Step 2 — The Sleuth Kit Installation and Verification

The Sleuth Kit was installed on the Ubuntu virtual machine to support filesystem-level forensic analysis and deleted-file recovery.

### Installation

```bash
sudo apt install sleuthkit

The Ubuntu package manager confirmed that `sleuthkit` was installed and up to date.

### Version Verification

The `fls` and `icat` forensic utilities were verified using:

```bash
fls -V
icat -V
```

Both utilities reported:

```text
The Sleuth Kit ver 4.12.1
```

**Installed version:** `4.12.1`

### Executable Verification

The locations of the `fls` and `icat` executables were verified using:

```bash
which fls
which icat
```

The system returned:

```text
/usr/bin/fls
/usr/bin/icat
```

### Evidence

![The Sleuth Kit installation and verification](02-sleuthkit-installation-and-verification.png)

### Analyst Note

The Sleuth Kit was successfully installed and verified on the Ubuntu virtual machine. The `fls` utility will later be used to examine the NTFS filesystem image and identify a deleted file entry. The `icat` utility will then be used to recover the deleted file's contents directly from the forensic image.

---

## Environment Setup Summary

The primary forensic analysis tools required for the lab were successfully installed and verified.

| Tool | Version | Purpose |
|---|---|---|
| ExifTool | 12.76 | Extract and analyze PDF and JPEG metadata |
| The Sleuth Kit | 4.12.1 | Filesystem forensic analysis |
| `fls` | 4.12.1 | Identify filesystem entries and deleted files |
| `icat` | 4.12.1 | Recover file contents from filesystem images |

The Ubuntu virtual machine is now prepared for the evidence creation, metadata analysis, and deleted-file recovery stages of the lab.
