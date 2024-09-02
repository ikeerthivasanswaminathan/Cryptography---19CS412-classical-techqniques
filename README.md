## Cryptography---19CS412-classical-techqniques

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

![exp2output](https://github.com/user-attachments/assets/8b24726a-5c65-401a-a4f2-af3eaacdeb95)

## RESULT:
The program for Playfair Cipher is executed successfully.
