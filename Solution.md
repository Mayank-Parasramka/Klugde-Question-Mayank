
# SOLUTION TO THE CHALLENGE

The solution basically consists of 3 Steps.

## STEP-1: Finding and Decoding Comment
In this step we apply
```bash
 exiftool spiral.jpg
```
and we get this in the comment:   **I5ZGKYLUEBLW64TLFQQE433XEB2HE6JAORXSAZDFMNXWIZJAORUGS4ZAKRUWMICSMR4GC2TKPJTSA5DCEBZEG5KRNRLGO3SONQ======**

This is Base 32  
we get:   
**Great Work, Now try to decode this Tif Rdxajjzg tb rCuQlVgnNl**

Now this was a custom cipher where the shift is in Fibonacci series, also hinted with the spiral picture.  
I used this code for its encryption
```python
def fibo(n_terms):

    # First two terms
    n_1 = 0
    n_2 = 1
    count = 1

    if n_terms == 0:
        return(n_1)
    elif n_terms == 1:
        return(n_2)
    else:
        while count < n_terms:
            nth = n_1 + n_2
            # At last, we will update values
            n_1 = n_2
            n_2 = nth
            count += 1
        return(n_2)

def encrypt_text(plaintext):
    ans = ""
    term_count = 0
    # iterate over the given text
    for i in range(len(plaintext)):
        n = fibo(term_count)
        ch = plaintext[i]

        # check if space is there then simply add space
        if ch == " ":
            ans += " "
        # check if a character is uppercase then encrypt it accordingly
        elif (ch.isupper()):
            ans += chr((ord(ch) + n - 65) % 26 + 65)
            term_count += 1
        # check if a character is lowercase then encrypt it accordingly
        else:
            ans += chr((ord(ch) + n - 97) % 26 + 97)
            term_count += 1

    return ans

plaintext = "The Password in sPiRaLliNg"
print("Plain Text is : " + plaintext)
print("Cipher Text is : " + encrypt_text(plaintext))
```
Similar code can be written for decryption.  
Hence the decrypted text would be:  
**The Password in spiRaLliNg**

## STEP-2: Using Steghide on Image
In this step we apply
```bash
steghide extract -sf spiral.jpg
```
with passphrase as **spiRaLliNg** and extract a txt file name fibo.txt.

## STEP-3: Using Stegsnow on fibo.txt
In this step we apply
```bash
stegsnow -C -p "spiRaLliNg" fibo.txt
```
and get this:  
**4wZ4z3PU9MoLy3RFqzjrzLs1juvyax6L2A4q7KbCzCKYN8RG9R6GV6KK8Fm8Nmdzvx**  
which is encoded in base 58.  
Decoding this will finally give out our flag:  
**kludgeCTF{f1B0n4Cc1_L0v3s_sPiR4L5_t00_011235811}**





