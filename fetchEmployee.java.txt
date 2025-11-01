import java.sql.*;

public class FetchEmployee {
    public static void main(String[] args) throws Exception {
        String url = "jdbc:mysql://localhost:3306/my_db";
        String user = "root";
        String pass = "";

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");

            Connection con = DriverManager.getConnection(url, user, pass);

            Statement stmt = con.createStatement();

            String query = "SELECT * FROM employee";

            ResultSet rs = stmt.executeQuery(query);

            System.out.println("EmpID\tName\t\tSalary");
            while (rs.next()) {
                System.out.println(rs.getInt("empID") + "\t" + rs.getString("name") + "\t" + rs.getInt("salary"));
            }

            con.close();
        } catch (Exception e) {
            System.out.println("Exception: " + e);
        }
    }
}
