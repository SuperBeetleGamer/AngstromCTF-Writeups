# mhsCTF2022-Green2-Writeup

`Yo listen up here's a story. About a little guy who lives in a green world. (alphabet = vxotbj9a8yqp7n5mh1rzwcd6gfiks3uel240{}_; ciphertext = jkwb44pg26teiu}78uu{)`
![chall](https://user-images.githubusercontent.com/81491665/155752200-0656cb01-2efb-4f6f-b698-aacc50d872c1.png)

At the start of the challenge we are given an alphabet, a ciphertext, and what appears to be a 4x4 grid of different shades of green. Same as Green 1, we extracted the green RGB value of each square on the 4x4 grid. This left with these values:
```
[16 31 32 32]
[2  3  16 31]
[18 31 15 16]
[28 21 3  25]
```
Wait a second! This looks like a Matrix! 
Matrices are pretty common in ciphers and other types of encryption and since this is a beginner/intermediate ctf, our team guessed that it as the [Hill Cipher](https://en.wikipedia.org/wiki/Hill_cipher). Further testing proved that this was correct.

Great! Now for the coding and solving part.

Reading through the Wiki, the matrix for decryption needed to be inverted by `mod len(alphabet)`. Usually it would be inverted by `mod 26` but the length of our alphabet is `mod 39`.
Easy enough, a few lines in [sagemath](https://sagecell.sagemath.org/) should do it.

```py
alphabet = 'vxotbj9a8yqp7n5mh1rzwcd6gfiks3uel240{}_'
m = Matrix(Integers(len(alphabet)), [[16,31,32,32],[2,3,16,31],[18,31,15,16],[28,21,3,25]]) # Integers(39) allows for all calculations under mod 39
m_inv = pow(m,-1) # Inverting the matrix
```

This gives us the output Matrix of m_inv:
```
[25 10 26 31]
[ 4  7 33 15]
[20 33 10 16]
[38 32 34 21]
```
Great! Now its just standard `Hill Cipher` decryption! Since the ciphertext is 20 chars long, we can split them up into 5 blocks of 4 chars each. Each block of 4 chars can be decrypted seperately.
First
