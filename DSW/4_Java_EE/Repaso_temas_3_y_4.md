# Conceptos

## JDK o SDK

Ambiente en el que se desarrolla cualquier aplicación de Java.  
Compuesto por:

- API de Java
- Compilador de Java
- JVM.

## JDBC

Java Dabase Connectivity es una interface de programación que permite acceder desde Java a practicamente cualquier fuente de datos tabulados.

## JSP

Java Server Pages, es un tipo de programa de Java que mezcla HTML con código Java. Se necesitará de un Servelet para que funcione

## Servlet

Es una clase Java que se ejecuta en un servidor de aplicaciones. Se usa para procesar peticiones de usuario, crear cotenidos dinámicos, verificar usuarios, etc.

Un JSP se convierte eventualmente en un Servlet, aunque el JSP lleva HTML incluido.

## Singleton

Un *singleton* o instancia única es un patrón de diseño que restringe la creación de objetos de una clase a una sola instancia.

Se implementa creando una instancia del objeto solo si todavía no existe alguna.

```java
public class dbConexion {
   private static Connection con=null;
   public static Connection getConnection(){
      try{
         if( con == null ){
            String driver="com.mysql.jdbc.Driver"; //el driver varia segun la DB que usemos
            String url="jdbc:mysql://localhost/nombre_DB?autoReconnect=true";
            String pwd="contraseña";
            String usr="usuario";
            Class.forName(driver);
            con = DriverManager.getConnection(url,usr,pwd);
            System.out.println("Conection Succesfull");
         }
     }
     catch(ClassNotFoundException | SQLException ex){
        ex.printStackTrace();
     }
     return con;
   }
}

// Luego para obtener la instancia
dbConexion.getConnection();

```