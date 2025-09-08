## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:

```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateMatrix(char key[], char matrix[SIZE][SIZE]) {
    int dict[26] = {0};
    int i, j, k = 0;
    char ch;

    dict['J' - 'A'] = 1;


    for (i = 0; key[i]; i++) {
        ch = toupper(key[i]);
        if (ch < 'A' || ch > 'Z') continue;
        if (ch == 'J') ch = 'I';
        if (!dict[ch - 'A']) {
            matrix[k / SIZE][k % SIZE] = ch;
            dict[ch - 'A'] = 1;
            k++;
        }
    }

    for (ch = 'A'; ch <= 'Z'; ch++) {
        if (!dict[ch - 'A']) {
            matrix[k / SIZE][k % SIZE] = ch;
            dict[ch - 'A'] = 1;
            k++;
        }
    }
}

void search(char matrix[SIZE][SIZE], char ch, int pos[]) {
    if (ch == 'J') ch = 'I';
    for (int i = 0; i < SIZE; i++)
        for (int j = 0; j < SIZE; j++)
            if (matrix[i][j] == ch) {
                pos[0] = i;
                pos[1] = j;
                return;
            }
}

void encryptPair(char matrix[SIZE][SIZE], char a, char b, char result[]) {
    int pos1[2], pos2[2];
    search(matrix, a, pos1);
    search(matrix, b, pos2);

    if (pos1[0] == pos2[0]) { // same row
        result[0] = matrix[pos1[0]][(pos1[1] + 1) % SIZE];
        result[1] = matrix[pos2[0]][(pos2[1] + 1) % SIZE];
    } else if (pos1[1] == pos2[1]) { // same column
        result[0] = matrix[(pos1[0] + 1) % SIZE][pos1[1]];
        result[1] = matrix[(pos2[0] + 1) % SIZE][pos2[1]];
    } else { // rectangle
        result[0] = matrix[pos1[0]][pos2[1]];
        result[1] = matrix[pos2[0]][pos1[1]];
    }
}


int main() {
    char key[50], text[100], matrix[SIZE][SIZE], result[100];
    int i, len, k = 0;

    printf("Enter key: ");
    scanf("%s", key);

    generateMatrix(key, matrix);

    printf("\nPlayfair Matrix:\n");
    for (i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++)
            printf("%c ", matrix[i][j]);
        printf("\n");
    }

    printf("Enter plaintext: ");
    scanf("%s", text);
    len = strlen(text);


    for (i = 0; i < len; i += 2) {
        char a = toupper(text[i]);
        char b = (i + 1 < len) ? toupper(text[i + 1]) : 'X';
        if (a == b) b = 'X';
        char enc[2];
        encryptPair(matrix, a, b, enc);
        result[k++] = enc[0];
        result[k++] = enc[1];
    }
    result[k] = '\0';

    printf("Ciphertext: %s\n", result);

    return 0;
}

```

Output:

<img width="335" height="326" alt="Screenshot (448)" src="https://github.com/user-attachments/assets/342e7420-0097-4005-9622-874662d1745c" />


RESULT:

Program has been executed successfully
