# CC1

## Challenge
A single netcat command
```shell
nc ctf.canthack.me 21367
```

## Solution
Carelessly pasting this command to a shell and executing, establishes a connection to the canthack server and greets us:

```
estdcplwwjhldlwteewpezzpldjhldyete
Enter text to encrypt it
Enter "e" to exit
```

Randomly entering text displays encrypted versions of the input
```
abcd
lmno
```
```
foobar
qzzmlc
```

A first relief is that the encryption is always the same - multiple encryptions
of `a` always yield `l`. And it appears the encryption is letter-by-letter and
independent of the position of a letter in the string: `aaaa` encrypts to
`llll`. Avoiding any elegance, one can create a translation table by
copy&pasting an alphabet, including line breaks from a text editor to the
netcat shell to retrieve an encrypted alphabet. One facepalm later, we avoid
quitting netcat upon encountering the letter `e` by testing `ee` individually
and then pasting the rest of the alphabet.

### python decrypter

A fast written and un-elegant decryption can then be hacked together with copy&paste and text-editor ninja-moves:

```python
dec = [
"a",
"b",
"c",
"d",
"e",
"f",
"g",
"h",
"i",
"j",
"k",
"l",
"m",
"n",
"o",
"p",
"q",
"r",
"s",
"t",
"u",
"v",
"w",
"x",
"y",
"z",
]


enc = [
"l",
"m",
"n",
"o",
"p",
"q",
"r",
"s",
"t",
"u",
"v",
"w",
"x",
"y",
"z",
"a",
"b",
"c",
"d",
"e",
"f",
"g",
"h",
"i",
"j",
"k",
]

encode = PUT ENCODED MESSAGE HERE!
decoded = ""

for l in encoded:
    i = enc.index(l)
    decoded += dec[i]

print decoded

```

Embarrassed, we realise that the encryption is not even a random letter
reshuffling but was just ROT11. Fine, the welcome message told us we're
looking for the clear text of `estdcplwwjhldlwteewpezzpldjhldyete`, so we add
it into the python script and see a suspicious decrypted message

```
thisreallywasalittletooeasywasntit
```

Reconnecting to netcat, the server tells us the flag:

```shell
~ > nc ctf.canthack.me 21367
estdcplwwjhldlwteewpezzpldjhldyete
Enter text to encrypt it
Enter "e" to exit
thisreallywasalittletooeasywasntit
mrmcd{8dae23cadeb44fd6fbadec54fc26ac3e}
```

With the flag **`mrmcd{8dae23cadeb44fd6fbadec54fc26ac3e}`**.
