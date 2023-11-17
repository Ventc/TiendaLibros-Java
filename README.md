# TiendaLibros-Java
 
### **Modelo/Libro**

```
package utn.tienda_libros.modelo;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@ToString
public class Libro {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) /*Se movio de lugar en mysql video 6 de semana 7 para orden alfabetico */
    Integer idLibro;
    String nombre;
    String autor;
    Double precio;
    Integer existencias;
}
```

**Package (utn.tienda_libros.modelo):**

**Este código pertenece al paquete utn.tienda_libros.modelo, lo que significa que la clase Libro está organizada en este paquete dentro del proyecto.
Anotaciones de JPA (@Entity, @Id, @GeneratedValue):**

**La anotación @Entity indica que esta clase es una entidad JPA (Java Persistence API) y está asociada a una tabla en la base de datos.
La anotación @Id marca el campo idLibro como la clave primaria de la entidad.
La anotación @GeneratedValue especifica la estrategia de generación de valores para la clave primaria. En este caso, se utiliza GenerationType.IDENTITY, que generalmente se asocia con la autoincrementación en bases de datos como MySQL.
Lombok Annotations (@Data, @NoArgsConstructor, @AllArgsConstructor, @ToString):**

**@Data: Esta anotación de Lombok genera automáticamente métodos como toString, hashCode, equals, getters y setters para todos los campos de la clase.
@NoArgsConstructor: Genera un constructor sin argumentos.
@AllArgsConstructor: Genera un constructor que incluye todos los campos de la clase como parámetros.
@ToString: Genera el método toString para imprimir una representación de cadena de la instancia de la clase.
Campos de la Clase (idLibro, nombre, autor, precio, existencias):**

**Integer idLibro: Es el identificador único del libro y está marcado con la anotación @Id.**
**String nombre: Representa el nombre del libro.**
**String autor: Representa el autor del libro.**
**Double precio: Representa el precio del libro.**
**Integer existencias: Representa la cantidad de existencias disponibles del libro.**
**Comentario sobre @GeneratedValue en MySQL (`/Se movió de lugar en MySQL video 6 de semana 7 para orden alfabético /):**

**Se proporciona un comentario que indica un cambio específico relacionado con la estrategia de generación de claves primarias en MySQL. Se menciona que este cambio se realizó en un video específico (video 6 de semana 7) para ordenar alfabéticamente.**

===========================================================================================

### **Repositorio/LibroRepositorio:**

```
package utn.tienda_libros.repositorio;

import org.springframework.data.jpa.repository.JpaRepository;
import utn.tienda_libros.modelo.Libro;

public interface LibroRepositorio extends JpaRepository<Libro, Integer> {
}
```

`Package (utn.tienda_libros.repositorio):`

**Este código pertenece al paquete utn.tienda_libros.repositorio, lo que significa que la interfaz LibroRepositorio está organizada en este paquete dentro del proyecto.
Importaciones:**

`import org.springframework.data.jpa.repository.JpaRepository;:` Importa la interfaz JpaRepository proporcionada por Spring Data JPA. Esta interfaz simplifica la creación de repositorios que interactúan con la capa de persistencia de la base de datos.**

`import utn.tienda_libros.modelo.Libro;:` Importa la clase Libro del paquete utn.tienda_libros.modelo, ya que esta interfaz trabajará con objetos de esta clase.**

**Declaración de la interfaz (LibroRepositorio):**

`public interface LibroRepositorio extends JpaRepository<Libro, Integer> {:` Declara la interfaz LibroRepositorio que extiende la interfaz JpaRepository. Esta interfaz se parametriza con dos tipos: el tipo de entidad (Libro) y el tipo de su clave primaria (Integer).
Herencia de JpaRepository:**

`extends JpaRepository<Libro, Integer>:` Indica que LibroRepositorio hereda de JpaRepository parametrizado con la clase Libro y el tipo de su clave primaria, que es Integer. Esto significa que la interfaz LibroRepositorio hereda métodos comunes para realizar operaciones de persistencia en la base de datos relacionadas con la entidad Libro.**

===========================================================================================

### **Servicio/ILibroServicio:**

````
package utn.tienda_libros.servicio;

import utn.tienda_libros.modelo.Libro;

import java.util.List;

public interface ILibroServicio {


    public List<Libro> listarLibros();

    public Libro buscarLibroPorId(Integer idLibro);

    public void guardarLibro(Libro libro);

    public void eliminarLibro(Libro libro);
}
````
`Package (utn.tienda_libros.servicio):`

**Este código pertenece al paquete utn.tienda_libros.servicio, lo que significa que la interfaz ILibroServicio está organizada en este paquete dentro del proyecto.
Importaciones:**

`import utn.tienda_libros.modelo.Libro;:` **Importa la clase Libro del paquete utn.tienda_libros.modelo, ya que esta interfaz define operaciones relacionadas con objetos de la clase Libro.
Declaración de la interfaz (ILibroServicio):**

`public interface ILibroServicio {:` **Declara la interfaz ILibroServicio, que define operaciones relacionadas con el servicio de libros.
Métodos de la interfaz:**

`public List<Libro> listarLibros();:` Define un método que devuelve una lista de todos los libros.**

`public Libro buscarLibroPorId(Integer idLibro);:` **Define un método que busca un libro por su identificador (idLibro) y devuelve el objeto Libro correspondiente.**

`public void guardarLibro(Libro libro);:` **Define un método para guardar un libro en el sistema. Toma un objeto Libro como parámetro.**

`public void eliminarLibro(Libro libro);:` **Define un método para eliminar un libro del sistema. Toma un objeto Libro como parámetro.**

===========================================================================================

### **Servicio/LibroServicio:**

````
package utn.tienda_libros.servicio;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import utn.tienda_libros.modelo.Libro;
import utn.tienda_libros.repositorio.LibroRepositorio;

import java.util.List;

@Service
public class LibroServicio implements ILibroServicio {

    @Autowired
    private LibroRepositorio libroRepositorio;

    @Override
    public List<Libro> listarLibros() {
        return libroRepositorio.findAll();
    }

    @Override
    public Libro buscarLibroPorId(Integer idLibro) {
        // Utiliza el método findById proporcionado por JpaRepository
        // Si no se encuentra el libro, devuelve null usando el método orElse(null)
        return libroRepositorio.findById(idLibro).orElse(null);
    }

    @Override
    public void guardarLibro(Libro libro) {
        // Utiliza el método save proporcionado por JpaRepository para guardar el libro
        libroRepositorio.save(libro);
    }

    @Override
    public void eliminarLibro(Libro libro) {
        // Utiliza el método delete proporcionado por JpaRepository para eliminar el libro
        libroRepositorio.delete(libro);
    }
}
````

`Package (utn.tienda_libros.servicio):`

**Este código pertenece al paquete utn.tienda_libros.servicio, que es el mismo paquete que la interfaz ILibroServicio.
Importaciones:**

`import org.springframework.beans.factory.annotation.Autowired;:` **Importa la anotación Autowired de Spring, que se utiliza para realizar la inyección de dependencias automáticamente.**

`import org.springframework.stereotype.Service;:` **Importa la anotación Service de Spring, que se utiliza para marcar la clase como un componente de servicio gestionado por el contenedor de Spring.**

`import utn.tienda_libros.modelo.Libro;:` **Importa la clase Libro del paquete utn.tienda_libros.modelo, ya que esta clase se utiliza en la implementación del servicio.**

`import utn.tienda_libros.repositorio.LibroRepositorio;:` **Importa la interfaz LibroRepositorio del paquete utn.tienda_libros.repositorio, ya que se utiliza para interactuar con la capa de persistencia de la base de datos.**

**Anotación @Service:**

`@Service:` Marca la clase como un componente de servicio de Spring, lo que permite la detección automática y la gestión de esta clase por parte del contenedor de Spring.
Inyección de Dependencias (@Autowired):**

`@Autowired private LibroRepositorio libroRepositorio;:` **Utiliza la anotación @Autowired para inyectar automáticamente una instancia de LibroRepositorio en la variable libroRepositorio. Esta inyección se realiza por el contenedor de Spring.**

**Implementación de Métodos de la Interfaz ILibroServicio:**

`listarLibros():` Utiliza el método findAll proporcionado por JpaRepository para recuperar una lista de todos los libros en la base de datos.**

`buscarLibroPorId(Integer idLibro):` Utiliza el método findById proporcionado por JpaRepository para buscar un libro por su identificador. Si no se encuentra el libro, devuelve null utilizando orElse(null).**

`guardarLibro(Libro libro):` Utiliza el método save proporcionado por JpaRepository para guardar un libro en la base de datos.**

`eliminarLibro(Libro libro):` Utiliza el método delete proporcionado por JpaRepository para eliminar un libro de la base de datos.**

===========================================================================================

### **Vista/LibroForm:**

````
package utn.tienda_libros.vista;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import utn.tienda_libros.modelo.Libro;
import utn.tienda_libros.servicio.LibroServicio;

import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

@Component
public class LibroForm extends JFrame {

    @Autowired
    private LibroServicio libroServicio;
    private JTextField libroTexto, autorTexto, precioTexto, existenciaTexto, idTexto;
    private JButton agregarButton, modificarButton, eliminarButton;
    private JTable tablaLibro;
    private DefaultTableModel tablaModeloLibros;

    @Autowired
    public LibroForm(LibroServicio libroServicio) {
        this.libroServicio = libroServicio;
        initForm();
        setListeners();
    }

    private void initForm() {
        // Configuración de la ventana JFrame
        // ...

        // Configuración de componentes Swing (JTextField, JButton, JTable, etc.)
        // ...
    }

    private void setListeners() {
        // Configuración de listeners (eventos) para botones y tabla
        // ...
    }

    private void agregarLibro() {
        // Lógica para agregar un libro
        // ...
    }

    private void cargarLibroSeleccionado() {
        // Lógica para cargar un libro seleccionado en los campos de texto
        // ...
    }

    private void modificarLibro() {
        // Lógica para modificar un libro
        // ...
    }

    private void eliminarLibro() {
        // Lógica para eliminar un libro
        // ...
    }

    private void limpiarFormulario() {
        // Lógica para limpiar los campos del formulario
        // ...
    }

    private void mostrarMensaje(String mensaje) {
        JOptionPane.showMessageDialog(this, mensaje);
    }

    private void createUIComponents() {
        // Configuración de componentes Swing personalizados (JTable, JTextField, etc.)
        // ...
    }

    private void listarLibros() {
        // Lógica para listar libros en la tabla
        // ...
    }
}
````
**En esta versión resumida, se agrupo la lógica relacionada en métodos más pequeños y se ha proporcionado comentarios que indican la función general de cada bloque de código. Además, se eliminaron detalles específicos para reducir la complejidad visual y resaltar las partes clave del código. En todo caso, se debe completar la configuración específica de la interfaz y ajustar las lógicas según tus necesidades.**

===========================================================================================

### **Vista/TiendaApplicationJava:**

````
// Punto de entrada principal de la aplicación Spring Boot
public class TiendaLibrosApplication {

    public static void main(String[] args) {
        // Paso 1: Inicia la aplicación Spring Boot y obtén el contexto de la aplicación
        ConfigurableApplicationContext contextoSpring = iniciarSpringApplication(args);

        // Paso 2: Ejecuta el código para cargar y mostrar el formulario de libros
        EventQueue.invokeLater(() -> mostrarLibroForm(contextoSpring));
    }

    // Método para iniciar la aplicación Spring Boot y obtener el contexto de la aplicación
    private static ConfigurableApplicationContext iniciarSpringApplication(String[] args) {
        // Configura y ejecuta la aplicación Spring Boot
        return new SpringApplicationBuilder(TiendaLibrosApplication.class)
                .headless(false)  // Habilita la interfaz gráfica
                .web(WebApplicationType.NONE)  // Configura como aplicación no web
                .run(args);  // Inicia la aplicación Spring y obtén el contexto
    }

    // Método para mostrar el formulario de libros
    private static void mostrarLibroForm(ConfigurableApplicationContext contextoSpring) {
        // Obtiene la instancia del formulario de libros desde el contexto de Spring
        LibroFrom libroFrom = contextoSpring.getBean(LibroFrom.class);
			libroFrom.setVisible(true);

		});
	}
}
````

**Configuración de la Aplicación Spring Boot:**

`@SpringBootApplication:` **Esta anotación indica que esta clase es la clase principal de la aplicación Spring Boot. Proporciona configuración automática y permite iniciar la aplicación de manera sencilla.
Método Principal (main):**

`public static void main(String[] args) {:` **Este es el punto de entrada principal de la aplicación.**

`ConfigurableApplicationContext contextoSpring = iniciarSpringApplication(args);:` **Aquí se llama a un método (iniciarSpringApplication) para iniciar la aplicación Spring Boot y obtener el contexto de la aplicación.**

`EventQueue.invokeLater(() -> { mostrarLibroForm(contextoSpring); });:` **Se utiliza EventQueue.invokeLater para ejecutar de manera segura la creación y visualización de la interfaz gráfica en el hilo de eventos de Swing. Luego, se llama a un método (mostrarLibroForm) para obtener y hacer visible el formulario de libros**

`Método iniciarSpringApplication:`

`private static ConfigurableApplicationContext iniciarSpringApplication(String[] args) {:` **Este método configura y ejecuta la aplicación Spring Boot.**

`return new SpringApplicationBuilder(TiendaLibrosApplication.class) .headless(false) .web(WebApplicationType.NONE) .run(args);:` **Aquí se utiliza el SpringApplicationBuilder para configurar la aplicación Spring Boot. Se especifica que la aplicación no es web (WebApplicationType.NONE) y que puede tener una interfaz gráfica (headless(false)).**

`Método mostrarLibroForm:`

`private static void mostrarLibroForm(ConfigurableApplicationContext contextoSpring) {:` **Este método obtiene y muestra el formulario de libros.**

`LibroFrom libroFrom = contextoSpring.getBean(LibroFrom.class);:` **Utiliza el contexto de Spring para obtener una instancia del formulario de libros (LibroFrom).**

`libroFrom.setVisible(true);:` **Hace visible el formulario de libros para que los usuarios puedan interactuar con él.**

===========================================================================================

### **Test/TiendaLibrosApplicationTest:**

````
package utn.tienda_libros;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class TiendaLibrosApplicationTests {

	@Test
	void contextLoads() {
	}

}
````
Anotación @SpringBootTest:

`@SpringBootTest:` **Esta anotación se utiliza en las pruebas de Spring Boot para indicar que se debe cargar y configurar el contexto de la aplicación de la misma manera que lo haría en una aplicación real. Esto permite realizar pruebas de integración con todas las configuraciones y componentes de la aplicación.

**Clase TiendaLibrosApplicationTests:**

`class TiendaLibrosApplicationTests {:` **Esta clase contiene pruebas para la aplicación Spring Boot.
Método contextLoads:**

`@Test:` **Esta anotación indica que el método contextLoads es un método de prueba.**

`void contextLoads() { }:` **Este método es una prueba simple que no realiza ninguna acción específica (void). Su nombre, contextLoads, es una convención común para pruebas que cargan el contexto de la aplicación.**

**En resumen, este archivo de prueba no realiza ninguna operación significativa en su método de prueba contextLoads. Su propósito principal es cargar el contexto de la aplicación Spring Boot y verificar que no haya errores durante la carga del contexto. Este tipo de prueba es utilizada para asegurarse de que la aplicación pueda iniciar correctamente con todas las configuraciones necesarias.**
