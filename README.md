# StudentDataProcessor
 Java application for managing student data and workload categories.

import java.io.*;

public class StudentDataProcessor {
    public static void main(String[] args) {
        // Define file paths for the input and output files
        String inputFilePath = "/Users/jakubzaruski/Desktop/students.txt";
        String outputFilePath = "/Users/jakubzaruski/Desktop/status.txt";

        try (BufferedReader reader = new BufferedReader(new FileReader(inputFilePath));
             PrintWriter writer = new PrintWriter(outputFilePath)) {

            String line;

            // Process each student's data from the input file
            while ((line = reader.readLine()) != null) {
                // Split the line to get the full name and then read the number of classes and student number
                String[] fullName = line.trim().split(" ");
                String numberOfClassesStr = reader.readLine().trim();
                String studentNumber = reader.readLine().trim();

                // Validate the student data
                if (isValidFullName(fullName) && isValidClassCount(numberOfClassesStr) && isValidStudentNumber(studentNumber)) {
                    // Convert the number of classes from String to integer
                    int numberOfClasses = Integer.parseInt(numberOfClassesStr);

                    // Write the valid data to the output file in the specified format
                    writer.println(studentNumber + " - " + fullName[1]); // fullName[1] is the last name
                    writer.println(getWorkloadDescription(numberOfClasses));
                } else {
                    // Output error message for invalid data
                    System.out.println("Invalid data for student: " + String.join(" ", fullName));
                }
            }
        } catch (IOException e) {
            // Handle exceptions related to file input/output operations
            System.err.println("Error: " + e.getMessage());
        }
    }

    // Method to validate full name format
    private static boolean isValidFullName(String[] fullName) {
        return fullName.length == 2 && fullName[0].matches("[A-Za-z]+") && fullName[1].matches("[A-Za-z0-9]+");
    }

    // Method to validate the number of classes (should be an integer between 1 and 8)
    private static boolean isValidClassCount(String countStr) {
        try {
            int count = Integer.parseInt(countStr);
            return count >= 1 && count <= 8;
        } catch (NumberFormatException e) {
            return false;
        }
    }

    // Method to validate the student number format
    private static boolean isValidStudentNumber(String number) {
        return number.matches("\\d{2}[A-Za-z]{2,}[0-9]*") && number.length() >= 6;
    }

    // Method to determine the workload description based on the number of classes
    private static String getWorkloadDescription(int numberOfClasses) {
        if (numberOfClasses == 1) {
            return "Very Light";
        } else if (numberOfClasses == 2) {
            return "Light";
        } else if (numberOfClasses >= 3 && numberOfClasses <= 5) {
            return "Part Time";
        } else {
            return "Full Time";
        }
    }
}