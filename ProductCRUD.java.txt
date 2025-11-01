import java.util.*;
import java.sql.*;

public class ProductCRUD {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/my_db";
        String user = "root";
        String pass = "";

        try (Connection con = DriverManager.getConnection(url, user, pass);
                Scanner sc = new Scanner(System.in)) {
            Class.forName("com.mysql.cj.jdbc.Driver");
            int choice;
            do {
                System.out.println("\n1. Insert\n2. Display\n3. Update\n4. Delete\n5. Exit");
                System.out.println("Enter your choice: ");
                choice = sc.nextInt();

                switch (choice) {
                    case 1 -> insertProduct(con, sc);
                    case 2 -> displayProducts(con);
                    case 3 -> updateProduct(con, sc);
                    case 4 -> deleteProduct(con, sc);
                }
            } while (choice != 5);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    static void insertProduct(Connection con, Scanner sc) throws SQLException {
        System.out.println("Enter ID, Name, Price, Quantity");
        int id = sc.nextInt();
        String name = sc.next();
        double price = sc.nextDouble();
        int qty = sc.nextInt();

        String query = "INSERT INTO Product VALUES (?, ?, ?, ?)";
        PreparedStatement ps = con.prepareStatement(query);
        ps.setInt(1, id);
        ps.setString(2, name);
        ps.setDouble(3, price);
        ps.setInt(4, qty);
        ps.executeUpdate();
        System.out.println("Product Inserted Successfully!");
    }

    static void displayProducts(Connection con) throws SQLException {
        Statement stmt = con.createStatement();
        ResultSet rs = stmt.executeQuery("SELECT * FROM Product");
        System.out.println("ID\tName\t\tPrice\tQuantity");
        while (rs.next()) {
            System.out.println(rs.getInt(1) + "\t" + rs.getString(2) + "\t\t" + rs.getDouble(3) + "\t" + rs.getInt(4));
        }
    }

    static void updateProduct(Connection con, Scanner sc) throws SQLException {
        con.setAutoCommit(false);
        try {
            System.out.println("Enter Product ID to Update: ");
            int id = sc.nextInt();
            System.out.println("Enter the new price: ");
            double price = sc.nextDouble();

            String query = "UPDATE Product SET Price=? WHERE ProductID=?";
            PreparedStatement ps = con.prepareStatement(query);
            ps.setDouble(1, price);
            ps.setInt(2, id);
            ps.executeUpdate();
            con.commit();
            System.out.println("Update Successful!");
        } catch (Exception e) {
            con.rollback();
            System.out.println("Update Failed! Rolled Back!");
        } finally {
            con.setAutoCommit(true);
        }
    }

    static void deleteProduct(Connection con, Scanner sc) throws SQLException {
        con.setAutoCommit(false);
        try {
            System.out.println("Enter Product ID to delete: ");
            int id = sc.nextInt();

            String query = "DELETE FROM Product WHERE ProductID=?";
            PreparedStatement ps = con.prepareStatement(query);
            ps.setInt(1, id);
            ps.executeUpdate();
            con.commit();
            System.out.println("Deleted Successfully!");
        } catch (Exception e) {
            con.rollback();
            System.out.println("Delete Failed! Rolled Back!");
        } finally {
            con.setAutoCommit(true);
        }
    }
}
