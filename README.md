## ¿Qué es JDBC?

JDBC (Java Database Connectivity) es una API que permite a las aplicaciones en Java (y lenguajes basados en la JVM como Scala) interactuar con bases de datos relacionales. JDBC proporciona una interfaz estándar para conectar, ejecutar consultas y gestionar resultados en una base de datos.

### Componentes de JDBC

1. **Driver Manager**: Gestiona los controladores (drivers) necesarios para conectarse a diferentes bases de datos.
2. **Driver**: Específico para cada base de datos, traduce las llamadas JDBC en comandos entendibles por la base de datos.
3. **Connection**: Representa la conexión activa entre la aplicación y la base de datos.
4. **Statement**: Permite ejecutar consultas SQL y actualizaciones.
5. **ResultSet**: Contiene los resultados devueltos por una consulta SQL.

## Librerías Scala para Conexión a Bases de Datos

| **Librería**  | **Características**                                                                                     | **Diferencias Principales**                                                                                                      |
|---------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| **Slick**     | Framework ORM (Object Relational Mapping) que permite trabajar con bases de datos de manera funcional.   | Orientada a trabajar con modelos de datos. Mayor abstracción y menos control directo sobre el SQL.                             |
| **Doobie**    | Librería funcional para trabajar con JDBC. Ofrece una integración sencilla con Cats Effect y ZIO.       | Enfoque funcional puro. Ofrece control más directo sobre las consultas SQL. No es un ORM.                                     |

## Cómo establecer una conexión a MySQL desde Scala
### Paso 1: Crear una base de datos en MySQL

1. Acceda a su servidor MySQL.
2. Ejecute el siguiente comando para crear una base de datos:

    ```sql
    CREATE DATABASE scala_test;
    ```

### Paso 2: Crear una tabla con datos de prueba y rellenar con datos
1. Use la base de datos creada:

    ```sql
    USE scala_test;
    ```

2. Cree una tabla de prueba:

    ```sql
    CREATE TABLE users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(50),
        email VARCHAR(50)
    );
    ```

3. Inserte datos de prueba:

    ```sql
    INSERT INTO users (name, email) VALUES
    ('Julio', 'juliolol@example.com'),
    ('Santi', 'santilol@example.com');
    ```

### Paso 3: Conexión a la base de datos desde Scala

1. **Configuración**: Asegúrese de que el driver MySQL está disponible en el proyecto (usando SBT):
    En el archivo `build.sbt`:
    ```scala
    libraryDependencies ++= Seq(
      "mysql" % "mysql-connector-java" % "8.0.32"
    )
    ```
2. **Código Scala**:

    ```scala
    package Bimestre2.sem14
    import java.sql.{Connection, DriverManager, ResultSet}

    object mySQL {
      def main(args: Array[String]): Unit = {
    val url = "jdbc:mysql://localhost:3306/scala_test"
    val username = "root"
    val password = "2565J"

    var connection: Connection = null
    try {
      // Establecer la conexión
      connection = DriverManager.getConnection(url, username, password)
      println("Conectada a la base de datos")
      // Consulta de datos
      val statement = connection.createStatement()
      val resultSet: ResultSet = statement.executeQuery("SELECT * FROM users") //iguaal que las tablas del msql
      // Imprimir resultados
      while (resultSet.next()) {
        println(s"ID: ${resultSet.getInt("id")}, Name: ${resultSet.getString("name")}, Email: ${resultSet.getString("email")}")
      }
    }
    }
  
    ```

### Capturas de Pantalla

1. Base de datos y tabla creadas en MySQL.
2. ![image](https://github.com/user-attachments/assets/65a9b894-80d4-48d3-93f0-98a1b065c885)

3. Conexión y ejecución del programa en Scala.
   ![image](https://github.com/user-attachments/assets/336231ac-fc5b-4e49-b277-c3aade467fd6)

## Referencias

- [Documentación de JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/)
- [MySQL Connector/J](https://dev.mysql.com/doc/connector-j/en/)
