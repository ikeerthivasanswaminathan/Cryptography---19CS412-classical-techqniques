## Cryptography---19CS412-classical-techqniques

##  Exp 1 - Caeser Cipher using with different key values

## AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.

## ALGORITHM :

Step 1 : Read the plain text from the user.

Step 2 : Read the key value from the user

Step 3 : If the key is positive then encrypt the text by adding the key with each character in the plain text

Step 4 : Else subtract the key from the plain text.

Step 5 : Display the cipher text obtained above.

## PROGRAM:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main() 
{
    char plain[100], cipher[100];
    int key, i, length;
    
    printf("Enter the plain text: ");
    scanf("%s", plain);
    printf("Enter the key value: ");
    scanf("%d", &key);

    length = strlen(plain);
    
    // Encryption
    printf("\nPLAIN TEXT: %s\n", plain);
    printf("\nENCRYPTED TEXT: ");
    
    for (i = 0; i < length; i++) 
    {
        if (isupper(plain[i])) 
        {
            cipher[i] = (plain[i] - 'A' + key) % 26 + 'A';
        } 
        else if (islower(plain[i])) 
        {
            cipher[i] = (plain[i] - 'a' + key) % 26 + 'a';
        }
        else 
        {
            cipher[i] = plain[i]; // Handle non-alphabetic characters
        }
        printf("%c", cipher[i]);
    }
    
    cipher[length] = '\0'; // Null-terminate the cipher text string
    // Decryption
    printf("\n\nDECRYPTED TEXT: ");
    for (i = 0; i < length; i++) 
    {
        if (isupper(cipher[i])) 
        {
            plain[i] = (cipher[i] - 'A' - key + 26) % 26 + 'A';
        } 
        else if (islower(cipher[i])) 
        {
            plain[i] = (cipher[i] - 'a' - key + 26) % 26 + 'a';
        } 
        else 
        {
            plain[i] = cipher[i]; // Handle non-alphabetic characters
        }
        printf("%c", plain[i]);
    }
    plain[length] = '\0'; // Null-terminate the plain text string
    return 0;
}
```

## OUTPUT:

Simulating Caesar Cipher

![exp1output](https://github.com/user-attachments/assets/45813b0e-391d-4630-b9a7-0908b2d0782c)

## RESULT:
The program for Caesar Cipher is executed successfully.
