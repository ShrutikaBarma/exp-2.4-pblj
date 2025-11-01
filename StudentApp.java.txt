package MVC.view;
import MVC.controller.StudentDAO;
import MVC.model.Student;
import java.util.*;

public class StudentApp {
    public static void main(String[] args) {
        try {
            StudentDAO dao = new StudentDAO();
            Scanner sc = new Scanner(System.in);
            int choice;

            do {
                System.out.println("\n1. Add Student\n2. View All\n3. Update Marks\n4. Delete Student\n5. Exit");
                System.out.print("Enter choice: ");
                choice = sc.nextInt();

                switch (choice) {
                    case 1 -> {
                        System.out.print("Enter ID, Name, Dept, Marks: ");
                        Student s = new Student(sc.nextInt(), sc.next(), sc.next(), sc.nextDouble());
                        dao.addStudent(s);
                    }
                    case 2 -> dao.getAllStudents().forEach(st ->
                        System.out.println(st.getId() + "\t" + st.getName() + "\t" + st.getDepartment() + "\t" + st.getMarks())
                    );
                    case 3 -> {
                        System.out.print("Enter ID and new Marks: ");
                        dao.updateStudent(sc.nextInt(), sc.nextDouble());
                    }
                    case 4 -> {
                        System.out.print("Enter ID to delete: ");
                        dao.deleteStudent(sc.nextInt());
                    }
                }
            } while (choice != 5);
            sc.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
