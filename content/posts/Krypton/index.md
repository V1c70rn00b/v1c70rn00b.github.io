---
title: "overthewire krypton war games"
date: 2025-07-26
description: This is a writeup on krypton from overthewire wargames.
draft: false # this section allows the post to be published and be public, is it is set to true the post will not be published.
summary: "overthewire wargames krypton series" # Here you can write a small summary of the post if needed
tags: [krypton, linux, cryptography]
categories: [Overthewire]
---

## Krypton 0-1

The goal of this challenges is too sharpen our skills on cryptography.

As for this level, we are provided with a text that is base64 encoded, upon which once decrypted, we get the password for the next level as shown below in the challenge briefing.

```jsx
Welcome to Krypton! The first level is easy. The following string
encodes the password using Base64:

S1JZUFRPTklTR1JFQVQ=

Use this password to log in to krypton.labs.overthewire.org with
username krypton1 using SSH on port 2231. You can find the files for other levels in
/krypton/
```

For this, we shall use our Ubuntu machine to decrypt the text as shown below.

```jsx
v1c70r-n00b@v1c70r-n00b:~/Desktop/overthewire/krypton$ echo "S1JZUFRPTklTR1JFQVQ=" | base64 -d
[REDACTED]
```

With that, we have our password for the next level.

## Krypton 1-2

The goal for this level, is to try and decrypt the encrypted password stored in the file `krypton2` in order to find the password for the next level. Below is a brief of the challenge.

```jsx
The password for level 2 is in the file ‘krypton2’. It is
‘encrypted’ using a simple rotation. It is also in non-standard
ciphertext format. When using alpha characters for cipher text it is
normal to group the letters into 5 letter clusters, regardless of
word boundaries. This helps obfuscate any patterns. This file has
kept the plain text word boundaries and carried them to the cipher
text. Enjoy!
```

Below is the text in the file that we are required to decrypt.

```jsx
krypton1@krypton:/krypton/krypton1$ ls
krypton2  README
krypton1@krypton:/krypton/krypton1$ cat krypton2 
YRIRY GJB CNFFJBEQ EBGGRA
```

From the README file, the text is encrypted using rot13.  Since we are encouraged to not use cryptool, we shall try a [regex](https://stackoverflow.com/questions/5442436/using-rot13-and-tr-command-for-having-an-encrypted-email-address) that can help to decrypt the text as shown below.

```jsx
krypton1@krypton:/krypton/krypton1$ cat krypton2 | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
[REDACTED]
krypton1@krypton:/krypton/krypton1$ 
```

And with that, we have the password for the next level.

## Krypton 2-3

We shall start by reading the `README` file to get the guidelines on how to solve the challenge for this level.

```jsx
Krypton 2

ROT13 is a simple substitution cipher.

Substitution ciphers are a simple replacement algorithm.  In this example
of a substitution cipher, we will explore a 'monoalphebetic' cipher.
Monoalphebetic means, literally, "one alphabet" and you will see why.

This level contains an old form of cipher called a 'Caesar Cipher'.
A Caesar cipher shifts the alphabet by a set number.  For example:

plain:	a b c d e f g h i j k ...
cipher:	G H I J K L M N O P Q ...

In this example, the letter 'a' in plaintext is replaced by a 'G' in the
ciphertext so, for example, the plaintext 'bad' becomes 'HGJ' in ciphertext.

The password for level 3 is in the file krypton3.  It is in 5 letter
group ciphertext.  It is encrypted with a Caesar Cipher.  Without any 
further information, this cipher text may be difficult to break.  You do 
not have direct access to the key, however you do have access to a program 
that will encrypt anything you wish to give it using the key.  
If you think logically, this is completely easy.

One shot can solve it!

Have fun.

Additional Information:

The `encrypt` binary will look for the keyfile in your current working
directory. Therefore, it might be best to create a working direcory in /tmp
and in there a link to the keyfile. As the `encrypt` binary runs setuid
`krypton3`, you also need to give `krypton3` access to your working directory.

Here is an example:

krypton2@melinda:~$ mktemp -d
/tmp/tmp.Wf2OnCpCDQ
krypton2@melinda:~$ cd /tmp/tmp.Wf2OnCpCDQ
krypton2@melinda:/tmp/tmp.Wf2OnCpCDQ$ ln -s /krypton/krypton2/keyfile.dat
krypton2@melinda:/tmp/tmp.Wf2OnCpCDQ$ ls
keyfile.dat
krypton2@melinda:/tmp/tmp.Wf2OnCpCDQ$ chmod 777 .
krypton2@melinda:/tmp/tmp.Wf2OnCpCDQ$ /krypton/krypton2/encrypt /etc/issue
krypton2@melinda:/tmp/tmp.Wf2OnCpCDQ$ ls
ciphertext  keyfile.dat
```

From the above description, this level uses Caesar cipher. We shall do as directed in the example given by first creating a temporary directory for us to solve the challenge and create a symbolic link of the `keyfile.dat` on the temporary directory in order for us to understand how the cipher works then decrypt our text.

```jsx
krypton2@krypton:/krypton/krypton2$ mkdir /tmp/news
krypton2@krypton:/krypton/krypton2$ cp encrypt krypton3  /tmp/news
krypton2@krypton:/krypton/krypton2$ cd /tmp/news
krypton2@krypton:/tmp/news$ ln -s /krypton/krypton2/keyfile.dat 
krypton2@krypton:/tmp/news$ ls
encrypt  keyfile.dat  krypton3  README

```

As seen below, we have tried to encrypt a file using the `encrypt` binary,as shown below.

```jsx
krypton2@krypton:/tmp/news$ echo "ABC"> me.txt
krypton2@krypton:/tmp/news$ /krypton/krypton2/encrypt me.txt 
krypton2@krypton:/tmp/news$ ls
ciphertext  encrypt  keyfile.dat  krypton3  me.txt  README
krypton2@krypton:/tmp/news$ cat ciphertext 
MNO
krypton2@krypton:/tmp/news$ 
```

From the output above, the key is 12, that is a forward shift of 12 (in simple terms the 12th alphabet from the initial letter, hence if A will be M then from B, we count to the 12th letter and that is what B will be)

From the binary, it is clear the forward shift being used to encrypt text is 12. We can use a shift of ±13 to reverse our ciphertext to a readable text as shown below.

```jsx
krypton2@krypton:/tmp/news$ cat krypton3 | tr '[M-ZA-Lm-za-l]' '[A-MN-Zam-n-z]'
[redacted]
```

 The `tr` command

`tr` stands for **translate**. It replaces characters from one set with characters from another.

 `'[M-ZA-Lm-za-l]'`

This set includes **all letters**, but **rearranged** in a Caesar-like shifted order:

- `M-Z A-L` (uppercase)
- `m-z a-l` (lowercase)

This is essentially a Caesar cipher with a **shift of 13**, commonly known as **ROT13**.

 `'[A-MN-Zam-n-z]'`

This is just the standard alphabet:

- `A-M N-Z` (uppercase)
- `a-m n-z` (lowercase)

We have our password for the next level.

## Krypton 3-4

We shall start by reading the `README` file to get the instructions for this level.

```jsx
Well done.  You've moved past an easy substitution cipher.

Hopefully you just encrypted the alphabet a plaintext 
to fully expose the key in one swoop.

The main weakness of a simple substitution cipher is 
repeated use of a simple key.  In the previous exercise
you were able to introduce arbitrary plaintext to expose
the key.  In this example, the cipher mechanism is not 
available to you, the attacker.

However, you have been lucky.  You have intercepted more
than one message.  The password to the next level is found
in the file 'krypton4'.  You have also found 3 other files.
(found1, found2, found3)

You know the following important details:

- The message plaintexts are in English (*** very important)
- They were produced from the same key (*** even better!)

Enjoy.

```

Moreover, we are provided with two hints and three found files as shown below.

```jsx
krypton3@krypton:/krypton/krypton3$ ls
found1  found2  found3  HINT1  HINT2  krypton4  README
krypton3@krypton:/krypton/krypton3$ 
```

Opening the two hint files, we get the following hints.

```jsx
krypton3@krypton:/krypton/krypton3$ cat HINT*
Some letters are more prevalent in English than others.
"Frequency Analysis" is your friend.
```

This is interesting, we have letters that are more prevalent than others, we can use some google fu to understand better what this means. From this [link](https://en.wikipedia.org/wiki/Frequency_analysis?ref=learnhacking.io), we get to understand more about what frequency analysis is. Further search online, we come across this [tool](https://md5decrypt.net/en/Letters-frequency-analysis/?ref=learnhacking.io) that will help us in mapping out the frequency of letters in out cipher texts. The tool did not serve us much, we can try another tool like [quipquip](https://quipqiup.com/) whereby, we shall use the contents of one of the found file then put the contents of the `krypton4` file at the end as shown below then click on solve to wait on the tool to give us a solution as shown below, in my case I concatenated found1 and krypton4 together, then uploaded the output to the tool for it to solve.

```jsx
krypton3@krypton:/krypton/krypton3$ cat found1 krypton4 
CGZNL YJBEN QYDLQ ZQSUQ NZCYD SNQVU BFGBK GQUQZ QSUQN UZCYD SNJDS UDCXJ ZCYDS NZQSU QNUZB WSBNZ QSUQN UDCXJ CUBGS BXJDS UCTYV SUJQG WTBUJ KCWSV LFGBK GSGZN LYJCB GJSZD GCHMS UCJCU QJLYS BXUMA UJCJM JCBGZ CYDSN CGKDC ZDSQZ DVSJJ SNCGJ DSYVQ CGJSO JCUNS YVQZS WALQV SJJSN UBTSX COSWG MTASN BXYBU CJCBG UWBKG JDSQV YDQAS JXBNS OQTYV SKCJD QUDCX JBXQK BMVWA SNSYV QZSWA LWAKB MVWAS ZBTSS QGWUB BGJDS TSJDB WCUGQ TSWQX JSNRM VCMUZ QSUQN KDBMU SWCJJ BZBTT MGCZQ JSKCJ DDCUE SGSNQ VUJDS SGZNL YJCBG UJSYY SNXBN TSWAL QZQSU QNZCY DSNCU BXJSG CGZBN YBNQJ SWQUY QNJBX TBNSZ BTYVS OUZDS TSUUM ZDQUJ DSICE SGNSZ CYDSN QGWUJ CVVDQ UTBWS NGQYY VCZQJ CBGCG JDSNB JULUJ STQUK CJDQV VUCGE VSQVY DQASJ UMAUJ CJMJC BGZCY DSNUJ DSZQS UQNZC YDSNC USQUC VLANB FSGQG WCGYN QZJCZ SBXXS NUSUU SGJCQ VVLGB ZBTTM GCZQJ CBGUS ZMNCJ LUDQF SUYSQ NSYNB WMZSW TBUJB XDCUF GBKGK BNFAS JKSSG QGWDC USQNV LYVQL UKSNS TQCGV LZBTS WCSUQ GWDCU JBNCS UESGN SUDSN QCUSW JBJDS YSQFB XUBYD CUJCZ QJCBG QGWQN JCUJN LALJD SSGWB XJDSU COJSS GJDZS GJMNL GSOJD SKNBJ STQCG VLJNQ ESWCS UMGJC VQABM JCGZV MWCGE DQTVS JFCGE VSQNQ GWTQZ ASJDZ BGUCW SNSWU BTSBX JDSXC GSUJS OQTYV SUCGJ DSSGE VCUDV QGEMQ ESCGD CUVQU JYDQU SDSKN BJSJN QECZB TSWCS UQVUB FGBKG QUNBT QGZSU QGWZB VVQAB NQJSW KCJDB JDSNY VQLKN CEDJU TQGLB XDCUY VQLUK SNSYM AVCUD SWCGS WCJCB GUBXI QNLCG EHMQV CJLQG WQZZM NQZLW MNCGE DCUVC XSJCT SQGWC GJKBB XDCUX BNTSN JDSQJ NCZQV ZBVVS QEMSU YMAVC UDSWJ DSXCN UJXBV CBQZB VVSZJ SWSWC JCBGB XDCUW NQTQJ CZKBN FUJDQ JCGZV MWSWQ VVAMJ JKBBX JDSYV QLUGB KNSZB EGCUS WQUUD QFSUY SQNSU KSVVW BGSJD SVSIS VXBMN YQUUK BNWCU ANMJS
```

![image.png](image.png)

With that, we now have the password for the next level.

## Krypton 4-5

Just like the other challenges, we shall start by first reading the `README` file to see what is required of us.

```jsx
Good job!

You more than likely used frequency analysis and some common sense
to solve that one.

So far we have worked with simple substitution ciphers.  They have
also been 'monoalphabetic', meaning using a fixed key, and 
giving a one to one mapping of plaintext (P) to ciphertext (C).
Another type of substitution cipher is referred to as 'polyalphabetic',
where one character of P may map to many, or all, possible ciphertext 
characters.

An example of a polyalphabetic cipher is called a Vigen�re Cipher.  It works
like this:

If we use the key(K)  'GOLD', and P = PROCEED MEETING AS AGREED, then "add"
P to K, we get C.  When adding, if we exceed 25, then we roll to 0 (modulo 26).

P     P R O C E   E D M E E   T I N G A   S A G R E   E D
K     G O L D G   O L D G O   L D G O L   D G O L D   G O

becomes:

P     15 17 14 2  4  4  3 12  4 4  19  8 13 6  0  18 0  6 17 4 4   3
K     6  14 11 3  6 14 11  3  6 14 11  3  6 14 11  3 6 14 11 3 6  14
C     21 5  25 5 10 18 14 15 10 18  4 11 19 20 11 21 6 20  2 8 10 17

So, we get a ciphertext of:

VFZFK SOPKS ELTUL VGUCH KR

This level is a Vigen�re Cipher.  You have intercepted two longer, english 
language messages.  You also have a key piece of information.  You know the 
key length!

For this exercise, the key length is 6.  The password to level five is in the usual
place, encrypted with the 6 letter key.

Have fun!
```

As stated in the README file, we are using the vigenere cipher, one of the poly-alphabetic ciphers. Moreover, from the description, the key length we are told is 6.

For this case, we shall use online tools to get the key from the found file texts then use the key to decrypt the message on the file `krypton5`. The tool we shall use is this [one](https://www.dcode.fr/vigenere-cipher).

![image.png](image%201.png)

From the screenshot above, we have found the key to use to decrypt the password for the next level,  we shall open a new tab and input the found key to decrypt the cipher text as shown below.

![image.png](image%202.png)

## Krypton 5-6

We start by reading the readme file as usual to get an understanding of the cipher or encryption algorithm we are about to deal with.

```jsx
Frequency analysis can break a known key length as well.  Lets try one
last polyalphabetic cipher, but this time the key length is unknown.

Enjoy.
```

From the description, we need to perform some sort of brute force to get the key length we can use. We shall use the same [tool](https://www.dcode.fr/vigenere-cipher) as the one we used in level 4 

![image.png](image%203.png)

As seen above, we have been able to get the key by using the cipher text from `found1` file and the key-length as well. We can use this key to decrypt the cipher text for password for level 6 as shown below.

![image.png](image%204.png)

## Krypton 6-7

From the `README` file, we get the following information.

```jsx
Hopefully by now its obvious that encryption using repeating keys
is a bad idea.  Frequency analysis can destroy repeating/fixed key
substitution crypto.

A feature of good crypto is random ciphertext.  A good cipher must
not reveal any clues about the plaintext.  Since natural language 
plaintext (in this case, English) contains patterns, it is left up
to the encryption key or the encryption algorithm to add the 
'randomness'.

Modern ciphers are similar to older plain substitution 
ciphers, but improve the 'random' nature of the key.

An example of an older cipher using a complex, random, large key
is a vigniere using a key of the same size of the plaintext.  For
example, imagine you and your confident have agreed on a key using
the book 'A Tale of Two Cities' as your key, in 256 byte blocks.

The cipher works as such:

Each plaintext message is broken into 256 byte blocks.  For each 
block of plaintext, a corresponding 256 byte block from the book
is used as the key, starting from the first chapter, and progressing.
No part of the book is ever re-used as key.  The use of a key of the 
same length as the plaintext, and only using it once is called a "One Time Pad".

Look in the krypton6/onetime  directory.  You will find a file called 'plain1', a 256 
byte block.  You will also see a file 'key1', the first 256 bytes of
'A Tale of Two Cities'.  The file 'cipher1' is the cipher text of 
plain1.  As you can see (and try) it is very difficult to break
the cipher without the key knowledge.

(NOTE - it is possible though.  Using plain language as a one time pad
key has a weakness.  As a secondary challenge, open README in that directory)

If the encryption is truly random letters, and only used once, then it
is impossible to break.  A truly random "One Time Pad" key cannot be
broken.  Consider intercepting a ciphertext message of 1000 bytes.  One
could brute force for the key, but due to the random key nature, you would
produce every single valid 1000 letter plaintext as well.  Who is to know
which is the real plaintext?!?

Choosing keys that are the same size as the plaintext is impractical.
Therefore, other methods must be used to obscure ciphertext against 
frequency analysis in a simple substitution cipher.  The
impracticality of an 'infinite' key means that the randomness, or
entropy, of the encryption is introduced via the method.

We have seen the method of 'substitution'.  Even in modern crypto,
substitution is a valid technique.  Another technique is 'transposition',
or swapping of bytes.

Modern ciphers break into two types; symmetric and asymmetric.

Symmetric ciphers come in two flavours: block and stream.

Until now, we have been playing with classical ciphers, approximating
'block' ciphers.  A block cipher is done in fixed size blocks (suprise!).
For example, in the previous paragraphs we discussed breaking text and keys
into 256 byte blocks, and working on those blocks.  Block ciphers use a
fixed key to perform substituion and transposition ciphers on each
block discretely.

Its time to employ a stream cipher.  A stream cipher attempts to create
an on-the-fly 'random' keystream to encrypt the incoming plaintext one
byte at a time.  Typically, the 'random' key byte is xor'd with the 
plaintext to produce the ciphertext.  If the random keystream can be
replicated at the recieving end, then a further xor will produce the
plaintext once again.

From this example forward, we will be working with bytes, not ASCII 
text, so a hex editor/dumper like hexdump is a necessity.  Now is the
right time to start to learn to use tools like cryptool.

In this example, the keyfile is in your directory, however it is 
not readable by you.  The binary 'encrypt6' is also available.
It will read the keyfile and encrypt any message you desire, using
the key AND a 'random' number.  You get to perform a 'known ciphertext'
attack by introducing plaintext of your choice.  The challenge here is 
not simple, but the 'random' number generator is weak.

As stated, it is now that we suggest you begin to use public tools, like cryptool,
to help in your analysis.  You will most likely need a hint to get going.
See 'HINT1' if you need a kicktstart.

If you have further difficulty, there is a hint in 'HINT2'.

The password for level 7 (krypton7) is encrypted with 'encrypt6'.

Good Luck!
```

We have now moved from classical ciphers to modern ciphers of symmetric and asymmetric ciphers but for this challenge, we are focusing mostly on the stream cipher symmetric,  a flavor of symmetric cipher.

Looking at the hints given.

```jsx
The 'random' generator has a limited number of bits, and is periodic.
Entropy analysis and a good look at the bytes in a hex editor will help.

There is a pattern!
8 bit LFSR
```

Just like level 2, we shall start first by creating a new temporary folder for our exploration, then copy the krypton file to the temporary folder and create a symbolic link o the keyfile to our temporary directory.

```jsx
rypton6@krypton:/krypton/krypton6$ mkdir /tmp/newz
krypton6@krypton:/krypton/krypton6$ cp krypton7 /tmp/newz
krypton6@krypton:/krypton/krypton6$ cd /tmp/newz
krypton6@krypton:/tmp/newz$ ln -s /krypton/krypton6/keyfile.dat 

```

Next I tried to print 50 As using python then encrypt using the binary provided to us and noticed a recurring pattern, where after 30 characters, the pattern starts repeating again as shown below.

```jsx
krypton6@krypton:/tmp/newz$ python3 -c "print( 'A'*50)" > test.txt
krypton6@krypton:/tmp/newz$ /krypton/krypton6/encrypt6 test.txt output.txt
krypton6@krypton:/tmp/newz$ cat output.txt
EICTDGYIYZKTHNSIRFXYCPFUEOCKRNEICTDGYIYZKTHNSIRFXY
```

As seen above, after 30 characters, the pattern starts to repeat `EICTDGYIYZKTHNSIRFXYCPFUEOCKRN EICTDGYIYZKTHNSIRFXY`

We can now try a python script that will fasten the process and be able to get the password for the next level.

```jsx
key = 'EICTDGYIYZKTHNSIRFXYCPFUEOCKRN'
cipher = 'PNUKLYLWRQKGKBE'

pt = ''
for i in range(len(cipher)):
    tmp = ord(cipher[i]) - ord(key[i])
    if tmp < 0: tmp += 26
    tmp += ord('A')
    pt += chr(tmp)
print (pt)

```

Below is a breakdown of the code

 🟦 `key` and `cipher`:

- `key` is a 30-character uppercase string (but only the first 15 are used).
- `cipher` is a 15-character encrypted message.

 🟦 `pt = ''`:

- This initializes the **plaintext result**.

---

 🔄 The `for` loop:

```python
for i in range(len(cipher)):

```

- Iterates over each character of the `cipher` (15 characters in total).

---

 🔤 Decryption Logic:

```python
tmp = ord(cipher[i]) - ord(key[i])

```

- Converts both the cipher character and the corresponding key character to their ASCII values.
- Subtracts the key letter from the cipher letter.

This is how the **Vigenère decryption** works:

```
P (cipher) - E (key) = K (plain)

```

 📉 `if tmp < 0: tmp += 26`

- If the result is negative (e.g., 'A' - 'Z'), wrap around the alphabet by adding 26.

 🔁 `tmp += ord('A')`

- Shifts the result back into the **uppercase letter range (ASCII 65–90)**.

 🔠 `pt += chr(tmp)`

- Converts the result back to a character and appends it to the plaintext.

---

 📤 Final Output:

Prints the decrypted message.

---

 🧪 Example Run:

If you run the code, you'll get the output:

```
KNOWLEDGEISPOWER

```

---

After executing the script we get the password as seen below.

```jsx
krypton6@krypton:/tmp/newz$ python3 a.py 
[REDACTED]
```

## Krypton 7

We have now aided superman overcome his weakness.

```jsx
krypton7@krypton:/krypton/krypton7$ ls
README
krypton7@krypton:/krypton/krypton7$ cat README 
Congratulations on beating Krypton!
krypton7@krypton:/krypton/krypton7$ 
```

TILL we meet again 😊.
