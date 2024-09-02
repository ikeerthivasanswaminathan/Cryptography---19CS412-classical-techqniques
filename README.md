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

![exp1output](https://github.com/user-attachments/assets/f0ca5203-e463-4996-b4b9-19578f2d898a)

## RESULT:
The program for Caesar Cipher is executed successfully.

-------------------------------------------------------------------------------------------------------------------------

## Exp 2 - Playfair Cipher using with different key values

## AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

ALGORITHM :

Step 1 : Read the plain text from the user

Step 2 : Read the keyword from the user

Step 3 : Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

Step 4 : Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

step 5 : Display the obtained cipher text

## PROGRAM:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5

void generateKeyTable(char key[], char keyTable[SIZE][SIZE]) 
{
    int dicty[26] = {0};
    int i, j, k = 0, len = strlen(key);


    for (i = 0; i < len; i++) 
    {
        if (key[i] != 'j' && dicty[key[i] - 'a'] == 0) 
        {
            keyTable[k / SIZE][k % SIZE] = key[i];
            dicty[key[i] - 'a'] = 1;
            k++;
        }
    }


    for (i = 0; i < 26; i++) 
    {
        if (i != 9 && dicty[i] == 0) 
        { // skip 'j'
            keyTable[k / SIZE][k % SIZE] = (char)(i + 'a');
            k++;
        }
    }
}

void prepareText(char text[], char preparedText[]) 
{
    int i, j = 0, len = strlen(text);

    for (i = 0; i < len; i++) 
    {
        text[i] = tolower(text[i]);
        if (text[i] == 'j') 
        {
            text[i] = 'i';
        }
    }

    for (i = 0; i < len; i++) 
    {
        if (isalpha(text[i])) 
        {
            preparedText[j++] = text[i];
        }
    }

    preparedText[j] = '\0';


    for (i = 0; i < j; i += 2) 
    {
        if (preparedText[i] == preparedText[i + 1]) 
        {
            memmove(preparedText + i + 2, preparedText + i + 1, j - i + 1);
            preparedText[i + 1] = 'x';
            j++;
        }
    }

    if (strlen(preparedText) % 2 != 0) 
    {
        preparedText[j++] = 'x';
        preparedText[j] = '\0';
    }
}

void searchPosition(char keyTable[SIZE][SIZE], char a, char b, int pos[]) 
{
    int i, j;

    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (i = 0; i < SIZE; i++) 
    {
        for (j = 0; j < SIZE; j++) 
        {
            if (keyTable[i][j] == a) 
            {
                pos[0] = i;
                pos[1] = j;
            }
            if (keyTable[i][j] == b) 
            {
                pos[2] = i;
                pos[3] = j;
            }
        }
    }
}

void encryptOrDecrypt(char text[], char keyTable[SIZE][SIZE], int mode) 
{
    int i, pos[4], len = strlen(text);

    for (i = 0; i < len; i += 2)
    {
        searchPosition(keyTable, text[i], text[i + 1], pos);
        if (pos[0] == pos[2])
        {
            text[i] = keyTable[pos[0]][(pos[1] + mode + SIZE) % SIZE];
            text[i + 1] = keyTable[pos[2]][(pos[3] + mode + SIZE) % SIZE];
        } 
        else if (pos[1] == pos[3]) 
        {
            text[i] = keyTable[(pos[0] + mode + SIZE) % SIZE][pos[1]];
            text[i + 1] = keyTable[(pos[2] + mode + SIZE) % SIZE][pos[3]];
        } 
        else 
        {
            text[i] = keyTable[pos[0]][pos[3]];
            text[i + 1] = keyTable[pos[2]][pos[1]];
        }
    }
}

int main()
{
    char key[30], text[100], preparedText[100], keyTable[SIZE][SIZE];
    int choice;
    printf("\nEnter the key: ");
    gets(key);
    generateKeyTable(key, keyTable);
    printf("\nEnter the text: ");
    gets(text);
    prepareText(text, preparedText);
    printf("\nEnter 1 to encrypt or 2 to decrypt: ");
    scanf("%d", &choice);
    if (choice == 1) 
    {
        encryptOrDecrypt(preparedText, keyTable, 1);  
        printf("\nEncrypted text: %s\n", preparedText);
    }
    else if (choice == 2) 
    {
        encryptOrDecrypt(preparedText, keyTable, -1); 
        printf("\nDecrypted text: %s\n", preparedText);
    } 
    else 
    {
        printf("\nInvalid choice!\n");
    }
    printf("\nEnter 1 to encrypt or 2 to decrypt: ");
    scanf("%d", &choice);
    return 0;
}
```

## OUTPUT:

![exp2output](https://github.com/user-attachments/assets/d7fd12dd-4068-46c1-8755-042d46802f17)

## RESULT:
The program for Playfair Cipher is executed successfully.

-------------------------------------------------------------------------------------------------------------------------

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

![exp3output](https://github.com/user-attachments/assets/d6456446-7aa1-490a-b793-f1e8d60218f4)

## RESULT:
The program for Hill Cipher is executed successfully.

-------------------------------------------------------------------------------------------------------------------------

## Exp 4 - Vigenere Cipher using with different key values

## AIM:

To develop a simple C program to implement Vigenere Cipher.

## ALGORITHM :

Step 1 : Arrange the alphabets in row and column of a 26*26 matrix

Step 2 : Circulate the alphabets in each row to position left such that the first letter is attached to last.

Step 3 : Repeat this process for all 26 rows and construct the final key matrix.

Step 4 : The keyword and the plain text is read from the user.

Step 5 : The characters in the keyword are repeated sequentially so as to match with that of the plain text.

Step 6 : Pick the first letter of the plain text and that of the keyword as the row indices and column indices respectively.

Step 7 : The junction character where these two meet forms the cipher character

Step 8 : Repeat the above steps to generate the entire cipher text.

## PROGRAM:

```
#include <stdio.h>
#include<conio.h>
#include <ctype.h>
#include <string.h>
void encipher();
void decipher();
int main()
{
    int choice;
    while(1)
    {
        printf("\n\n1. Encrypt Text");
        printf("\n2. Decrypt Text");
        printf("\n3. Exit");
        printf("\n\nEnter Your Choice : ");
        scanf("%d",&choice);
        if(choice == 3)
            exit(0);
        else if(choice == 1)
            encipher();
        else if(choice == 2)
            decipher();
        else
            printf("Please Enter Valid Option.");
    }
}

void encipher()
{
    unsigned int i,j;
    char input[50],key[10];
    printf("\nEnter Plain Text: ");
    scanf("%s",input);
    printf("\nEnter Key Value: ");
    scanf("%s",key);
    printf("\nResultant Cipher Text: ");
    for(i=0,j=0;i<strlen(input);i++,j++)
    {
        if(j>=strlen(key))
        { 
            j=0;
        }
    printf("%c",65+(((toupper(input[i])-65)+(toupper(key[j])-65))%26));
    }
}

void decipher()
{
    unsigned int i,j;
    char input[50],key[10];
    int value;
    printf("\nEnter Cipher Text: ");
    scanf("%s",input);
    printf("\nEnter the key value: ");
    scanf("%s",key);
    printf("\n");
    for(i=0,j=0;i<strlen(input);i++,j++)
    {
        if(j>=strlen(key))
        { 
            j=0;
        }
        value = (toupper(input[i])-64)-(toupper(key[j])-64);
        if( value < 0)
        {
            value = value * -1;
        }
        printf("%c",65 + (value % 26));
    }  
return 0;
}
```

## OUTPUT:

Simulating Vigenere Cipher

![exp4output](https://github.com/user-attachments/assets/f5128ffb-329c-423e-b4f1-2d46fe15847b)

## RESULT:
The program for Vigenere Cipher is executed successfully.

-------------------------------------------------------------------------------------------------------------------------

## Exp 5 - Rail Fence Cipher using with different key values

## AIM:

To develop a simple C program to implement Rail Fence Cipher.

## ALGORRITHM :

Step 1 : Read the Plain text.

Step 2 : Arrange the plain text in row columnar matrix format

Step 3 : Now read the keyword depending on the number of columns of the plain text

Step 4 : Arrange the characters of the keyword in sorted order and the corresponding columns of the plain text.

Step 5 : Read the characters row wise or column wise in the former order to get the

## PROGRAM:

```
#include<conio.h>
#include<string.h>
int main()
{
    int i,j,k,l;
    char a[20],c[20],d[20];
    printf("\n\t\t RAIL FENCE TECHNIQUE");
    printf("\n\nEnter the input string : ");
    gets(a);
    l=strlen(a);
    //Ciphering
    for(i=0,j=0;i<l;i++)
    {
        if(i%2==0)
        c[j++]=a[i];
    }
    for(i=0;i<l;i++)
    {
        if(i%2==1)
        c[j++]=a[i];
    }
    c[j]='\0';
    printf("\nCipher text after applying rail fence : ");
    printf("%s",c);
    //Deciphering
    if(l%2==0)
    {
        k=l/2;
    }
    else
    {
        k=(l/2)+1;
    }
    for(i=0,j=0;i<k;i++)
    {
        d[j]=c[i];
        j=j+2;
    }
    for(i=k,j=1;i<l;i++)
    {
        d[j]=c[i];
        j=j+2;
    }
    d[l]='\0';
    printf("\n\nText after decryption : ");
    printf("%s",d);
    return 0;
}
```

## OUTPUT:

![exp5output](https://github.com/user-attachments/assets/9b0e6a8e-1675-440f-8e6b-f55092bb1f97)

## RESULT:
The program for Rail Fence Cipher is executed successfully.
