# CipherToolkit
```bash
import java.util.Scanner;

public class CipherToolkit {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\n=== Classical Cipher Toolkit ===");
            System.out.println("1. Caesar Cipher");
            System.out.println("2. Affine Cipher");
            System.out.println("3. Playfair Cipher");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            int choice = sc.nextInt();
            sc.nextLine();

            if (choice == 1) {
                // Caesar Cipher
                System.out.print("Enter text: ");
                String text = sc.nextLine();
                System.out.print("Enter shift (number): ");
                int shift = sc.nextInt();
                sc.nextLine();
                System.out.print("Encrypt or Decrypt? (e/d): ");
                String ed = sc.nextLine();

                if (ed.equalsIgnoreCase("d")) shift = -shift;

                String result = "";
                for (int i = 0; i < text.length(); i++) {
                    char c = text.charAt(i);
                    if (Character.isLetter(c)) {
                        char base = Character.isUpperCase(c) ? 'A' : 'a';
                        result += (char)((c - base + shift + 26) % 26 + base);
                    } else {
                        result += c;
                    }
                }
                System.out.println("Result: " + result);

            } else if (choice == 2) {
                // Affine Cipher
                System.out.print("Enter text: ");
                String text = sc.nextLine();
                System.out.print("Enter key 'a' (coprime with 26): ");
                int a = sc.nextInt();
                System.out.print("Enter key 'b': ");
                int b = sc.nextInt();
                sc.nextLine();
                System.out.print("Encrypt or Decrypt? (e/d): ");
                String ed = sc.nextLine();

                String result = "";
                if (ed.equalsIgnoreCase("e")) {
                    for (int i = 0; i < text.length(); i++) {
                        char c = text.charAt(i);
                        if (Character.isLetter(c)) {
                            char base = Character.isUpperCase(c) ? 'A' : 'a';
                            result += (char)((a * (c - base) + b) % 26 + base);
                        } else {
                            result += c;
                        }
                    }
                } else {
                    // Find modular inverse of a
                    int a_inv = -1;
                    for (int i = 0; i < 26; i++) {
                        if ((a * i) % 26 == 1) {
                            a_inv = i;
                            break;
                        }
                    }
                    if (a_inv == -1) {
                        System.out.println("Invalid key 'a', no modular inverse!");
                        continue;
                    }
                    for (int i = 0; i < text.length(); i++) {
                        char c = text.charAt(i);
                        if (Character.isLetter(c)) {
                            char base = Character.isUpperCase(c) ? 'A' : 'a';
                            result += (char)((a_inv * ((c - base) - b + 26)) % 26 + base);
                        } else {
                            result += c;
                        }
                    }
                }
                System.out.println("Result: " + result);

            } else if (choice == 3) {
                // Playfair Cipher
                System.out.print("Enter text (remove spaces, replace J with I): ");
                String text = sc.nextLine().toUpperCase().replace(" ", "").replace("J","I");
                System.out.print("Enter key (letters only, J -> I): ");
                String key = sc.nextLine().toUpperCase().replace("J","I");

                // Build 5x5 matrix
                String matrix = "";
                for (int i = 0; i < key.length(); i++) {
                    char c = key.charAt(i);
                    if (matrix.indexOf(c) == -1 && Character.isLetter(c)) matrix += c;
                }
                for (char c = 'A'; c <= 'Z'; c++) {
                    if (c != 'J' && matrix.indexOf(c) == -1) matrix += c;
                }

                // Prepare pairs
                StringBuilder pairs = new StringBuilder();
                int i = 0;
                while (i < text.length()) {
                    char a = text.charAt(i);
                    char b = 'X';
                    if (i + 1 < text.length()) {
                        b = text.charAt(i+1);
                        if (a == b) {
                            b = 'X';
                            i++;
                        } else {
                            i += 2;
                        }
                    } else {
                        i++;
                    }
                    pairs.append(a).append(b);
                }

                System.out.print("Encrypt or Decrypt? (e/d): ");
                String ed = sc.nextLine();
                StringBuilder result = new StringBuilder();

                for (i = 0; i < pairs.length(); i += 2) {
                    char a = pairs.charAt(i);
                    char b = pairs.charAt(i+1);
                    int a_row = matrix.indexOf(a)/5;
                    int a_col = matrix.indexOf(a)%5;
                    int b_row = matrix.indexOf(b)/5;
                    int b_col = matrix.indexOf(b)%5;

                    if (a_row == b_row) {
                        if (ed.equalsIgnoreCase("e")) {
                            result.append(matrix.charAt(a_row*5 + (a_col+1)%5));
                            result.append(matrix.charAt(b_row*5 + (b_col+1)%5));
                        } else {
                            result.append(matrix.charAt(a_row*5 + (a_col+4)%5));
                            result.append(matrix.charAt(b_row*5 + (b_col+4)%5));
                        }
                    } else if (a_col == b_col) {
                        if (ed.equalsIgnoreCase("e")) {
                            result.append(matrix.charAt(((a_row+1)%5)*5 + a_col));
                            result.append(matrix.charAt(((b_row+1)%5)*5 + b_col));
                        } else {
                            result.append(matrix.charAt(((a_row+4)%5)*5 + a_col));
                            result.append(matrix.charAt(((b_row+4)%5)*5 + b_col));
                        }
                    } else {
                        result.append(matrix.charAt(a_row*5 + b_col));
                        result.append(matrix.charAt(b_row*5 + a_col));
                    }
                }
                System.out.println("Result: " + result.toString());

            } else if (choice == 4) {
                System.out.println("Exiting...");
                break;
            } else {
                System.out.println("Invalid choice!");
            }
        }

        sc.close();
    }
}

```
