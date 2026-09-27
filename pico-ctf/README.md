# PicoCTF Writeups

Step-by-step challenge notes by adump, with the original screenshots kept in reading order.

## Challenges

- [Riddle Registry](#riddle-registry)
- [Log Hunt](#log-hunt)
- [Hidden in Plain Sight](#hidden-in-plain-sight)
- [Flag in Flame](#flag-in-flame)

[Download the original Word writeup](../originals/Pico-CTF-Writeup.docx)

## Notes

- Environment: Kali Linux through Windows Subsystem for Linux
- AI tools were used while solving some challenges

## Riddle Registry

The challenge provides a PDF that looks ordinary but hints that the useful information is hidden in its metadata.

![Riddle Registry challenge description](assets/screenshot-001.png)

### Solution

1. Copy the PDF download link from the challenge page.

   ![Copying the PDF download link](assets/screenshot-002.png)

2. Download the file with `wget` and save it as `confidential.pdf`.

   ```bash
   wget <challenge-file-url> -O confidential.pdf
   ```

   ![Downloading confidential.pdf](assets/screenshot-003.png)

3. Confirm that the download is a PDF, then inspect its metadata with `pdfinfo`.

   ```bash
   file confidential.pdf
   pdfinfo confidential.pdf
   ```

4. The `Author` field contains a Base64-encoded value. Decode that value to recover the flag.

   ```bash
   echo 'cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9jMjA3MzY2OX0=' | base64 -d
   ```

   ![Inspecting the metadata and decoding the Author value](assets/screenshot-004.png)

**Flag:** `picoCTF{puzzl3d_m3tadata_f0und!_c2073669}`

## Log Hunt

The server log contains repeated flag fragments mixed with normal log messages. The goal is to identify the fragments and reconstruct their original order.

![Log Hunt challenge description](assets/screenshot-005.png)

### Solution

1. Download the supplied log file.

   ```bash
   wget <challenge-file-url> -O server.log
   ```

   ![Downloading the server log](assets/screenshot-006.png)

2. Check the file type. It is ordinary ASCII text, so it can be inspected with standard text tools.

   ```bash
   file server.log
   ```

   ![Checking the server log file type](assets/screenshot-007.png)

3. Reading the file reveals entries labelled `INFO FLAGPART`. Each entry contains one piece of the flag.

   ```bash
   cat server.log
   ```

   ![Finding the first flag fragment in the log](assets/screenshot-008.png)

4. Filter the log to show only the flag fragments. The same values appear repeatedly, so keep the first occurrence of each new fragment and join them in order:

   - `picoCTF{us3_`
   - `y0urlinux_`
   - `sk1lls_`
   - `cedfa5fb}`

   ```bash
   grep 'INFO FLAGPART' server.log
   ```

   ![Filtering repeated FLAGPART entries](assets/screenshot-009.png)

**Flag:** `picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}`

## Hidden in Plain Sight

The supplied JPEG displays normally, but its metadata contains a clue for extracting data hidden inside the image.

![Hidden in Plain Sight challenge description](assets/screenshot-010.png)

### Solution

1. Download the JPEG as `img.jpg`.

   ```bash
   wget <challenge-file-url> -O img.jpg
   ```

   ![Downloading img.jpg](assets/screenshot-011.png)

2. Opening the image does not reveal anything obvious, so inspect its metadata instead.

   ![Previewing the supplied image](assets/screenshot-012.png)

3. Run `exiftool`. The `Comment` field contains the Base64 value `c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9`.

   ```bash
   exiftool img.jpg
   ```

   ![Finding the encoded comment with exiftool](assets/screenshot-013.png)

4. Decode the comment. It produces a `steghide` clue and the passphrase `pAzzword`.

   ```bash
   echo 'c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9' | base64 -d
   echo 'cEF6endvcmQ=' | base64 -d
   ```

5. Extract the hidden file with `steghide`, enter the recovered passphrase, and read `flag.txt`.

   ```bash
   steghide extract -sf img.jpg
   cat flag.txt
   ```

   ![Decoding the passphrase and extracting flag.txt](assets/screenshot-014.png)

**Flag:** `picoCTF{h1dd3n_1n_1m4g3_656e4d79}`

## Flag in Flame

The challenge supplies a very large line of encoded text. The hint indicates that the data should first be decoded from Base64 to recreate an image.

![Flag in Flame challenge description](assets/screenshot-015.png)

### Solution

1. Download the encoded log data.

   ```bash
   wget <challenge-file-url> -O logs.txt
   ```

   ![Downloading the encoded log data](assets/screenshot-016.png)

2. The `file` command reports ASCII text with one extremely long line, which is consistent with Base64 data.

   ```bash
   file logs.txt
   ```

   ![Identifying the Base64 text file](assets/screenshot-017.png)

3. Decode the text and save the binary output.

   ```bash
   base64 -d logs.txt > output
   ```

   ![Decoding the Base64 data](assets/screenshot-018.png)

4. The output is an image. Rename it with a `.png` extension and open it.

   ```bash
   mv output output.png
   open output.png
   ```

   ![Renaming and opening the decoded image](assets/screenshot-019.png)

   ![Decoded image containing a hexadecimal string](assets/screenshot-020.png)

5. A hexadecimal string appears along the bottom of the image. Copy it into CyberChef and apply the **From Hex** recipe. The decoded text in the screenshot begins with `picoCTO`; change the final `O` to `F` to match the required `picoCTF{...}` flag format.

   ![Decoding the hexadecimal text in CyberChef](assets/screenshot-021.png)

**Flag:** `picoCTF{forensics_analysis_is_amazing_be860279}`
