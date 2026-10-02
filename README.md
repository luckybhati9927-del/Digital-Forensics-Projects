# Digital Forensics Projects

Hands-on digital forensics practice by **Lucky Gurjar**. Each project is a short, documented case with tools, steps, findings and limitations.

## Project 1: Forensic Analysis of a Camera Memory Card Image

A practice investigation of a Canon camera memory card image using **Autopsy**. The goal was to identify the device, read photo metadata (EXIF), and find deleted images and check whether they can be recovered.

**Full report:** [`report/Lucky_Gurjar_Digital_Forensics_Report.pdf`](report/Lucky_Gurjar_Digital_Forensics_Report.pdf)

### Evidence

| Item | Details |
|---|---|
| Image | `nps-2009-canon2-gen3.E01` (EnCase E01 disk image) |
| Source | [Digital Corpora](https://digitalcorpora.org/) public forensic practice corpus (nps-2009-canon2) |
| MD5 | `9B1F461C0F16A9703DAC74B327A54694` |
| Device (from EXIF) | Canon PowerShot SD800 IS |

The evidence image is not included in this repository. It is a public training image and can be downloaded from the source above. The hash was recorded after download and before analysis.

### Tools

- Autopsy (disk image analysis, EXIF parsing, deleted file review)
- Windows PowerShell `Get-FileHash` (evidence hashing)
- Ingest modules: File Type Identification, Extension Mismatch Detector, Hash Lookup, Exif Parser, Embedded File Extractor

### Method

1. Downloaded the image and calculated its MD5 hash before opening it.
2. Created a case in Autopsy and added the `.E01` file as a disk image data source.
3. Ran the ingest modules.
4. Reviewed **Analysis Results > EXIF Metadata** and **File Views > Deleted Files**.
5. Recorded file names, paths and sizes, and checked metadata and previews.
6. Documented findings, limitations and a chain of custody note in the report.

### Key Findings

- **33 images** with EXIF metadata, taken with a Canon PowerShot SD800 IS on 23 Dec 2008 (14:12:38 to 14:26:13 as displayed by Autopsy).
- **5 deleted image entries** found:

| # | File | Location | Size (bytes) | Preview |
|---|---|---|---|---|
| 1 | `_MG_0037.JPG` | `/vol_vol2/DCIM/100CANON/` | 1,347,778 | Not available |
| 2 | `_MG_0025.JPG` | `/vol_vol2/DCIM/100CANON/` | 791,333 | Not available |
| 3 | `_MG_0030.JPG` | `/vol_vol2/DCIM/100CANON/` | 867,833 | Not available |
| 4 | `_MG_0035.JPG` | `/vol_vol2/DCIM/100CANON/` | 820,105 | Not available |
| 5 | `f0000000.jpg` | `/vol_vol2/$CarvedFiles/1/` | 855,935 | Displayed |

- **1 image recovered** (`f0000000.jpg`). It was found by file carving (searching raw data for JPEG signatures), not through the normal file table. It shows a photo of a computer monitor with a TextEdit window displaying the digit "1".
- Content of the other four deleted files could not be previewed in Autopsy.

### Screenshots

| Autopsy directory tree | Recovered image |
|---|---|
| ![Directory tree](screenshots/directory_tree.png) | ![Recovered image](screenshots/recovered_f0000000.jpg) |

### Limitations

- Only Autopsy was used. The four non-previewable files may be recoverable with other tools or may be partly overwritten; this was not tested.
- Timestamps are shown as displayed by Autopsy with an India time zone setting. Cameras store local time without a time zone, so the real time zone is unknown, and the camera clock may not be accurate.
- The hash was not compared with a published value.

### Skills Demonstrated

Evidence hashing and integrity, disk image analysis, EXIF metadata analysis, deleted file identification, file carving, forensic report writing, chain of custody documentation.

## Planned Next Projects

- Evidence acquisition and hash verification with FTK Imager
- Network traffic analysis with Wireshark
- Metadata and email header analysis
- A combined mini case report

## Disclaimer

All analysis was done on public training data meant for forensic practice. No real person's private data was used.
