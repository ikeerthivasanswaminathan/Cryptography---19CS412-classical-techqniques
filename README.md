## Cryptography---19CS412-classical-techqniques

## Exp 3 - Hill Cipher using with different key values

## AIM:

To develop a simple C program to implement Hill Cipher.

## ALGORITHM :

Step 1 : Read the plain text and key from the user.

Step 2 : Split the plain text into groups of length three

Step 3 : Arrange the keyword in a 3*3 matrix.

Step 4 : Multiply the two matrices to obtain the cipher text of length three.

Step 5 : Combine all these groups to get the complete cipher text.

## PROGRAM:

```
#include <stdio.h>
 #include <string.h>
 #include <ctype.h>  // Include the necessary header for toupper()
 int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
 int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };
 char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
 void encode(char a, char b, char c, char ret[4]) 
 {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;
    
    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];
    
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
 }
 void decode(char a, char b, char c, char ret[4]) 
 {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;
    
    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];
    ret[0] = key[(x % 26 + 26) % 26];
    ret[1] = key[(y % 26 + 26) % 26];
    ret[2] = key[(z % 26 + 26) % 26];
    ret[3] = '\0';
 }
 
 int main() 
 {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;
    strcpy(msg, "KEERTHIVASAN");
    printf("Simulation of Hill Cipher\n");
    printf("\nInput message : %s\n", msg);
    for (int i = 0; i < strlen(msg); i++) 
    {
        msg[i] = toupper(msg[i]);
    }
    // Remove spaces
    n = strlen(msg) % 3;
    // Append padding text X
    if (n != 0) 
    {
        for (int i = 1; i <= (3 - n); i++) 
        {
            strcat(msg, "X");
        }
    }
    printf("\nPadded message : %s\n", msg);
    for (int i = 0; i < strlen(msg); i += 3) 
    {
        char a = msg[i];
        char b = msg[i + 1];
        char c = msg[i + 2];
        char ret[4];
        encode(a, b, c, ret);
        strcat(enc, ret);
    }
    printf("\nEncoded message : %s\n", enc);
    for (int i = 0; i < strlen(enc); i += 3)
    {
        char a = enc[i];
        char b = enc[i + 1];
        char c = enc[i + 2];
        char ret[4];
        decode(a, b, c, ret);
        strcat(dec, ret);
    }
    printf("\nDecoded message : %s\n", dec);    
    return 0;
 }
```

## OUTPUT:

Simulating Hill Cipher

![Uploading exp3output.png…]()

## RESULT:
The program for Hill Cipher is executed successfully.
