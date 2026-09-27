# Hack10 Writeups

Step-by-step challenge notes by adump, with the original screenshots kept in reading order.

## Challenges

- [Meowww (Forensics)](#meowww-forensics)
- [Detonator (Reverse Engineering)](#detonator-reverse-engineering)
- [DearHiringManager (Forensics)](#dearhiringmanager-forensics)
- [Is It Stacy, Becky, or Kesha? (Reverse Engineering)](#is-it-stacy-becky-or-kesha-reverse-engineering)
- [Hakari Domain 2 (Cryptography)](#hakari-domain-2-cryptography)

[Download the original Word writeup](../originals/Hack10-Writeup.docx)

## Meowww (Forensics)

![Screenshot 1](assets/screenshot-001.png)

- Chal.jpg

![Screenshot 2](assets/screenshot-002.png)

- Perform basic typical forensics image check

![Screenshot 3](assets/screenshot-003.png)

- Try extract embedded file using empty & common passpharase

![Screenshot 4](assets/screenshot-004.png)

![Screenshot 5](assets/screenshot-005.png)

- Try bruteforcing using Rockyou.txt

![Screenshot 6](assets/screenshot-006.png)

- Found the password and embedded files

- Checking the embedded fike content

![Screenshot 7](assets/screenshot-007.png)

- Extracted the obfuscated flag

- Asking chatgpt for suggestions to deobfuscated the flag

![Screenshot 8](assets/screenshot-008.png)

- Put on cyberchef

- Select the recipe

![Screenshot 9](assets/screenshot-009.png)

- Boom ! flag found

**Flag:** `hack10{p0w3r_d3c0d3}`

## Detonator (Reverse Engineering)

![Screenshot 10](assets/screenshot-010.png)

- Download the files

![Screenshot 11](assets/screenshot-011.png)

- Perform basic reverse engineering using Decompiler Explorer ( low size file )

![Screenshot 12](assets/screenshot-012.png)

- Find the using seach function

- Found the fake flag

![Screenshot 13](assets/screenshot-013.png)

- Paste all the decompiler code to chatgpt for analysis

- The flag is static after analyzed ( by gpt )

![Screenshot 14](assets/screenshot-014.png)

- Get the flag

- However we can highlighted a few useful function to understand the program core logic

![Screenshot 15](assets/screenshot-015.png)

- As we can see here the flag here is HACK10 + md5 ( the path in local_78)

![Screenshot 16](assets/screenshot-016.png)

- After we extracted the strings inside local_78 functions we encrypt it using md5

- That explain what chatgpt response after we compare the encrypted strings and chatgpt given flag

![Screenshot 17](assets/screenshot-017.png)

![Screenshot 18](assets/screenshot-018.png)

## DearHiringManager (Forensics)

![Screenshot 19](assets/screenshot-019.png)

- Download the chall files first

![Screenshot 20](assets/screenshot-020.png)

- Perform basic forensics check which is file type , metadata and embedded files

![Screenshot 21](assets/screenshot-021.png)

- Use pdf-parser to extract FULL pdf content

![Screenshot 22](assets/screenshot-022.png)

- Found  flag after using grep

- The flag seems to be obfuscated and after a few while trying to deobfuscated that I decided to this as a fake flag first and look up another suspicious strings / functions

![Screenshot 23](assets/screenshot-023.png)

- Found another suspicious  func that executed separated in var a and var b

![Screenshot 24](assets/screenshot-024.png)

- Reason we  choose this functions

![Screenshot 25](assets/screenshot-025.png)

- Then I decided to combina strings from var a dan var b

- After combined it became this BOPCd0edrK 1i+mVBeXU8:ddd$

![Screenshot 26](assets/screenshot-026.png)

- Next we ask chatgpt to analyzed the strings and we can used dcode to identify matched cipher

![Screenshot 27](assets/screenshot-027.png)

![Screenshot 28](assets/screenshot-028.png)

- We compared the suitable and found after analysis then decrypt it

- I use dcode and cyberchef

![Screenshot 29](assets/screenshot-029.png)

![Screenshot 30](assets/screenshot-030.png)

- Found the flag : hack10{M4l1ci0s_PDF}

## Is It Stacy, Becky, or Kesha? (Reverse Engineering)

![Screenshot 31](assets/screenshot-031.png)

- Download the chall first

- Upload the chall file to decompile the program so that we can analyze the the program logic

![Screenshot 32](assets/screenshot-032.png)

- I picked ghidra and paste to chatgpt to be analyzed

![Screenshot 33](assets/screenshot-033.png)

![Screenshot 34](assets/screenshot-034.png)

- This explain what this chall about which is we need to find suitable candidate as said in chall description that requires us to find who’s email has the access

- Its also compute the emails in md5 so we need to find match md5(email)

- So  we need to find the emails ( dump or common emails ) first

- Then I try to find any information through strings and cat

- Found nothing on strings

![Screenshot 35](assets/screenshot-035.png)

- Suddenly I found a link in cat

![Screenshot 36](assets/screenshot-036.png)

![Screenshot 37](assets/screenshot-037.png)

[https://appsecmy.com/d22646ad92dfaa334f9fa1c3579b4801.txt](https://appsecmy.com/d22646ad92dfaa334f9fa1c3579b4801.txt)

![Screenshot 38](assets/screenshot-038.png)

- found the emails dump

- I downloaded it using wget

![Screenshot 39](assets/screenshot-039.png)

- Now that we has the emails dump and the program I asked gpt to create a program to find matched emails

![Screenshot 40](assets/screenshot-040.png)

- Then we executed the python program to find matched md5(email)

![Screenshot 41](assets/screenshot-041.png)

- Found the email then we compute it as final flag

**Flag:** `HACK10{wa00d6d88epd0z1x6gro@rediffmail.com}`

## Hakari Domain 2 (Cryptography)

![Screenshot 42](assets/screenshot-042.png)

- We download the chall files first

![Screenshot 43](assets/screenshot-043.png)

- We cat and analyzed the program

- chall.py

```python
import os
import random
import sys

from AES import AES

JACKPOT_STREAK = 3
MAX_ATTEMPTS = 250


def huh(*_args):
    return None

def load_flag() -> bytes:
    return os.getenv("FLAG", "hack10{REDACTED}").encode()


def read_guess(jackpot: bool) -> str:
    if jackpot:
        return input("Predict the next number or type 'exit': ").strip()
    return input("Guess the next number: ").strip()


def bytes_to_int(block: bytes) -> int:
    return int.from_bytes(block, "big")


def int_to_bytes(value: int) -> bytes:
    return value.to_bytes(16, "big")


def main() -> None:
    seed = os.urandom(8)
    random.seed(seed)
    flag = load_flag()
    streak = 0
    attempts = 0
    jackpot = False

    print("Welcome to Hakari Domain 2.")
    print(f"Guess the next 32-bit number in [0, {2**32 - 1}].")
    print(f"Reach {JACKPOT_STREAK} correct guesses in a row to hit jackpot.")
    print(f"You have {MAX_ATTEMPTS} total attempts before the connection closes.")
    print()

    while attempts < MAX_ATTEMPTS:
        try:
            raw = read_guess(jackpot)
        except EOFError:
            print("\nBye.")
            return

        if jackpot and raw.lower() == "exit":
            print("Leaving with your collected ciphertexts.")
            return

        try:
            guess = int(raw)
        except ValueError:
            print("Numbers only.")
            continue

        if not 0 <= guess < 2**32:
            print(f"Guess must be between 0 and {2**32 - 1}.")
            continue

        target = random.getrandbits(32)
        attempts += 1

        if guess != target:
            streak = 0
            print(f"Wrong. The number was {target}.")
            print(f"Attempts used: {attempts}/{MAX_ATTEMPTS}")
            continue

        streak += 1
        print(f"Correct. Current streak: {streak}")

        if not jackpot and streak == JACKPOT_STREAK:
            jackpot = True
            print("Jackpot unlocked.")

            key = os.urandom(16)
            secret = os.urandom(16)
            cipher = AES(key)
            cipher._sub_bytes = huh
            secret_enc = cipher.encrypt(secret)

            print("Congratulations! You've hit the jackpot and unlocked the next phase.\n")
            print("Keep predicting the next number to continue choosing actions.")
            print("Encrypted Secret:", secret_enc.hex())
            while True:
                next_target = random.getrandbits(32)
                prediction = int(input("Predict the next number or type -1 to exit: "))

                if prediction == -1:
                    print("Leaving with your collected ciphertexts.")
                    return

                if prediction != next_target:
                    print(f"Wrong. The number was {next_target}.")
                    return

                option = int(input("[1] encrypt, [2] decrypt: "))

                if option == 1:
                    plaintext = bytes.fromhex(input("Input plaintext to encrypt in hex: "))
                    assert len(plaintext) == 16

                    ciphertext = cipher.encrypt(plaintext)
                    print(f"enc(plaintext) = {bytes.hex(ciphertext)}")

                    if plaintext == secret:
                        print(flag)
                        exit()

                elif option == 2:
                    ciphertext = bytes.fromhex(input("Input ciphertext to decrypt in hex: "))
                    assert len(ciphertext) == 16

                    if ciphertext == secret_enc:
                        print("No way!")
                        continue

                    plaintext = cipher.decrypt(ciphertext)
                    print(f"dec(ciphertext) = {bytes.hex(plaintext)}")

        print(f"Attempts used: {attempts}/{MAX_ATTEMPTS}")

    print(f"{MAX_ATTEMPTS} attempts used. Connection closed.")


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\nInterrupted.", file=sys.stderr)
```

- I uploaded the program to chatgpt to let it analyze the program

![Screenshot 44](assets/screenshot-044.png)

- So we need to create solver script , I named it as solve.py

- Python solver script ( solve.py )

```python
from pwn import *
import random

HOST = "34.126.187.50"
PORT = 5501
LEAKS = 234

def unshift_right(x, shift):
    res = x
    for _ in range(32):
        res = x ^ (res >> shift)
    return res

def unshift_left(x, shift, mask):
    res = x
    for _ in range(32):
        res = x ^ ((res << shift) & mask)
    return res

def untemper(v):
    v = unshift_right(v, 18)
    v = unshift_left(v, 15, 0xEFC60000)
    v = unshift_left(v, 7, 0x9D2C5680)
    v = unshift_right(v, 11)
    return v

def invert_step(si, si227):
    x = si ^ si227
    mti1 = (x & 0x80000000) >> 31
    if mti1:
        x ^= 0x9908B0DF
    x = (x << 1) & 0xFFFFFFFF
    mti = x & 0x80000000
    mti1 = (mti1 + (x & 0x7FFFFFFF)) & 0xFFFFFFFF
    return mti, mti1

def init_genrand(seed):
    mt = [0] * 624
    mt[0] = seed & 0xFFFFFFFF
    for i in range(1, 624):
        mt[i] = ((0x6C078965 * (mt[i - 1] ^ (mt[i - 1] >> 30))) + i) & 0xFFFFFFFF
    return mt

def recover_kj_from_ji(ji, ji1, i):
    const = init_genrand(19650218)
    key = ji - (const[i] ^ ((ji1 ^ (ji1 >> 30)) * 1664525))
    return key & 0xFFFFFFFF

def recover_ji_from_ii(ii, ii1, i):
    ji = (ii + i) ^ ((ii1 ^ (ii1 >> 30)) * 1566083941)
    return ji & 0xFFFFFFFF

def recover_kj_from_ii(ii, ii1, ii2, i):
    ji = recover_ji_from_ii(ii, ii1, i)
    ji1 = recover_ji_from_ii(ii1, ii2, i - 1)
    return recover_kj_from_ji(ji, ji1, i)

def recover_seed_candidates(outputs):
    s = [untemper(x) for x in outputs]

    i230_, i231 = invert_step(s[3], s[230])
    i231_, i232 = invert_step(s[4], s[231])
    i232_, i233 = invert_step(s[5], s[232])
    i233_, i234 = invert_step(s[6], s[233])

    i231 = (i231 + i231_) & 0xFFFFFFFF
    i232 = (i232 + i232_) & 0xFFFFFFFF
    i233 = (i233 + i233_) & 0xFFFFFFFF

    seed_l = (recover_kj_from_ii(i233, i232, i231, 233) - 16) & 0xFFFFFFFF
    seed_h1 = (recover_kj_from_ii(i234, i233, i232, 234) - 17) & 0xFFFFFFFF
    seed_h2 = (recover_kj_from_ii((i234 + 0x80000000) & 0xFFFFFFFF, i233, i232, 234) - 17) & 0xFFFFFFFF

    return [
        ((seed_h1 << 32) | seed_l).to_bytes(8, "big"),
        ((seed_h2 << 32) | seed_l).to_bytes(8, "big"),
    ]

def pick_seed(outputs):
    for seed in recover_seed_candidates(outputs):
        r = random.Random()
        r.seed(seed)
        if [r.getrandbits(32) for _ in range(len(outputs))] == outputs:
            return seed
    raise ValueError("seed recovery failed")

def gf_mul(a, b):
    res = 0
    for _ in range(8):
        if b & 1:
            res ^= a
        hi = a & 0x80
        a = (a << 1) & 0xFF
        if hi:
            a ^= 0x1B
        b >>= 1
    return res

def gf_pow(a, e):
    res = 1
    while e:
        if e & 1:
            res = gf_mul(res, a)
        a = gf_mul(a, a)
        e >>= 1
    return res

def gf_inv(a):
    if a == 0:
        raise ZeroDivisionError
    return gf_pow(a, 254)

def solve_gf256(mat, rhs):
    n = len(mat)
    aug = [mat[i][:] + [rhs[i]] for i in range(n)]

    row = 0
    for col in range(n):
        pivot = None
        for r in range(row, n):
            if aug[r][col] != 0:
                pivot = r
                break
        if pivot is None:
            raise ValueError("singular matrix")

        aug[row], aug[pivot] = aug[pivot], aug[row]

        inv = gf_inv(aug[row][col])
        aug[row] = [gf_mul(x, inv) for x in aug[row]]

        for r in range(n):
            if r != row and aug[r][col] != 0:
                factor = aug[r][col]
                aug[r] = [aug[r][c] ^ gf_mul(factor, aug[row][c]) for c in range(n + 1)]

        row += 1

    return bytes(aug[i][n] for i in range(n))

def recv_guess_result(io):
    line = io.recvline()
    if b"Wrong. The number was " in line:
        return int(line.strip().split()[-1].rstrip(b"."))
    if b"Correct. Current streak:" in line:
        # if your dummy guess randomly hit, that output was 0
        return 0
    raise ValueError(f"unexpected line: {line!r}")

def oracle_encrypt(io, rng, pt):
    nxt = rng.getrandbits(32)
    io.recvuntil(b"Predict the next number or type -1 to exit: ")
    io.sendline(str(nxt).encode())

    io.recvuntil(b"[1] encrypt, [2] decrypt: ")
    io.sendline(b"1")
    io.recvuntil(b"Input plaintext to encrypt in hex: ")
    io.sendline(pt.hex().encode())

    io.recvuntil(b"enc(plaintext) = ")
    return bytes.fromhex(io.recvline().strip().decode())

def main():
    context.log_level = "error"
    io = remote(HOST, PORT)

    outputs = []
    for _ in range(LEAKS):
        io.recvuntil(b"Guess the next number: ")
        io.sendline(b"0")
        outputs.append(recv_guess_result(io))
        io.recvuntil(b"Attempts used: ")
        io.recvline()

    seed = pick_seed(outputs)
    print(f"[+] recovered seed: {seed.hex()}")

    rng = random.Random()
    rng.seed(seed)
    for x in outputs:
        assert rng.getrandbits(32) == x

    for streak in range(1, 4):
        io.recvuntil(b"Guess the next number: ")
        io.sendline(str(rng.getrandbits(32)).encode())
        io.recvuntil(f"Correct. Current streak: {streak}".encode())

    io.recvuntil(b"Encrypted Secret: ")
    secret_enc = bytes.fromhex(io.recvline().strip().decode())
    print(f"[+] secret_enc = {secret_enc.hex()}")

    base = oracle_encrypt(io, rng, b"\x00" * 16)

    cols = []
    for i in range(16):
        pt = bytearray(16)
        pt[i] = 1
        ct = oracle_encrypt(io, rng, bytes(pt))
        cols.append(bytes(c ^ d for c, d in zip(ct, base)))

    mat = [[cols[c][r] for c in range(16)] for r in range(16)]
    rhs = [c ^ d for c, d in zip(secret_enc, base)]
    secret = solve_gf256(mat, rhs)
    print(f"[+] recovered secret: {secret.hex()}")

    nxt = rng.getrandbits(32)
    io.recvuntil(b"Predict the next number or type -1 to exit: ")
    io.sendline(str(nxt).encode())
    io.recvuntil(b"[1] encrypt, [2] decrypt: ")
    io.sendline(b"1")
    io.recvuntil(b"Input plaintext to encrypt in hex: ")
    io.sendline(secret.hex().encode())

    print(io.recvall(timeout=2).decode(errors="ignore"))

if __name__ == "__main__":
    main()
```

- We executed the solver script and get the flag

![Screenshot 45](assets/screenshot-045.png)
