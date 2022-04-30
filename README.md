```
public static void main(String[] args) {
    EventQueue.invokeLater(new Runnable() {
        public void run() {
            DropDown frame = new DropDown();
            frame.setVisible(true);
        }
	});
}
/**
 * Create the frame.
*/
public DropDown() {
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    setBounds(100, 100, 409, 243);
    setTitle("Player Stats");
    getContentPane().setLayout(null);

    // Combobox
    Vector Items = new Vector();

    Connection connect =  ConnectDB();
    Statement s = null;
	
	try {
		s = connect.createStatement();
		String sql = "SELECT * FROM  Player ORDER BY PlayerID ASC";
				
		ResultSet rec = s.executeQuery(sql);
		int row = 0;
		while((rec!=null) && (rec.next())){		
			Items.add(rec.getString("CustomerID") + " - " + rec.getString("Name"));
	    }
		rec.close();
	} catch (Exception e) {
		JOptionPane.showMessageDialog(null, e.getMessage());
		e.printStackTrace();
	}
	try {
		if(s != null) {
		  s.close();
			  connect.close();
		}
	} catch (SQLException e) {
		e.printStackTrace();
	}
	// Label Result
	final JLabel lblResult = new JLabel("Result");
	lblResult.setHorizontalAlignment(SwingConstants.CENTER);
	lblResult.setBounds(62, 154, 272, 14);
	getContentPane().add(lblResult);
		
	// Combobox
	DefaultComboBoxModel model = new DefaultComboBoxModel(Items);
	final JComboBox comboBox = new JComboBox(model);
	
	comboBox.addActionListener(new ActionListener() {
	
		public void actionPerformed(ActionEvent e) {
			lblResult.setText(comboBox.getSelectedItem().toString());
		}
	});
    comboBox.setBounds(113, 58, 167, 20);
    getContentPane().add(comboBox);	 
}

// Connection
private Connection ConnectDB() {
    try {
        Class.forName("com.mysql.jdbc.Driver");
        return  DriverManager.getConnection("jdbc:mysql://localhost/mydatabase" 
                + "?user=root&password=root");
    } catch (ClassNotFoundException e) {
          e.printStackTrace();
    } catch (SQLException e) {
          e.printStackTrace();
	  }
    return null;
}```
