package MVC.controller;
import MVC.model.Student;
import java.sql.*;
import java.util.*;

public class StudentDAO {
    Connection con;

    public StudentDAO() throws Exception {
        Class.forName("com.mysql.cj.jdbc.Driver");
        con = DriverManager.getConnection("jdbc:mysql://localhost:3306/my_db", "root", "");
    }

    public void addStudent(Student s) throws SQLException {
        String sql = "INSERT INTO Student VALUES (?, ?, ?, ?)";
        PreparedStatement ps = con.prepareStatement(sql);
        ps.setInt(1, s.getId());
        ps.setString(2, s.getName());
        ps.setString(3, s.getDepartment());
        ps.setDouble(4, s.getMarks());
        ps.executeUpdate();
        System.out.println("Student added successfully!");
    }

    public List<Student> getAllStudents() throws SQLException {
        List<Student> list = new ArrayList<>();
        ResultSet rs = con.createStatement().executeQuery("SELECT * FROM Student");
        while (rs.next()) {
            list.add(new Student(rs.getInt(1), rs.getString(2), rs.getString(3), rs.getDouble(4)));
        }
        return list;
    }

    public void updateStudent(int id, double marks) throws SQLException {
        String sql = "UPDATE Student SET Marks=? WHERE StudentID=?";
        PreparedStatement ps = con.prepareStatement(sql);
        ps.setDouble(1, marks);
        ps.setInt(2, id);
        ps.executeUpdate();
        System.out.println("Student marks updated!");
    }

    public void deleteStudent(int id) throws SQLException {
        String sql = "DELETE FROM Student WHERE StudentID=?";
        PreparedStatement ps = con.prepareStatement(sql);
        ps.setInt(1, id);
        ps.executeUpdate();
        System.out.println("Student deleted successfully!");
    }
}
