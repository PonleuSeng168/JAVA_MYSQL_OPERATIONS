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
	+ For clear data when add new data on form
	    private void clearData(){
	        txt_name_id.setText("");
	    }
	+ Forget latest ID from database. 
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
	+ Get data from input form. 
	        String example_name = txt_example _name.getText();
		sql = "INSERT INTO `example_db`(`example_name`) VALUES ('"+ example_name +"')";
	+ Save to database. 
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

# Get data from jTable for pass data to input form for update and delete
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

# CRUD OPERATIONS WIHT CLASS
	* First of all, going to create the class name first!
	* Import packages
		import java.sql.Connection;
		import java.sql.PreparedStatement;
		import java.sql.ResultSet;

	* Connection 
	    Connection cn;
	    PreparedStatement pst;
	    ResultSet rs;
	    String sql;

	* Declare Variable 
	    public String exampleId;
	    public String exampleName;

	 * Generate Getter and Setter 
		public String getExampleId() {
	        return productId;
	    }

	    public void SetExampleId(String exampleId) {
	        this.exampleId = exampleId;
	    }

# Insert, Update and Delete
    public boolean insert() {
    cn = ConnectionDB.getConnection();
    boolean b = false;
    sql = "INSERT INTO `example_db`(`category_id`, `supplier_id`) VALUES (?,?,?)";
    try {
        pst = cn.prepareStatement(sql);
        pst.setString(1, exampleId);
        pst.setString(2, exampleName);
        // For image 
        FileInputStream fis = new FileInputStream(new File(images));
        pst.setBinaryStream(3, fis);
        int row = pst.executeUpdate();
        if (row > 0) {
            b = true;
            } else {
                b = false;
            }
        } catch (Exception e) {
            e.printStackTrace();
            b = false;
        }
        return b;
    }

# Get data and form load 
    public TableModel selectRecord() {
    cn = ConnectionDB.getConnection();
    TableModel model = null;
		sql = "SELECT * FROM `example_db`";
        try {
            pst = cn.prepareStatement(sql);
            rs = pst.executeQuery();
            model = DbUtils.resultSetToTableModel(rs);
        } catch (Exception e) {
            model = null;
            e.printStackTrace();
        }
        return model;
    }

# Get data for jComboBox
    public ResultSet selectComboBox() {
        cn = ConnectionDB.getConnection();
        ResultSet rssup;
        sql = "SELECT CONCAT(id, \"-\", name) AS sup FROM `example_db`";
        try {
            pst = cn.prepareStatement(sql);
            rs = pst.executeQuery();
            rssup = rs;
        } catch (Exception e) {
            rssup = null;
            e.printStackTrace();
        }
        return rssup;
    }

# Search 
    public TableModel SeachByExample(String example) {
        cn = ConnectionDB.getConnection();
        TableModel model = null;
		sql = "SELECT * FROM `example_db` WHERE barcode LIKE '%" + example + "%'";
        try {
            pst = cn.prepareStatement(sql);
            rs = pst.executeQuery();
            model = DbUtils.resultSetToTableModel(rs);
        } catch (Exception e) {
            model = null;
            e.printStackTrace();
        }
        return model;
    }	

# Java Forms
# How to load data from class to jComboBox 
    ResultSet rss;
    rss = pro.selectJComBoBox();
    try {
        while (rss.next()) {
            String jcombox = rss.getString("jcombox");
            cbo_jcombox.addItem(jcombox);
        }
    } catch (Exception e) {
        e.printStackTrace();
    }

# How to load default Image 
	ImageIcon imageIcon = new ImageIcon(new ImageIcon(this.getClass().getResource("/image/No_Image_Available.jpg")).getImage().getScaledInstance(txt_example_img.getWidth(), txt_example_img.getHeight(), Image.SCALE_DEFAULT));
	txt_example_img.setIcon(imageIcon);	   

# How to hide column in jTable 
    void hideColumn() {
        TableColumnModel tcm = tbl_example.getColumnModel();
        tbl_products.removeColumn(tcm.getColumn(0));
        tbl_products.removeColumn(tcm.getColumn(1));
    }

# How to save data from input form to database 
    void subSetData() {
        pro.exampletId = txt_example_id.getText();

        String example = cbo_example.getSelectedItem().toString();
        String[] example1 = example.split("-");
        pro.exampleId = example1[0];

        BufferedImage img = null;
        try {
            img = ImageIO.read(new File(pathname));
        } catch (Exception e) {
            e.printStackTrace();
        }
        pro.images = pathname;
    }
    
# How to search data in form by KeyReleased event 
    if (cbo_find_example.getSelectedItem().equals("example")) {
        tbl_examples.setModel(pro.SeachByExample(txt_find_example.getText()));
    }









