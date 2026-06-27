# Actividad 
## Integración de Datos e Interoperabilidad

**Nombre:** Cecilia Maria Alejandra Barrios Castillo 
**Carnet:** 202407174 
**Curso:** Introducción a la Programación y Computación 2  
**Fecha:** 26 de junio de 2026

---

# Parte 1. Evaluación Conceptual

## 1. Tabla comparativa

| Formato | Ventajas | Desventajas |
|----------|----------|-------------|
| CSV | Es ligero, fácil de leer y procesar. Ideal para grandes volúmenes de datos. | No soporta estructuras jerárquicas ni tipos de datos complejos. |
| XML | Permite representar información jerárquica y es altamente estructurado. | Es más pesado, consume más espacio y su procesamiento es más lento. |

---

## 2. Diferencia entre Serialización y Deserialización
**1. Serialización:**
Consiste en convertir un objeto de C# en un formato como JSON para almacenarlo o enviarlo a través de una red.
**2. Deserialización:**
Realiza el proceso inverso, convirtiendo un archivo o texto JSON nuevamente en un objeto de C# utilizando la librería **System.Text.Json**.

---

## 3. Antipatrón N+1

El problema de rendimiento **N+1** ocurre cuando una aplicación realiza una consulta inicial y luego ejecuta una consulta adicional por cada registro obtenido, generando una gran cantidad de accesos innecesarios.

Para evitar este problema se utiliza la técnica de **Batching**, que consiste en agrupar múltiples registros y procesarlos en un solo lote, reduciendo la cantidad de consultas y mejorando significativamente el rendimiento.

---

# Parte 2. Implementación Práctica

## Desafío 1. Consumo de Endpoints y Deserialización

```csharp
using System.Net.Http;
using System.Text.Json;

public async Task<Alumno?> ObtenerAlumnoAsync()
{
    using HttpClient cliente = new HttpClient();

    try
    {
        HttpResponseMessage respuesta =
            await cliente.GetAsync("https://api.usac.edu/v1/alumnos");

        respuesta.EnsureSuccessStatusCode();

        string json = await respuesta.Content.ReadAsStringAsync();

        var opciones = new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        };

        Alumno? alumno =
            JsonSerializer.Deserialize<Alumno>(json, opciones);

        return alumno;
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        return null;
    }
}
```

---

## Desafío 2. Endpoint para carga masiva CSV

```csharp
[HttpPost]
public async Task<IActionResult> CargarArchivo(IFormFile archivo)
{
    if (archivo == null || archivo.Length == 0)
    {
        return BadRequest("Debe seleccionar un archivo.");
    }

    List<Alumno> alumnos = new List<Alumno>();

    using (var reader = new StreamReader(archivo.OpenReadStream()))
    {
        string? linea;

        await reader.ReadLineAsync();

        while ((linea = await reader.ReadLineAsync()) != null)
        {
            string[] datos = linea.Split(',');

            Alumno alumno = new Alumno
            {
                Carnet = datos[0],
                Nombre = datos[1],
                Carrera = datos[2]
            };

            alumnos.Add(alumno);
        }
    }

    _context.Alumnos.AddRange(alumnos);

    await _context.SaveChangesAsync();

    return Ok("Carga masiva completada correctamente.");
}
```

---

# Parte 3. Referencia Bibliográfica

Facultad de Ingeniería, Universidad de San Carlos de Guatemala. (2026). *Sesión 20: Integración de Datos. Consumo de APIs Externas y Carga Masiva (CSV/XML).* Laboratorio del curso Introducción a la Programación y Computación 2. Guatemala.