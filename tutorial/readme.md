# Tutorial 
## 1
```java
import java.io.*;
import java.util.Random;
import java.util.Scanner;

public class TutorialQ1_Text {
    public static void main(String[] args) {
        String filename = "integer.txt";
        Random rand = new Random();

        try (PrintWriter out = new PrintWriter(new FileOutputStream(filename))) {
            for (int i = 0; i < 10; i++) {
                out.println(rand.nextInt(1001));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        try (Scanner sc = new Scanner(new FileInputStream(filename))) {
            int max = Integer.MIN_VALUE;
            while (sc.hasNextInt()) {
                int num = sc.nextInt();
                System.out.println(num);
                if (num > max) {
                    max = num;
                }
            }
            System.out.println("Largest integer: " + max);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
```java
import java.io.*;
import java.util.Random;

public class TutorialQ1_Binary {
    public static void main(String[] args) {
        String filename = "integer.dat";
        Random rand = new Random();

        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(filename))) {
            for (int i = 0; i < 10; i++) {
                out.writeInt(rand.nextInt(1001));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream(filename))) {
            int sum = 0;
            int count = 0;
            try {
                while (true) {
                    int num = in.readInt();
                    System.out.println(num);
                    sum += num;
                    count++;
                }
            } catch (EOFException e) {}
            
            double average = (count > 0) ? (double) sum / count : 0;
            System.out.println("Average: " + average);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
---

## 2
### a
```java
PrintWriter out = new PrintWriter(new FileOutputStream("d:\\data\\matrix.txt"));
```
### b
```java
try {
    PrintWriter out = new PrintWriter(new FileOutputStream("data.txt"));
    out.close();
} catch (FileNotFoundException e) {
    System.out.println("Problem with file output");
}
```
### c
```java
int num;
Scanner a = new Scanner(new FileInputStream("data.dat"));
num = a.nextInt();
a.close();
```
### d
```java
ObjectOutputStream o = new ObjectOutputStream(new FileOutputStream("data.dat"));
o.writeChar('A');
o.close();
```

---

## 3
```java
import java.io.*;
import java.util.Scanner;

public class TutorialQ3 {
    public static void main(String[] args) {
        String fileName = "data.txt";
        String sentence = "Hello World";

        try (PrintWriter out = new PrintWriter(new FileOutputStream(fileName))) {
            for (char c : sentence.toCharArray()) {
                String binary = Integer.toBinaryString(c);
                String formatted = String.format("%8s", binary).replace(' ', '0');
                out.print(formatted + " ");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        try (Scanner sc = new Scanner(new FileInputStream(fileName))) {
            while (sc.hasNext()) {
                String binaryStr = sc.next();
                int ascii = Integer.parseInt(binaryStr, 2);
                System.out.print((char) ascii);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
