# Omniscient Flag's Metadata

As you step into the second chamber, a figure manifests. Before you stands a forgotten deity, a dead god spoken of only in whispers. Known by countless names: “Apostle of Epilogue and Eternity”, “Lone Messiah” and many more lost to time.
They leave nothing but a single image, a relic carrying his final secret. Hidden within its layers lies the key to ascend to the next chamber.

## Solution:

- On opening the file attached in the challenge, it shows: <img width="640" height="1017" alt="image" src="https://github.com/user-attachments/assets/30ee5e21-3ab2-4af8-96a2-220206ddd7d7" />
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
    "https://citadel.cryptonitemit.in/files/2378893c721bd88ce6e99b644c4ef130/challenge.jpg? token=eyJ1c2VyX2lkIjoyMTUsInRlYW1faWQiOjY3LCJmaWxlX2lkIjo0NX0.aOEVfA.ITrIVbCALijLLz16Cv7L39lg5sM",
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
- Use `open hidden.png` to view the flag. <img width="514" height="456" alt="image" src="https://github.com/user-attachments/assets/320fb246-4ce6-4c4b-802f-1df30dcf2380" />


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

