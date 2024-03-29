# JAVA_MYSQL_OPERATIONS
# CRUD OPERATIONS WIHTOUT CLASS
# Connection 
import java.sql.Connection;
import java.sql.DriverManager;
public class Connection_Name {
    public static String SERVER_NAME = "localhost";
    public static String MYSQL_PORT = "3306";
    public static String DB_NAME = "database_name";
    public static String UID = "username";
    public static String PWD = "password";
    public static String URL = "jdbc:mysql://"+SERVER_NAME+":"+MYSQL_PORT+"/"+DB_NAME+" ?useUnicode=true&characterEncoding=utf-8";
    private static Connection conn;
    public static Connection getConnection(){
        try {
            conn = DriverManager.getConnection(URL, UID, PWD);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return conn;
    }
}

# Import Packages
	import Class.ConnectionDB_NAME;
	import java.sql.Connection;
	import java.sql.PreparedStatement;
	import java.sql.ResultSet;

# Add New
o	For clear data when add new data on form
    private void clearData(){
        txt_name_id.setText("");
    }
o	Forget latest ID from database. 
    private void getMax(){
        sql = "SELECT MAX(id) alias_name FROM db_name";
        try {
            pst = cn.prepareStatement(sql);
            rs = pst.executeQuery();
            if (rs.next()) {
                Integer i = rs.getInt("cat_id");
                Integer j = i +1;
                txt_id_name.setText(j.toString());
            }else{
                txt_id_name.setText("1");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

# Get data and form load.
    sql = "SELECT * FROM `example_db`";
    try {
        pst = cn.prepareStatement(sql);
        rs = pst.executeQuery();
        model = DbUtils.resultSetToTableModel(rs);
        txt_jTable.setModel(model);
    } catch (Exception e) {
        e.printStackTrace();
    }

# Insert 
o	Get data from input form. 
        String example_name = txt_example _name.getText();
		sql = "INSERT INTO `example_db`(`example_name`) VALUES ('"+ example_name +"')";
o	Save to database. 
        try {
            pst = cn.prepareStatement(sql);
            int rows = pst.executeUpdate();
            if(rows > 0){
                JOptionPane.showMessageDialog(null, "Inserted!");
                formWindowOpened(null);
            }else{
                JOptionPane.showMessageDialog(null, "Failed!");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

# Get data from jTable for paste data to form for update and delete
    int rows = txt_jTable.getSelectedRow();
    txt_example_id.setText(model.getValueAt(rows, 0).toString());
	txt_example_name.setText(model.getValueAt(rows, 0).toString());

# Update 
    sql = "UPDATE `example_db` SET `example_name`=? WHERE id=?";
    try {
        pst = cn.prepareStatement(sql);
        pst.setString(1, txt_example_name1.getText());
        pst.setString(2, txt_example_name2.getText());
        int rows = pst.executeUpdate();
        if(rows > 0){
            JOptionPane.showMessageDialog(null, "Updated!");
            formWindowOpened(null);
        }else{
            JOptionPane.showMessageDialog(null, "Failed!");
        }
    } catch (Exception e) {
        e.printStackTrace();
    }

# Delete 
  	sql = "DELETE FROM `db_name` WHERE id=?";
	int opt = JOptionPane.showConfirmDialog(null, "Are you sure to delete?", "Delete", JOptionPane.YES_NO_OPTION);
	if(opt == 0){
	    try {
	        pst = cn.prepareStatmentCall(sql);
	        pst.setString(1, txt_example_id.getText());
	        int rows  = pst.executeUpdate();
	            if(rows > 0){
	                // delete message
	                formWindowOpened(null);
	            }
	    } catch (Exception e) {
	        e.printStackTrace();
	    }
	}

# Close or Exit 
	WindowEvent winclosevent = new WindowEvent(this, WindowEvent.WINDOW_CLOSING);
	Toolkit.getDefaultToolkit().getSystemEventQueue().postEvent(winclosevent);







