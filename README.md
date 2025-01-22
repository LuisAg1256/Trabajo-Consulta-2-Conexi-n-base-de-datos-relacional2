# Trabajo Consulta 2 Conexion base de datos relacional
# JDBC y Conexión desde Scala a una Base de Datos Relacional

## ¿Qué es JDBC y cuáles son sus componentes?

JDBC (Java Database Connectivity) es una API de Java que permite a las aplicaciones interactuar con bases de datos relacionales. Proporciona un conjunto de interfaces y clases que facilitan la conexión, ejecución de consultas SQL y manejo de resultados.

### Componentes principales de JDBC

1. **Driver Manager**: Gestiona la carga y el acceso a los controladores JDBC.
2. **Driver**: Una implementación específica para comunicarse con una base de datos concreta.
3. **Connection**: Representa la conexión activa con la base de datos.
4. **Statement**: Permite ejecutar consultas SQL (Statement, PreparedStatement, CallableStatement).
5. **ResultSet**: Maneja los resultados de las consultas SQL.
6. **SQLException**: Maneja errores relacionados con la base de datos.

---

## Librerías de Scala para conectarse a bases de datos relacionales

| **Librería**         | **Descripción**                                                                 | **Ventajas**                                              | **Desventajas**                                    |
|----------------------|-------------------------------------------------------------------------------|----------------------------------------------------------|--------------------------------------------------|
| **Slick**           | Framework funcional para trabajar con bases de datos relacionales en Scala.   | Integración con el paradigma funcional.                   | Curva de aprendizaje moderada.                   |
| **Doobie**          | Librería ligera para manejar JDBC con un enfoque funcional.                  | Alto control sobre las consultas SQL.                     | Requiere manejo explícito de recursos.           |

---

## Configuración para establecer una conexión a MySQL desde Scala

### 1. Crear una base de datos en MySQL

```sql
CREATE DATABASE ejemplo_db;
```

### 2. Crear una tabla con datos de prueba

```sql
USE ejemplo_db;

CREATE TABLE usuarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    edad INT NOT NULL
);

INSERT INTO usuarios (nombre, edad) VALUES
('Juan', 25),
('María', 30),
('Carlos', 28);
```

### 3. Conexión desde Scala

#### Configuración del proyecto en SBT

En el archivo `build.sbt`, se agrego las dependencias necesarias:

```scala
libraryDependencies ++= Seq(
  "mysql" % "mysql-connector-java" % "8.0.33",
  "org.tpolecat" %% "doobie-core" % "1.0.0-RC2"
)
```

#### Código para conectarse a la base de datos

```scala
import java.sql.{Connection, DriverManager}

object ConexionMySQL {
  def main(args: Array[String]): Unit = {
    val url = "jdbc:mysql://localhost:3306/ejemplo_db"
    val user = "root"
    val password = "kame"

    try {
      // Establecer conexión
      val conexion: Connection = DriverManager.getConnection(url, user, password)
      println("Conexión exitosa a la base de datos")

      // Consulta de prueba
      val statement = conexion.createStatement()
      val resultSet = statement.executeQuery("SELECT * FROM usuarios")

      println("Datos de la tabla:")
      while (resultSet.next()) {
        println(s"ID: ${resultSet.getInt("id")}, Nombre: ${resultSet.getString("nombre")}, Edad: ${resultSet.getInt("edad")}")
      }

      conexion.close()
    } catch {
      case e: Exception => e.printStackTrace()
    }
  }
}
```

#### Capturas de pantalla
Capturas de:

1. La base de datos y tabla creadas en MySQL.
   ![image](https://github.com/user-attachments/assets/58a33d4f-1110-4ab7-9e0f-5a1ca69d24ec)

3. La ejecución del programa en Scala mostrando la conexión exitosa y los datos de la tabla.
![image](https://github.com/user-attachments/assets/fff68f12-a95b-4ea8-b155-f707bc493f6b)

---

## Referencias

- [Documentación oficial de JDBC](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/)
- [Slick](https://scala-slick.org/)
- [Doobie](https://tpolecat.github.io/doobie/)
- [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)
