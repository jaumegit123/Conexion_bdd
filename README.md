# Conexion_bdd
Código para conectarse a cualquier base de datos (unas cuantas). Con interfaz Swing, y con estructura de clases separadas para clarificar el código.

## Clase Conector
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Conector {

	public void Conexio(Finestra f1) {
		switch (f1.getComboBox().getSelectedItem().toString()) {
		case "MySQL":
			try {
				conectarMySQL(f1);
			} catch (ClassNotFoundException | SQLException e1) {
				e1.printStackTrace();
			}
			break;
		case "SQLite":
			try {
				conectarSQLite(f1);
			} catch (ClassNotFoundException | SQLException e2) {
				e2.printStackTrace();
			}
			break;
		case "Apache Derby":
			try {
				conectarApache(f1);
			} catch (ClassNotFoundException | SQLException e1) {
				e1.printStackTrace();
			}
			break;
		}
	}
	
	public void conectarMySQL(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("com.mysql.jdbc.Driver");
		Connection con = DriverManager
				.getConnection("jdbc:mysql://localhost:3306/" + f1.getRUTA_DB().getText(), f1.getUser().getText(), f1.getPass().getText());
		validar(con);
	}
	
	public void conectarSQLite(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("org.sqlite.JDBC");
		Connection con = DriverManager.getConnection("jdbc:sqlite:" + f1.getRUTA_DB().getText());
		validar(con);
	}

	public void conectarApache(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("org.apache.derby.jdbc.EmbeddedDriver");
		Connection con = DriverManager.getConnection("jdbc:derby:" + f1.getRUTA_DB().getText() + ";");
		validar(con);
	}

	/*
	public void conectarHSQLDB(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("org.hsqldb.jdbcDriver");
		Connection con = DriverManager.getConnection("jdbc:hsqldb:file:" + f1.getRUTA_DB().getText());
		validar(con);
	}

	public void conectarH2(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("org.h2.Driver");
		Connection con = DriverManager
				.getConnection("jdbc:h2:" + f1.getRUTA_DB().getText() + "," + f1.getUser().getText() + "," + f1.getPass().getText());
		validar(con);
	}

	public void conectarAccess(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("net.ucanaccess.jdbc.UcanaccessDriver");
		Connection con = DriverManager.getConnection("jdbc:ucanaccess:" + f1.getRUTA_DB().getText());
		validar(con);
	}
	
	public void conectarOracle(Finestra f1) throws ClassNotFoundException, SQLException {
		Class.forName("oracle.jdbc.driver.OracleDriver");
		Connection con = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:XE" + "," + f1.getUser().getText() + "," + f1.getPass().getText());
		validar(con);
	}
	*/
	
	public void validar(Connection con) throws SQLException {
		if (con != null) {
			System.out.println("Conexió establerta!");
			MainAct.conexioEstablerta(con);
		} else {
			System.out.println("Error en la conexió.");
		}
	}
	
}
```

## Clase Ventana
```java
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.border.EmptyBorder;
import javax.swing.JComboBox;
import javax.swing.JLabel;
import javax.swing.DefaultComboBoxModel;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;
import javax.swing.JButton;
import javax.swing.JTextField;
import javax.swing.SwingConstants;
import java.awt.Font;

public class Finestra extends JFrame {

	private JPanel contentPane;
	private JTextField RUTA_DB;
	private JTextField user;
	private JTextField pass;
	private JComboBox comboBox;

	public Finestra() {
		this.setTitle("Conectar a una base de dades");
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 460, 269);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		contentPane.setLayout(null);

		// CREACIÓ DE COMPONENTS
		comboBox = new JComboBox();
		comboBox.setFont(new Font("Calibri", Font.PLAIN, 16));
		comboBox.setModel(new DefaultComboBoxModel(new String[] {"MySQL", "SQLite", "Apache Derby"}));
		comboBox.setBounds(256, 56, 107, 20);
		contentPane.add(comboBox);

		JLabel lblNewLabel = new JLabel("Ruta DB:");
		lblNewLabel.setFont(new Font("Calibri", Font.PLAIN, 16));
		lblNewLabel.setHorizontalAlignment(SwingConstants.LEFT);
		lblNewLabel.setBounds(56, 31, 68, 14);
		contentPane.add(lblNewLabel);

		JButton btnConectar = new JButton("Conectar");
		btnConectar.setFont(new Font("Calibri", Font.PLAIN, 16));
		btnConectar.setBounds(257, 123, 100, 29);
		contentPane.add(btnConectar);

		RUTA_DB = new JTextField();
		RUTA_DB.setText("act_manejoconectores");
		RUTA_DB.setBounds(56, 56, 135, 20);
		contentPane.add(RUTA_DB);
		RUTA_DB.setColumns(10);

		JLabel lblDb = new JLabel("Base de datos:");
		lblDb.setFont(new Font("Calibri", Font.PLAIN, 16));
		lblDb.setHorizontalAlignment(SwingConstants.LEFT);
		lblDb.setBounds(256, 31, 107, 14);
		contentPane.add(lblDb);

		JLabel lblUsuari = new JLabel("User:");
		lblUsuari.setFont(new Font("Calibri", Font.PLAIN, 14));
		lblUsuari.setHorizontalAlignment(SwingConstants.RIGHT);
		lblUsuari.setBounds(86, 118, 38, 14);
		contentPane.add(lblUsuari);

		JLabel lblPassword = new JLabel("Password:");
		lblPassword.setFont(new Font("Calibri", Font.PLAIN, 14));
		lblPassword.setHorizontalAlignment(SwingConstants.RIGHT);
		lblPassword.setBounds(63, 149, 61, 14);
		contentPane.add(lblPassword);

		user = new JTextField();
		user.setText("root");
		user.setColumns(10);
		user.setBounds(134, 115, 86, 20);
		contentPane.add(user);

		pass = new JTextField();
		pass.setColumns(10);
		pass.setBounds(134, 146, 86, 20);
		contentPane.add(pass);

		btnConectar.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				Conector c = new Conector();
				c.Conexio(MainAct.frame);
			}
		});
	}

	public JTextField getRUTA_DB() {
		return RUTA_DB;
	}

	public JTextField getUser() {
		return user;
	}

	public JTextField getPass() {
		return pass;
	}

	public JComboBox getComboBox() {
		return comboBox;
	}

}
```

## Clase Ventana2
```java
import java.sql.Connection;
import java.sql.SQLException;
import java.text.ParseException;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.border.EmptyBorder;
import javax.swing.JLabel;
import javax.swing.JTextField;
import javax.swing.JButton;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;

public class Finestra2 extends JFrame {

	private JPanel contentPane;
	//Clientes
	private JTextField IDClientes;
	private JTextField nombre;
	private JTextField direccion;
	private JTextField poblacion;
	private JTextField tlf;
	private JTextField dni;
	//Productos
	private JTextField IDProductos;
	private JTextField descripcion;
	private JTextField stockAnual;
	private JTextField stockMin;
	private JTextField PVP;
	//Ventas
	private JTextField IDVentas;
	private JTextField fechaVenta;
	private JTextField IDCliente_fk;
	private JTextField IDProducto_fk;
	private JTextField Cantidad;

	public Finestra2(Connection con) throws SQLException {
		this.setTitle("Introduïr valors en les taules");
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 748, 345);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		contentPane.setLayout(null);
		
		JLabel lblProductos = new JLabel("Stock m\u00EDnimo");
		lblProductos.setBounds(279, 140, 62, 14);
		contentPane.add(lblProductos);
		
		JLabel lblClientes = new JLabel("Clientes");
		lblClientes.setBounds(96, 40, 46, 14);
		contentPane.add(lblClientes);
		
		JLabel lblPvp_1 = new JLabel("PVP");
		lblPvp_1.setBounds(279, 165, 62, 14);
		contentPane.add(lblPvp_1);
		
		JLabel lblStockAnual_1 = new JLabel("Stock anual");
		lblStockAnual_1.setBounds(279, 115, 62, 14);
		contentPane.add(lblStockAnual_1);
		
		JLabel lblDescripcin_1 = new JLabel("Descripci\u00F3n");
		lblDescripcin_1.setBounds(279, 90, 62, 14);
		contentPane.add(lblDescripcin_1);
		
		JLabel label_3 = new JLabel("Productos");
		label_3.setBounds(317, 40, 62, 14);
		contentPane.add(label_3);
		
		JLabel lblPvp = new JLabel("Tel\u00E9fono");
		lblPvp.setBounds(55, 165, 62, 14);
		contentPane.add(lblPvp);
		
		JLabel lblId = new JLabel("ID");
		lblId.setBounds(55, 65, 62, 14);
		contentPane.add(lblId);
		
		JLabel lblDescripcin = new JLabel("Nombre");
		lblDescripcin.setBounds(55, 90, 62, 14);
		contentPane.add(lblDescripcin);
		
		JLabel lblStockAnual = new JLabel("Direcci\u00F3n");
		lblStockAnual.setBounds(55, 115, 62, 14);
		contentPane.add(lblStockAnual);
		
		JLabel lblStockMnimo = new JLabel("Poblaci\u00F3n");
		lblStockMnimo.setBounds(55, 140, 62, 14);
		contentPane.add(lblStockMnimo);
		
		JLabel lblDninif = new JLabel("DNI/NIF");
		lblDninif.setBounds(55, 190, 62, 14);
		contentPane.add(lblDninif);
		
		JLabel lblId_1 = new JLabel("ID");
		lblId_1.setBounds(279, 65, 62, 14);
		contentPane.add(lblId_1);
		
		IDClientes = new JTextField();
		IDClientes.setBounds(127, 62, 86, 20);
		contentPane.add(IDClientes);
		IDClientes.setColumns(10);
		
		nombre = new JTextField();
		nombre.setColumns(10);
		nombre.setBounds(127, 87, 86, 20);
		contentPane.add(nombre);
		
		direccion = new JTextField();
		direccion.setColumns(10);
		direccion.setBounds(127, 112, 86, 20);
		contentPane.add(direccion);
		
		poblacion = new JTextField();
		poblacion.setColumns(10);
		poblacion.setBounds(127, 137, 86, 20);
		contentPane.add(poblacion);
		
		tlf = new JTextField();
		tlf.setColumns(10);
		tlf.setBounds(127, 162, 86, 20);
		contentPane.add(tlf);
		
		dni = new JTextField();
		dni.setColumns(10);
		dni.setBounds(127, 187, 86, 20);
		contentPane.add(dni);
		
		IDProductos = new JTextField();
		IDProductos.setColumns(10);
		IDProductos.setBounds(351, 62, 86, 20);
		contentPane.add(IDProductos);
		
		descripcion = new JTextField();
		descripcion.setColumns(10);
		descripcion.setBounds(351, 87, 86, 20);
		contentPane.add(descripcion);
		
		stockAnual = new JTextField();
		stockAnual.setColumns(10);
		stockAnual.setBounds(351, 112, 86, 20);
		contentPane.add(stockAnual);
		
		stockMin = new JTextField();
		stockMin.setColumns(10);
		stockMin.setBounds(351, 137, 86, 20);
		contentPane.add(stockMin);
		
		PVP = new JTextField();
		PVP.setColumns(10);
		PVP.setBounds(351, 162, 86, 20);
		contentPane.add(PVP);
		
		JLabel lblVentas = new JLabel("Ventas");
		lblVentas.setBounds(544, 40, 62, 14);
		contentPane.add(lblVentas);
		
		JLabel label = new JLabel("ID");
		label.setBounds(498, 65, 62, 14);
		contentPane.add(label);
		
		JLabel lblDechaDeVenta = new JLabel("Fecha venta");
		lblDechaDeVenta.setBounds(498, 90, 62, 14);
		contentPane.add(lblDechaDeVenta);
		
		JLabel lblIdCliente = new JLabel("ID Cliente");
		lblIdCliente.setBounds(498, 115, 62, 14);
		contentPane.add(lblIdCliente);
		
		JLabel lblIdProducto = new JLabel("ID Producto");
		lblIdProducto.setBounds(498, 140, 62, 14);
		contentPane.add(lblIdProducto);
		
		JLabel lblCantidad = new JLabel("Cantidad");
		lblCantidad.setBounds(498, 165, 62, 14);
		contentPane.add(lblCantidad);
		
		IDVentas = new JTextField();
		IDVentas.setColumns(10);
		IDVentas.setBounds(563, 62, 86, 20);
		contentPane.add(IDVentas);
		
		fechaVenta = new JTextField();
		fechaVenta.setColumns(10);
		fechaVenta.setBounds(563, 87, 86, 20);
		contentPane.add(fechaVenta);
		
		IDCliente_fk = new JTextField();
		IDCliente_fk.setEditable(false);
		IDCliente_fk.setColumns(10);
		IDCliente_fk.setBounds(563, 112, 86, 20);
		contentPane.add(IDCliente_fk);
		
		IDProducto_fk = new JTextField();
		IDProducto_fk.setEditable(false);
		IDProducto_fk.setColumns(10);
		IDProducto_fk.setBounds(563, 137, 86, 20);
		contentPane.add(IDProducto_fk);
		
		Cantidad = new JTextField();
		Cantidad.setColumns(10);
		Cantidad.setBounds(563, 162, 86, 20);
		contentPane.add(Cantidad);
		
		JButton btnInsertar = new JButton("Insertar valors");
		btnInsertar.setBounds(306, 243, 131, 29);
		contentPane.add(btnInsertar);
		
		btnInsertar.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent arg0) {
				try {
					MainAct.insertarDatos(con);
				} catch (SQLException e) {
					e.printStackTrace();
				} catch (ParseException e) {
					e.printStackTrace();
				}
			}
		});
		
	}

	public JTextField getIDClientes() {
		return IDClientes;
	}

	public JTextField getNombre() {
		return nombre;
	}

	public JTextField getDireccion() {
		return direccion;
	}

	public JTextField getPoblacion() {
		return poblacion;
	}

	public JTextField getDni() {
		return dni;
	}

	public JTextField getIDProductos() {
		return IDProductos;
	}

	public JTextField getDescripcion() {
		return descripcion;
	}

	public JTextField getStockAnual() {
		return stockAnual;
	}

	public JTextField getStockMin() {
		return stockMin;
	}

	public JTextField getPVP() {
		return PVP;
	}

	public JTextField getIDVentas() {
		return IDVentas;
	}

	public JTextField getFechaVenta() {
		return fechaVenta;
	}

	public JTextField getCantidad() {
		return Cantidad;
	}

	public JTextField getTlf() {
		return tlf;
	}

}
```

## Clase MainAct
Aquí probamos unos inserts en las tablas correspondientes.
```java
import java.awt.EventQueue;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.text.ParseException;
import java.text.SimpleDateFormat;  
import java.util.Date;

import javax.swing.JOptionPane;  

public class MainAct {
	
	protected static Finestra frame;
	protected static Finestra2 frame2;
	
	static String queryClientes = "insert into clientes (id, nombre, direccion, poblacion, telef, nif) values (?, ?, ?, ?, ?, ?);";
	static String queryProductos = "insert into productos (id, descripcion, stock_anual, stock_minimo, pvp) values (?, ?, ?, ?, ?);";
	static String queryVentas = "insert into ventas (id, fecha_venta, id_cliente, id_producto, cantidad) values (?, ?, ?, ?, ?);";
	
	public static void main(String[] args) {

		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					frame = new Finestra();
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
		
	}
	
	public static void conexioEstablerta(Connection con) throws SQLException {
		frame2 = new Finestra2(con);
		frame2.setVisible(true);
	}
	
	public static void insertarDatos(Connection con) throws SQLException, ParseException {
		PreparedStatement insertar;
		
		try {
			//Productos
			insertar = con.prepareStatement(queryProductos);
			insertar.setInt(1, Integer.parseInt(frame2.getIDProductos().getText()));
			insertar.setString(2, frame2.getDescripcion().getText());
			insertar.setInt(3, Integer.parseInt(frame2.getStockAnual().getText()));
			insertar.setInt(4, Integer.parseInt(frame2.getStockMin().getText()));
			insertar.setInt(5, Integer.parseInt(frame2.getPVP().getText()));
			insertar.executeUpdate();
			
			//Clientes
			insertar = con.prepareStatement(queryClientes);
			insertar.setInt(1, Integer.parseInt(frame2.getIDClientes().getText()));
			insertar.setString(2, frame2.getNombre().getText());
			insertar.setString(3, frame2.getDireccion().getText());
			insertar.setString(4, frame2.getPoblacion().getText());
			insertar.setString(5, frame2.getTlf().getText());
			insertar.setString(6, frame2.getDni().getText());
			insertar.executeUpdate();
			
			//Ventas
			Date date1 = new SimpleDateFormat("dd/MM/yyyy").parse(frame2.getFechaVenta().getText());
			java.sql.Date sqlFecha = new java.sql.Date(date1.getTime());
			
			insertar = con.prepareStatement(queryVentas);
			insertar.setInt(1, Integer.parseInt(frame2.getIDVentas().getText()));
			insertar.setDate(2, sqlFecha);
			insertar.setInt(3, Integer.parseInt(frame2.getIDClientes().getText()));
			insertar.setInt(4, Integer.parseInt(frame2.getIDProductos().getText()));
			insertar.setInt(5, Integer.parseInt(frame2.getCantidad().getText()));
			insertar.executeUpdate();
		} catch (NumberFormatException e) {
			JOptionPane.showMessageDialog(frame, "Has introduït caràcters en un espai númeric!");
		}
		
	}
	
}
```
