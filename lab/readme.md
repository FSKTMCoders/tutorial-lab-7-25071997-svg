# Lab
import java.io.*;
import java.net.URL;
import java.net.URLConnection;
import java.util.*;

public class Lab7 {

    public static void main(String[] args) {
        question1(); 
        // question2();
        // question3();
        // question4();
        // question5();
        // question6(); 
    }

    public static void question1() {
        String fileName = "coursename.dat";
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(fileName))) {
            Map<String, String> courses = new HashMap<>();
            courses.put("WXES1116", "Programming I");
            courses.put("WXES1115", "Data Structure");
            courses.put("WXES1110", "Operating System");
            courses.put("WXES1112", "Computing Mathematics I");
            out.writeObject(courses);
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }

        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream(fileName))) {
            @SuppressWarnings("unchecked")
            Map<String, String> readCourses = (Map<String, String>) in.readObject();
            
            Scanner sc = new Scanner(System.in);
            System.out.print("Enter a course code: ");
            String searchCode = sc.next();

            if (readCourses.containsKey(searchCode)) {
                System.out.println("Course Name: " + readCourses.get(searchCode));
            } else {
                System.out.println("Course code not found.");
            }
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    public static void question2() {
        try {
            URL u = new URL("http://www.fsktm.um.edu.my"); 
            URLConnection cnn = u.openConnection();
            InputStream stream = cnn.getInputStream();
            Scanner in = new Scanner(stream);
            PrintWriter out = new PrintWriter(new FileWriter("index.htm"));

            while (in.hasNextLine()) {
                out.println(in.nextLine());
            }
            
            in.close();
            out.close();
        } catch (IOException e) {
            System.out.println("IO Error: " + e.getMessage());
        }
    }

    public static void question3() {
        try (Scanner in = new Scanner(new File("original.txt"));
             PrintWriter out = new PrintWriter(new FileWriter("reverse.txt"))) {
            
            while (in.hasNextLine()) {
                String line = in.nextLine();
                String reversed = new StringBuilder(line).reverse().toString();
                out.println(reversed);
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    public static void question4() {
        int charCount = 0;
        int wordCount = 0;
        int lineCount = 0;

        try (Scanner in = new Scanner(new File("original.txt"))) {
            while (in.hasNextLine()) {
                String line = in.nextLine();
                lineCount++;
                charCount += line.length();
                if (!line.trim().isEmpty()) {
                    wordCount += line.split("\\s+").length;
                }
            }
            System.out.printf("Lines: %d, Words: %d, Characters: %d\n", lineCount, wordCount, charCount);
        } catch (FileNotFoundException e) {
            System.out.println("File not found.");
        }
    }

    public static void question5() {
        try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("person.dat"))) {
            out.writeInt(3);
            out.writeUTF("Ali"); out.writeInt(25); out.writeChar('M');
            out.writeUTF("Siti"); out.writeInt(23); out.writeChar('F');
            out.writeUTF("Ah Meng"); out.writeInt(24); out.writeChar('M');
        } catch (IOException e) {}

        List<Person> people = new ArrayList<>();

        try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("person.dat"))) {
            int totalRecords = in.readInt();
            
            for (int i = 0; i < totalRecords; i++) {
                String name = in.readUTF();
                int age = in.readInt();
                char gender = in.readChar();
                people.add(new Person(name, age, gender));
            }

            Collections.sort(people, Comparator.comparing(p -> p.name));

            for (Person p : people) {
                System.out.println(p);
            }

        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
