## Cryptography---19CS412-classical-techqniques

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

```#include<stdio.h>
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

![exp5output](https://github.com/user-attachments/assets/3e50f772-354f-459f-8e6e-fe9a2878cc0c)

## RESULT:
The program for Rail Fence Cipher is executed successfully.
