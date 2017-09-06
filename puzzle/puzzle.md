# Puzzle

## Challenge
The challenge consisted of a `zip` archive that could be downloaded.
In the Archive were 4 PNG images.

## Solution

### Hints:
The `PNG` images have the cryptic names:
* `eX0K.png`
* `Q0R7.png`
* `TVJN.png`
* `ZTRz.png`

### Careful examination:
This looks cryptic enough that it could be base64 encoded, therefore:
```python
#!/usr/bin/env python3
import os
import itertools
import base64

dirlisting = os.listdir('.')
png_listing = []

for x in dirlisting:
    if ".png" in x:
        png_listing.append(x.strip('.png'))

for base64encoded in itertools.permutations(png_listing):
    print(base64.standard_b64decode("".join(base64encoded)))
```

#### The output
```
b'e4sMRMCD{y}\n'
b'e4sMRMy}\nCD{'
b'e4sCD{MRMy}\n'
b'e4sCD{y}\nMRM'
b'e4sy}\nMRMCD{'
b'e4sy}\nCD{MRM'
b'MRMe4sCD{y}\n'
b'MRMe4sy}\nCD{'
b'MRMCD{e4sy}\n'
b'MRMCD{y}\ne4s'
b'MRMy}\ne4sCD{'
b'MRMy}\nCD{e4s'
b'CD{e4sMRMy}\n'
b'CD{e4sy}\nMRM'
b'CD{MRMe4sy}\n'
b'CD{MRMy}\ne4s'
b'CD{y}\ne4sMRM'
b'CD{y}\nMRMe4s'
b'y}\ne4sMRMCD{'
b'y}\ne4sCD{MRM'
b'y}\nMRMCD{e4s'
b'y}\nMRMe4sCD{'
b'y}\nCD{e4sMRM'
b'y}\nCD{MRMe4s'
```
There we see the string
`b'MRMCD{e4sy}\n'`
which looks quite flaggy and surprise **IS THE FLAG**
