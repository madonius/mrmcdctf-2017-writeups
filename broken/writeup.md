# Br0ken

## Challenge:
A PDF which seems to be corrupted.

## Solution:
By letting binwalk run over the PDF we see various signatures, most of them are `Unix Path` or `Zlib Compressed` but some are JPEGs
```
  $> binwalk challenge.pdf
  …
  19593909      0x12AFAB5       Zlib compressed data, best compression
  19661432      0x12C0278       Zlib compressed data, best compression
  20343555      0x1366B03       Digi International firmware, load address: 0x52284D31, entry point: 0x33303231,
  21795539      0x14C92D3       JPEG image data, JFIF standard 1.01
  25802509      0x189B70D       Unix path: /www.w3.org/1999/02/22-rdf-syntax-ns#">
  25803177      0x189B9A9       Unix path: /purl.org/dc/elements/1.1/">
  25803828      0x189BC34       Unix path: /ns.adobe.com/xap/1.0/mm/">
  26617228      0x196258C       ASCII cpio archive (SVR4 with CRC) file name: "", file name length: "0x20707275", file size: "0x00207071"
  $>
  $>
```

We, in this case want to have a look at the JPEG.

To extract it we can use `dd`
```
$> dd if=challenge.pdf of=6e657274706f61697220636f736b000a.jpg skip=21795539 count=4006970 bs=1
```
or use `binwalk` itself
```
$> binwalk challenge.pdf -e -D "jpeg image:jpg"
```
where:
* `-e`:  Automatically extract known file types
* `-D`:  Extract <type> signatures, give the files an extension of <ext>

In either case we will have a jpeg. With dd, in the directory the output was specified, the directory we are right now, in the example. With binwalk in a `_challenge.pdf.extracted` folder.

This file can be viewed with any image viewer and there you go:
![6e657274706f61697220636f736b000a](6e657274706f61697220636f736b000a.jpg)

## Notes:
### binwalk
Binwalk is a program that iterates over a file and diplays any valid signatures found in such a file
