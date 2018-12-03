# Tema 3. JDBC

## 1 Introducción

JDBC (Java Database Connectivy)API que permite la conexión entre Java y una BD relacional.
Se encuentra dentro del paquete *java.sql*.

Clases JDBC:

- Clase *DriverManager*: Establece la conexión con la base de datos.
- Interface *Connection*: Representa la conexión con la BD.
- Interface *Statement*: Ejecución de consultas SQL.
- Interface *PreparedStatement*: Ejecución de consultas preparadas y procedimientos almacenados.
- Interface *ResultSet*: Manipulación de registros en consultas de tipo `select`.
- Interface *ResultSetMetadata*: Información sobre la estructura de los datos.

## 1.1 Driver JDBC

**Driver**: Conjunto de clases que implementan las interfaces JDBC. Proporciona la comunicacioón entre la aplicacioón Java y la BD.

## 1.2 Acceso a BD

### 1) Importar paquetes

`import java.sql.*;`

### 2) Cargar el driver

`Class.forName(String driver);`

```java
// Driver JDBC para MySQL
Class.forName("com.mysql.jdbc.Driver");
```

### 3) Crear la conexión

`DriverManager.getConnection(String url);`

url: *jdbc:mysql://ordenador/DB*

Aplicando esto, nos queda:

```java
// usuario: root
// password: 1daw
Connection conexion = DriverManager.getConnection("jdbc:mysql://localhost/prueba", "root", "1daw");
```

### 4) Ejecutar la consulta

```java
Statement stmt = conexion.createStatement();
```

Una vez creado el objecto statement podemos usar métodos para enviar la consulta a la BD:

- **boolean execute(String sentenciaSQL)**:
  - Consulta de acción -> return false
  - Consulta de selección -> return true;
- **int executeUpdate(String sentenciaSQL)**: Envía una consulta de acción y devuelve el número de registros afectados por dicha acción.
- **ResultSet executeQuery(String sentenciaSQL)**: Envía una consulta de selección. Devuelve un objeto `ResulSet` con los datos obtenidos.

```java
ResultSet rs = stmt.executeQuery("SELECT * FROM persona");
```

### 5) Manipular resultado de la consulta

#### Desplazarse por ResultSet

**ResultSet**: Objeto de sólo avance y sólo lectura.

El ResulSet dispone de un cursor que se sitúa antes del primer registro.

**Next()**: Permite desplazarnos por los registros. Devuelve `false` cuando ha llegado al final.

```java
while (rs.next()) {
  // ...
}
```

#### Acceder a los campos del resultado de una consulta

Dos grupos de métodos, donde *xxx* es cualquier tipo de dato básico de Java, más *String*, *Date* y *Object*.  
O si no conocemos el tipo de dato: `getObject()`.

- *xxx getXxx(int pos)*: Obtiene el valor del campo que ocupa la posición indicada.
- *xxx getXxx(String name)*: Obtiene el valor del campo que coincide con el nombre indicado.

```java
// Ejemplo
while (rs.next()) {
  System.out.println(rs.getInt("Id") + " " + rs.getString(2) + " " + rs.getDate(3));
}
```

Otros métodos: `boolean isFirst()`, `boolean isLast()` e `int getRow()`.

### 6) Desconexión de BD

Se realiza madiente `close()` en `Connection`

```java
conexion.close();
```

#### ResulSet desplazables y modificables

Estos ResulSet tienen más prestaciones, pero tendremos que crearlos de forma diferente:

```java
Statement createStatement(int tipoResultSet, int cocunrrenciaResultSet);
```

- Tipos de *tipoResultSet*:
  - ResultSet.TYPE_FORWARD_ONLY: De solo avance. Por defecto.
  - ResultSet.TYPE_SCROLL_INSENSITIVE. Ambas direcciones. No refleja cambios de la BD.
  - ResultSet.TYPE_SCROLL_SENSITIVE Ambas direcciones. Refleja cambios.
- Tipos de *concurrenciaREsulSet*:
  - ResultSet.CONCUR_READ_ONLY: Sólo lectura. Por defecto.
  - ResultSet.CONCUR_UPDATETABLE: Campos modificables.

Otros métodos (de desplazar cursor):

- boolean first()
- void beforeFirst()
- boolean last()
- void afterLast()
- boolean absolute(int pos)

Para modificar un campo del ResultSet, utilizamos métodos *updateXxx*.

```java
rs.absolute(2); // Nos situamos en el registro a modificar
rs.updateString(2, "Ana Lozano"); // Modificamos el campo 2. Nuevo valor: "Ana Lozano"
rs.updateRow(); // Se actualiza el registro
```

---
---

#### Ejemplos

##### Ejemplo de ResultSet sencillo

```java
package es.cifpcm;

import java.sql.*;

public class EjemploAccesoBD1 {

  public static void main(String[] args) {
    Connection conexion = null;

    try {
      // Cargar el driver
      Class.forName("com.mysql.jdbc.Driver");

      // Se obtiene una conexión con la base de datos.
      // En este caso nos conectamos a la base de datos prueba
      // con el usuario root y contraseña 1daw
      conexion = DriverManager.getConnection("jdbc:mysql://localhost/prueba", "root",
      "password");

      // Se crea un Statement, para realizar la consulta
      Statement s = conexion.createStatement();

      // Se realiza la consulta. Los resultados se guardan en el ResultSet rs
      ResultSet rs = s.executeQuery("select * from persona");

      // Se recorre el ResultSet, mostrando por pantalla los resultados.
      while (rs.next()) {
        System.out.println(rs.getInt("Id") + " " + rs.getString(2) + " " + rs.getString(3)+ " " + rs.getDate(4));
      }
    } catch (SQLException | ClassNotFoundException e) {
      System.out.println(e.getMessage());
    } finally { // Se cierra la conexión con la base de datos.
      try {
        if (conexion != null) {
          conexion.close();
        }
      } catch (SQLException ex) {
        System.out.println(ex.getMessage());
      }
    }
  }
}
```

---

##### Ejemplo de ResultSet complejo

```java
import java.sql.*;

public class EjemploAccesoBD6 {
  public static void main(String[] args) {
    Connection conexion = null;

    try {
      Class.forName("com.mysql.jdbc.Driver");
      conexion = DriverManager.getConnection("jdbc:mysql://localhost/prueba", "root", "2daw");

      //Indicamos que el ResultSet será desplazable y modificable
      Statement s = conexion.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
      ResultSet.CONCUR_UPDATABLE);
      ResultSet rs = s.executeQuery("select * from persona");

      //Recorrer el ResultSet desde el final hasta el principio
      rs.afterLast();

      while (rs.previous()) {
        System.out.println(rs.getInt(1) + " " + rs.getString(2) + " " + rs.getDate(3));
      }

      // modificar el campo nombre (2) del registro 2 del ResultSet
      // el cambio también se produce en la base de datos
      rs.absolute(2);
      rs.updateString(2, "Ana Lozano");
      rs.updateRow();

      //Recorrer el ResultSet para comprobar la modificación.
      //Para recorrer el ResultSet desde el principio hasta el final nos debemos situar
      //de nuevo al principio
      rs.beforeFirst();

      while (rs.next()) {
        System.out.println(rs.getInt(1) + " " + rs.getString(2) + " " + rs.getDate(3));
      }
    } catch (SQLException e) {
      System.out.println(e.getMessage());

    } catch (ClassNotFoundException e) {
      System.out.println(e.getMessage());
    } finally {
      try {
        if (conexion != null) {
          conexion.close();
        }
      } catch (SQLException ex) {
        System.out.println(ex.getMessage());
      }
    }
  }
}
```