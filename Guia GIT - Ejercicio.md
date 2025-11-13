# Proyecto Java – Sistema de Canciones con Login y CRUD (JSON)
## Descripción

Este proyecto en Java simula una pequeña aplicación musical con:

* Login y registro de usuarios
* Gestión de canciones personales (CRUD completo)
* Datos almacenados en archivos JSON compartidos
* Cada canción pertenece a un usuario mediante una clave (userId)




1) Paso vamos a crear un proyecto nuevo de java con el nombre de ProyectoCanciones.

2) Inicializa Git en la carpeta principal del proyecto creado:
```bash
git init
```

3) Vamos a verificar el estado: 
```bash
git status
```

4) Vamos a agregar los archivos

```bash
git add .
```

5) Subiremos el primer commit 

```bash
git commit -am"Proyecto incial Java"
```

4) Agregamos el origen
```bash
git remote add origin https://github.com/jguidoc04/proyecto-canciones.git
```

5) Subimos los cambios
```bash
git push -u origin main
```


6) Vamos a agregar la dependencia de JSON en el archivos pom.xml

### Dependencias
* Si usas Maven
Agrega a tu pom.xml:

```xml
<dependencies>
    <dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.10.1</version>
    </dependency>
</dependencies>
```


7) Vamos a subir el cambio a git:

```bash
git add .
git commit -am "Agregando dependencia JSON"
git push
```



## Clases:


8) Vamos a crear un package (paquete) llamado modelo y vamos a crear la clase Usuario,Cancion y vamos a pegarles el siguiente codigo en su respectivo archivo:

### Clase Usuario.java

```java
public class Usuario {
    private int id;
    private String nombre;
    private String password;

    public Usuario(int id, String nombre, String password) {
        this.id = id;
        this.nombre = nombre;
        this.password = password;
    }

    public int getId() {
        return id;
    }

    public String getNombre() {
        return nombre;
    }

    public String getPassword() {
        return password;
    }
}
```

### Clase Cancion.java
```java
public class Cancion {
    private int id;
    private int userId;  // Clavee
    private String titulo;
    private String artista;

    public Cancion(int id, int userId, String titulo, String artista) {
        this.id = id;
        this.userId = userId;
        this.titulo = titulo;
        this.artista = artista;
    }

    public int getId() {
        return id;
    }

    public int getUserId() {
        return userId;
    }

    public String getTitulo() {
        return titulo;
    }

    public String getArtista() {
        return artista;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }

    public void setArtista(String artista) {
        this.artista = artista;
    }

    @Override
    public String toString() {
        return "[" + id + "] 🎵 " + titulo + " - " + artista;
    }
}
```


9) Vamos a subir los cambios del paquete model a Git
```bash
git add .
git commit-am "Agregando clases Persona y cancion"
git push
```

10) Vamos a crear el paquete gestion y en ella vamos a crear la clase GestorUsuarios y GestorCanciones.

### Clase GestorUsuarios.java

```java
import com.google.gson.reflect.TypeToken;
import com.google.gson.Gson;
import java.io.*;
import java.util.*;
import modelo.*;

public class GestorUsuarios {
    private static final String FILE_PATH = "usuarios.json";
    private static final Gson gson = new Gson();

}
```

### Clase GestorCanciones.java (CRUD Completo)

```java
import com.google.gson.reflect.TypeToken;
import com.google.gson.Gson;
import java.io.*;
import java.util.*;
import modelo.*;

public class GestorCanciones {
    private static final String FILE_PATH = "canciones.json";
    private static final Gson gson = new Gson();

}
```

11) Vamos a subir los cambios a GIT.

```bash
git add .
git commit-am "Agregando clases Gestor Canciones y Usuarios"
git push
```

12) En clase GestorUsuarios vamos a agregar las siguientes funciones

```java
    public static List<Usuario> cargarUsuarios() {
        try (Reader reader = new FileReader(FILE_PATH)) {
            return gson.fromJson(reader, new TypeToken<List<Usuario>>(){}.getType());
        } catch (IOException e) {
            return new ArrayList<>();
        }
    }

    public static void guardarUsuarios(List<Usuario> usuarios) {
        try (Writer writer = new FileWriter(FILE_PATH)) {
            gson.toJson(usuarios, writer);
        } catch (IOException e) {
            System.out.println("Error guardando usuarios: " + e.getMessage());
        }
    }

```

Estas  2 funciones hacen referencia a como vamos a guardar los datos en el archivo JSON

13) Subimos los cambios a GIT

```bash
git add .
git commit-am "Agregando funciones cargar usuarios y guardar usuarios"
git push
```


14) Vamos a agregar las funciones login y registrar en GestorUsuarios

```java
    public static Usuario login(String nombre, String password) {
        List<Usuario> usuarios = cargarUsuarios();
        for (Usuario u : usuarios) {
            if (u.getNombre().equals(nombre) && u.getPassword().equals(password)) {
                return u;
            }
        }
        return null;
    }

    public static Usuario registrar(String nombre, String password) {
        List<Usuario> usuarios = cargarUsuarios();
        int nuevoId = usuarios.size() + 1;
        Usuario nuevo = new Usuario(nuevoId, nombre, password);
        usuarios.add(nuevo);
        guardarUsuarios(usuarios);
        return nuevo;
    }
```


```bash
git add .
git commit-am "Agregando funciones login y registro"
git push
```

14) Vamos a trabajar en la clase Main, vamos a importar nuestros modelos y gestores. Asimismo de un pequeño menu para saber si el usuario va a inciar sesion o registrarse, y vamos a llamar a los metodos creados en la clase Gestor Usuarios.


```java
import modelo.*;
import gestor.*;
```


Clase Main.java

```java
import java.util.*;
import modelo.*;
import gestor.*;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Usuario usuarioActual = null;

        System.out.println("🎶 Bienvenido al Sistema de Canciones 🎶");
        System.out.println("1. Login");
        System.out.println("2. Registrar");
        System.out.print("Seleccione opción: ");
        int opcion = sc.nextInt(); sc.nextLine();

        if (opcion == 1) {
            System.out.print("Usuario: ");
            String nombre = sc.nextLine();
            System.out.print("Contraseña: ");
            String pass = sc.nextLine();
            usuarioActual = GestorUsuarios.login(nombre, pass);
            if (usuarioActual == null) {
                System.out.println("❌ Usuario o contraseña incorrectos.");
                return;
            }
        } else if (opcion == 2) {
            System.out.print("Nuevo usuario: ");
            String nombre = sc.nextLine();
            System.out.print("Contraseña: ");
            String pass = sc.nextLine();
            usuarioActual = GestorUsuarios.registrar(nombre, pass);
            System.out.println("✅ Usuario registrado con éxito.");
        }

       
    }
}
```

15) Subimos los cambios a GIT
```bash
git add .
git commit-am "Agregando login y registro en clase main"
git push
```

16) Vamos a agregar los metodos cargarCanciones y guardarCanciones en el archivo GestorCanciones


```java
    public static List<Cancion> cargarCanciones() {
        try (Reader reader = new FileReader(FILE_PATH)) {
            return gson.fromJson(reader, new TypeToken<List<Cancion>>(){}.getType());
        } catch (IOException e) {
            return new ArrayList<>();
        }
    }

    public static void guardarCanciones(List<Cancion> canciones) {
        try (Writer writer = new FileWriter(FILE_PATH)) {
            gson.toJson(canciones, writer);
        } catch (IOException e) {
            System.out.println("Error guardando canciones: " + e.getMessage());
        }
    }

```
Cabe mencionar que estos metodos hacen referencia a como vamos a guardar y cargar las canciones desde un archivo JSON como tal.


17) Subimos los cambios a GIT
```bash
git add .
git commit-am "Agregando cargar y guardar canciones en el gestor"
git push
```

18) Vamos a agregar los metodos obtenerCancionesUsuario, agregarCancion, eliminarCancion, editarCancion.

```java


    public static List<Cancion> obtenerCancionesUsuario(int userId) {
        List<Cancion> todas = cargarCanciones();
        List<Cancion> propias = new ArrayList<>();
        for (Cancion c : todas) {
            if (c.getUserId() == userId) {
                propias.add(c);
            }
        }
        return propias;
    }

    public static void agregarCancion(int userId, String titulo, String artista) {
        List<Cancion> canciones = cargarCanciones();
        int nuevoId = canciones.size() + 1;
        Cancion nueva = new Cancion(nuevoId, userId, titulo, artista);
        canciones.add(nueva);
        guardarCanciones(canciones);
    }

    public static boolean eliminarCancion(int userId, int cancionId) {
        List<Cancion> canciones = cargarCanciones();
        boolean eliminada = canciones.removeIf(c -> c.getId() == cancionId && c.getUserId() == userId);
        if (eliminada) guardarCanciones(canciones);
        return eliminada;
    }

    public static boolean editarCancion(int userId, int cancionId, String nuevoTitulo, String nuevoArtista) {
        List<Cancion> canciones = cargarCanciones();
        for (Cancion c : canciones) {
            if (c.getId() == cancionId && c.getUserId() == userId) {
                c.setTitulo(nuevoTitulo);
                c.setArtista(nuevoArtista);
                guardarCanciones(canciones);
                return true;
            }
        }
        return false;
    }

```



17) Subimos los cambios a GIT
```bash
git add .
git commit-am "Agregando agregar,editar, obtener y eliminar canciones"
git push
```


19) Vamos a modificar el main para seguir el flujo, una vez ya habiendo iniciado sesion haya un menu donde podamos gestiornar nuestras canciones.

```java

 if (usuarioActual != null) {
            System.out.println("\n👋 Bienvenido, " + usuarioActual.getNombre());
            while (true) {
                System.out.println("\nMenú Principal:");
                System.out.println("1. Ver mis canciones");
                System.out.println("2. Agregar canción");
                System.out.println("3. Editar canción");
                System.out.println("4. Eliminar canción");
                System.out.println("5. Salir");
                System.out.print("Seleccione opción: ");
                int op = sc.nextInt(); sc.nextLine();

            }
        }

```

20) Abajo del la linea op que hace referencia a opcion vamos a agregar el siguiente if:


```java
                if (op == 1) {
                    List<Cancion> canciones = GestorCanciones.obtenerCancionesUsuario(usuarioActual.getId());
                    if (canciones.isEmpty()) {
                        System.out.println("🎵 No tienes canciones registradas.");
                    } else {
                        canciones.forEach(System.out::println);
                    }
                }

```
 21) Abajo de la anterior condicion bajos a agregar los siguientes else if:

```java
                else if (op == 2) {
                    System.out.print("Título: ");
                    String titulo = sc.nextLine();
                    System.out.print("Artista: ");
                    String artista = sc.nextLine();
                    GestorCanciones.agregarCancion(usuarioActual.getId(), titulo, artista);
                    System.out.println("✅ Canción agregada.");
                } 
                else if (op == 3) {
                    System.out.print("ID de la canción a editar: ");
                    int id = sc.nextInt(); sc.nextLine();
                    System.out.print("Nuevo título: ");
                    String titulo = sc.nextLine();
                    System.out.print("Nuevo artista: ");
                    String artista = sc.nextLine();
                    boolean ok = GestorCanciones.editarCancion(usuarioActual.getId(), id, titulo, artista);
                    System.out.println(ok ? "✅ Canción actualizada." : "❌ No se encontró la canción.");
                } 
                else if (op == 4) {
                    System.out.print("ID de la canción a eliminar: ");
                    int id = sc.nextInt(); sc.nextLine();
                    boolean ok = GestorCanciones.eliminarCancion(usuarioActual.getId(), id);
                    System.out.println(ok ? "🗑️ Canción eliminada." : "❌ No se encontró la canción.");
                } 
                else if (op == 5) {
                    System.out.println("👋 Hasta luego, " + usuarioActual.getNombre() + "!");
                    break;
                }
```

17) Subimos los cambios a GIT
```bash
git add .
git commit-am "Agregando menu de canciones y llamando las funciones"
git push
```


18) Por ultimo va a copiar la url de su repositorio publico de git y la va a subir en un archivo word, y este lo va a subir en la entrega del taller.