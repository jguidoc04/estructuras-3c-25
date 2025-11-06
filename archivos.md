# GUÍA EDUCATIVA: Guardar y Leer un ArrayList de Objetos en Java (.txt y .json)
## Objetivo
Aprender a guardar, leer y cargar información desde archivos usando Java, tanto en formato de texto (.txt) como JSON (.json).
Esto es muy útil para proyectos estudiantiles donde se necesita almacenar datos de estudiantes, notas, productos, etc.

 1. Clase base Estudiante

```java
public class Estudiante {
    private String nombre;
    private double nota;

    // Constructor
    public Estudiante(String nombre, double nota) {
        this.nombre = nombre;
        this.nota = nota;
    }

    // Getters
    public String getNombre() {
        return nombre;
    }

    public double getNota() {
        return nota;
    }

    // Para mostrar los datos fácilmente
    @Override
    public String toString() {
        return nombre + " - Nota: " + nota;
    }
}

```

2. Guardar un ArrayList de estudiantes en un archivo de texto (.txt)

```java
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;

public class GuardarEstudiantesTXT {
    public static void main(String[] args) {
        ArrayList<Estudiante> lista = new ArrayList<>();
        lista.add(new Estudiante("Ana", 95.5));
        lista.add(new Estudiante("Luis", 87.0));
        lista.add(new Estudiante("María", 91.3));

        try (FileWriter writer = new FileWriter("estudiantes.txt")) {
            for (Estudiante e : lista) {
                // Guardamos el nombre y la nota separados por coma
                writer.write(e.getNombre() + "," + e.getNota() + "\n");
            }
            System.out.println("✅ Estudiantes guardados en estudiantes.txt");
        } catch (IOException e) {
            System.out.println("❌ Error al guardar: " + e.getMessage());
        }
    }
}
```

Archivo generado (estudiantes.txt):

```bash
Ana,95.5
Luis,87.0
María,91.3
```
📖 3. Leer la lista desde el archivo de texto

```java
import java.io.*;
import java.util.ArrayList;

public class LeerEstudiantesTXT {
    public static void main(String[] args) {
        ArrayList<Estudiante> lista = new ArrayList<>();

        try (BufferedReader reader = new BufferedReader(new FileReader("estudiantes.txt"))) {
            String linea;
            while ((linea = reader.readLine()) != null) {
                String[] partes = linea.split(",");
                String nombre = partes[0];
                double nota = Double.parseDouble(partes[1]);
                lista.add(new Estudiante(nombre, nota));
            }
            System.out.println("✅ Estudiantes cargados desde archivo:");
        } catch (IOException e) {
            System.out.println("❌ Error al leer: " + e.getMessage());
        }

        // Mostrar los datos
        for (Estudiante e : lista) {
            System.out.println(e);
        }
    }
}
```

📤 Salida esperada:

 Estudiantes cargados desde archivo:

```
Ana - Nota: 95.5
Luis - Nota: 87.0
María - Nota: 91.3
```

💾 4. Guardar el ArrayList en formato JSON (.json)

Para usar JSON, instalamos la librería Gson.

 Dependencia Maven:

```xml
<dependency>
  <groupId>com.google.code.gson</groupId>
  <artifactId>gson</artifactId>
  <version>2.10.1</version>
</dependency>
```


Guardar en JSON

```java
import com.google.gson.Gson;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;

public class GuardarEstudiantesJSON {
    public static void main(String[] args) {
        ArrayList<Estudiante> lista = new ArrayList<>();
        lista.add(new Estudiante("Ana", 95.5));
        lista.add(new Estudiante("Luis", 87.0));
        lista.add(new Estudiante("María", 91.3));

        Gson gson = new Gson();

        try (FileWriter writer = new FileWriter("estudiantes.json")) {
            gson.toJson(lista, writer);
            System.out.println("✅ Estudiantes guardados en estudiantes.json");
        } catch (IOException e) {
            System.out.println("❌ Error al guardar: " + e.getMessage());
        }
    }
}
```


 Archivo generado (estudiantes.json):
```
[
  {"nombre":"Ana","nota":95.5},
  {"nombre":"Luis","nota":87.0},
  {"nombre":"María","nota":91.3}
]
```

 Leer desde JSON

```
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import java.io.FileReader;
import java.io.IOException;
import java.lang.reflect.Type;
import java.util.ArrayList;

public class LeerEstudiantesJSON {
    public static void main(String[] args) {
        Gson gson = new Gson();
        ArrayList<Estudiante> lista = new ArrayList<>();

        try (FileReader reader = new FileReader("estudiantes.json")) {
            Type tipoLista = new TypeToken<ArrayList<Estudiante>>(){}.getType();
            lista = gson.fromJson(reader, tipoLista);
            System.out.println("✅ Estudiantes cargados desde JSON:");
        } catch (IOException e) {
            System.out.println("❌ Error al leer: " + e.getMessage());
        }

        for (Estudiante e : lista) {
            System.out.println(e);
        }
    }
}
```

 Salida esperada:
Estudiantes cargados desde JSON:

```
Ana - Nota: 95.5
Luis - Nota: 87.0
María - Nota: 91.3
```