# Digital Forensics Metadata and File Carving Lab

Hands-on digital forensics lab demonstrating metadata analysis, EXIF inspection, NTFS filesystem examination, deleted-file recovery, and cryptographic integrity verification using ExifTool and The Sleuth Kit.

---

## Project Overview

This project demonstrates a controlled digital forensic investigation performed in an Ubuntu virtual machine.

The lab follows evidence from initial creation and metadata examination through NTFS filesystem analysis, deleted-file recovery, and final integrity verification.

The investigation demonstrates how forensic analysts can extract metadata, examine filesystem structures, recover deleted evidence, and verify that recovered data remains identical to the original evidence.

---

## Objectives

The primary objectives of this lab were to:

- Configure and verify a Linux-based digital forensics environment.
- Create controlled PDF and JPEG evidence.
- Extract and interpret PDF metadata.
- Examine JPEG EXIF metadata.
- Analyze an NTFS forensic image.
- Identify deleted filesystem entries.
- Recover deleted file contents directly from a forensic image.
- Examine metadata retained after recovery.
- Calculate MD5 and SHA-256 hashes.
- Perform byte-for-byte evidence comparison.
- Verify the integrity of recovered digital evidence.

---

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Ubuntu Linux | Forensic analysis environment |
| ExifTool | PDF and JPEG metadata extraction |
| The Sleuth Kit | Filesystem-level forensic analysis |
| `fsstat` | NTFS filesystem examination |
| `fls` | Filesystem and deleted-entry identification |
| `icat` | Deleted-file content recovery |
| NTFS-3G | NTFS filesystem support |
| LibreOffice Writer | Controlled document evidence creation |
| `file` | File-type identification |
| `md5sum` | MD5 integrity verification |
| `sha256sum` | SHA-256 integrity verification |
| `cmp` | Byte-for-byte comparison |

---

## Investigation Workflow

```text
Environment Setup
       |
       v
Controlled Evidence Creation
       |
       v
PDF Metadata Analysis
       |
       v
JPEG EXIF Analysis
       |
       v
NTFS Forensic Image Examination
       |
       v
Deleted File Identification
       |
       v
Deleted File Recovery
       |
       v
Recovered Metadata Examination
       |
       v
MD5 / SHA-256 Verification
       |
       v
Byte-for-Byte Comparison
       |
       v
Evidence Integrity Confirmed
```

---

## Evidence Documentation

All screenshots are organized by investigation phase under:

```text
evidence/screenshots/
```

### 01 — Environment Setup

Installation and verification of the primary forensic tools used throughout the investigation.

**Evidence documented:**

- ExifTool installation and verification
- The Sleuth Kit installation and verification
- NTFS-3G verification
- LibreOffice Writer verification

[View Environment Setup Evidence](evidence/screenshots/01-environment-setup/)

---

### 02 — PDF Evidence

Creation of a controlled document containing known metadata followed by PDF export and forensic metadata examination.

**Evidence documented:**

- Controlled document creation
- Metadata configuration
- PDF export verification
- ExifTool PDF metadata analysis

[View PDF Evidence](evidence/screenshots/02-pdf-evidence/)

---

### 03 — JPEG Metadata

ExifTool was used to examine EXIF information embedded within controlled JPEG evidence.

Recovered metadata included information such as:

- Image description
- Camera manufacturer
- Camera model
- Original timestamp
- Image dimensions

The examination also identified an inconsistency between the `SONY DSC` image-description field and the reported Canon camera information, demonstrating why forensic metadata should be interpreted critically.

[View JPEG Metadata Evidence](evidence/screenshots/03-jpeg-metadata/)

---

### 04 — NTFS Forensic Image

The controlled forensic image `carve1.img` was examined to verify its filesystem structure.

The Sleuth Kit `fsstat` utility confirmed:

```text
File System Type: NTFS
First Cluster of MFT: 4
MFT Entry Size: 1024 bytes
Sector Size: 512 bytes
Cluster Size: 4096 bytes
```

This established that the image could be processed for filesystem-level forensic analysis.

[View NTFS Forensic Image Evidence](evidence/screenshots/04-ntfs-forensic-image/)

---

### 05 — Deleted File Recovery

The Sleuth Kit `fls` utility was used to identify a deleted JPEG within the NTFS forensic image.

The deleted entry was identified as:

```text
-/r * 64-128-2: deleted-evidence.jpg
```

The metadata address was then supplied to `icat`:

```bash
icat carve1.img 64-128-2 > recovered-evidence.jpg
```

The recovered file was successfully recognized as valid JPEG image data.

ExifTool was subsequently used to confirm that readable metadata remained present in the recovered evidence.

[View Deleted File Recovery Evidence](evidence/screenshots/05-deleted-file-recovery/)

---

### 06 — Integrity Verification

The original and recovered JPEG files were compared using multiple verification methods.

#### MD5

```text
6a460220c997a60e0300ad3cc4cb0ff4
```

#### SHA-256

```text
f3dc904215b9ec79451dfa3514b1d9bddd2daa29286952ba2d0d5ae84bd70775
```

Both files produced identical MD5 and SHA-256 hashes.

A direct byte comparison was also performed:

```bash
cmp -s picture.jpg_original recovered-evidence.jpg && echo "MATCH: Files are byte-for-byte identical"
```

Result:

```text
MATCH: Files are byte-for-byte identical
```

These results confirmed that the recovered JPEG was identical to the original controlled evidence.

[View Integrity Verification Evidence](evidence/screenshots/06-integrity-verification/)

---

## Key Findings

The forensic examination produced the following findings:

1. Metadata embedded within PDF and JPEG evidence was successfully extracted using ExifTool.
2. JPEG EXIF information contained useful camera and image characteristics.
3. The controlled forensic image contained a recognizable NTFS filesystem.
4. NTFS filesystem metadata retained a reference to a deleted JPEG.
5. The deleted file was successfully identified using `fls`.
6. The deleted file contents were successfully recovered using `icat`.
7. The recovered JPEG remained structurally valid and retained readable metadata.
8. The original and recovered JPEG produced identical MD5 hashes.
9. The original and recovered JPEG produced identical SHA-256 hashes.
10. A byte-for-byte comparison confirmed that the recovery process reproduced the original evidence without modification.

---

## Evidence Integrity Results

| Evidence | Verification | Result |
|---|---|---|
| Original JPEG | MD5 | `6a460220c997a60e0300ad3cc4cb0ff4` |
| Recovered JPEG | MD5 | `6a460220c997a60e0300ad3cc4cb0ff4` |
| Original JPEG | SHA-256 | `f3dc904215b9ec79451dfa3514b1d9bddd2daa29286952ba2d0d5ae84bd70775` |
| Recovered JPEG | SHA-256 | `f3dc904215b9ec79451dfa3514b1d9bddd2daa29286952ba2d0d5ae84bd70775` |
| Original vs. recovered | Byte comparison | Identical |
| `carve1.img` | SHA-256 | `ae890fcff6d26ea957702c5a4cf38b227336ddad00ae22053aa332208f62deec7` |

---

## Repository Structure

```text
digital-forensics-metadata-file-carving-lab/
|
├── README.md
|
└── evidence/
    └── screenshots/
        |
        ├── 01-environment-setup/
        │   ├── 01-exiftool-installation-and-verification.png
        │   ├── 02-sleuthkit-installation-and-verification.png
        │   ├── 03-ntfs-tools-installation-and-verification.png
        │   ├── 04-libreoffice-installation-and-verification.png
        │   └── README.md
        |
        ├── 02-pdf-evidence/
        │   ├── 05-controlled-document-creation.png
        │   ├── 06-document-metadata-configuration.png
        │   ├── 07-pdf-export-verification.png
        │   ├── 08-pdf-metadata-exiftool-analysis.png
        │   └── README.md
        |
        ├── 03-jpeg-metadata/
        │   ├── 09-jpeg-exif-metadata-analysis.png
        │   └── README.md
        |
        ├── 04-ntfs-forensic-image/
        │   ├── 10-ntfs-forensic-image-verification.png
        │   └── README.md
        |
        ├── 05-deleted-file-recovery/
        │   ├── 11-deleted-file-identification-fls.png
        │   ├── 12-deleted-file-recovery-icat.png
        │   ├── 14-recovered-exif-metadata-verification.png
        │   └── README.md
        |
        └── 06-integrity-verification/
            ├── 13-recovered-file-integrity-verification.png
            ├── 15-final-evidence-hash-summary.png
            └── README.md
```

---

## Skills Demonstrated

This project demonstrates practical experience with:

- Digital forensics
- Linux forensic analysis
- Evidence handling
- Metadata analysis
- EXIF analysis
- PDF metadata examination
- NTFS filesystem analysis
- Master File Table concepts
- Deleted-file identification
- Deleted-file recovery
- The Sleuth Kit
- ExifTool
- Cryptographic hashing
- MD5 and SHA-256 verification
- Evidence integrity validation
- Byte-level file comparison
- Forensic documentation
- Analyst reporting

---

## Forensic Takeaways

This lab demonstrates that file deletion does not necessarily mean that the underlying data has been immediately destroyed.

Filesystem metadata can retain references to deleted files, while file contents may remain recoverable until the associated storage space is overwritten.

The investigation also demonstrates why recovered evidence should be validated using cryptographic hashes and other comparison methods before conclusions are made about its integrity.

---

## Conclusion

This project demonstrates an end-to-end controlled digital forensic investigation involving evidence creation, metadata examination, filesystem analysis, deleted-file recovery, and integrity verification.

ExifTool provided visibility into metadata contained within PDF and JPEG evidence, while The Sleuth Kit enabled direct examination of the NTFS filesystem and recovery of deleted file contents.

The recovered JPEG produced the same MD5 and SHA-256 hashes as the original evidence and was confirmed through direct comparison to be byte-for-byte identical.

The lab therefore demonstrates both successful deleted-file recovery and a defensible process for validating the integrity of recovered digital evidence.

---

---

## Author

**Anik Nohan**

This hands-on Digital Forensics Metadata and File Carving Lab was completed to demonstrate practical skills in:

- PDF and JPEG metadata analysis
- EXIF metadata examination
- NTFS filesystem analysis
- Deleted-file identification
- Deleted-file recovery using The Sleuth Kit
- Evidence verification using MD5 and SHA-256
- Byte-for-byte integrity validation
- Forensic evidence documentation

### Tools Used

`ExifTool` • `The Sleuth Kit` • `fls` • `icat` • `fsstat` • `NTFS-3G` • `LibreOffice` • `md5sum` • `sha256sum` • `cmp`

---

*This project was completed in a controlled lab environment for digital forensics training and portfolio demonstration.*


