# C-// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");

using System;
using System.Collections.Generic;

// Definimos la clase Tarea, que representa una tarea en la lista de tareas.
public class Tarea
{
    // Propiedades para almacenar la descripción de la tarea, la fecha límite (opcional) y el estado de completada.
    public string Descripcion { get; set; }   // Propiedad para la descripción de la tarea.
    public DateTime? FechaLimite { get; set; }   // Propiedad para la fecha límite opcional. Se usa DateTime? para permitir valores nulos.
    public bool Completada { get; private set; }   // Propiedad para el estado de completada. Solo se puede modificar dentro de la clase.

    // Constructor que inicializa la tarea con una descripción y una fecha límite opcional.
    public Tarea(string descripcion, DateTime? fechaLimite = null)
    {
        Descripcion = descripcion;   // Asigna la descripción proporcionada al campo Descripcion.
        FechaLimite = fechaLimite;   // Asigna la fecha límite proporcionada al campo FechaLimite.
        Completada = false;   // Inicializa la tarea como no completada.
    }

    // Método para marcar la tarea como completada.
    public void MarcarComoCompletada()
    {
        Completada = true;   // Cambia el estado de la tarea a completada.
    }
}

// Clase principal que contiene la lógica del programa.
class Program
{
    // Lista genérica para almacenar las tareas. Es una lista de objetos de tipo Tarea.
    static List<Tarea> tareas = new List<Tarea>();

    // Método Main, que es el punto de entrada de la aplicación.
    static void Main(string[] args)
    {
        // Bucle infinito para mostrar el menú y procesar las opciones hasta que el usuario decida salir.
        while (true)
        {
            // Mostrar el menú de opciones al usuario.
            Console.WriteLine("\n1. Agregar tarea\n2. Listar tareas\n3. Marcar tarea como completada\n4. Eliminar tarea\n5. Salir");

            // Leer la opción seleccionada por el usuario y convertirla a un número entero.
            int opcion = Convert.ToInt32(Console.ReadLine());

            // Switch para manejar la opción seleccionada por el usuario.
            switch (opcion)
            {
                case 1: AgregarTarea(); break;   // Llama al método AgregarTarea si el usuario elige la opción 1.
                case 2: ListarTareas(); break;   // Llama al método ListarTareas si el usuario elige la opción 2.
                case 3: MarcarTareaComoCompletada(); break;   // Llama al método MarcarTareaComoCompletada si el usuario elige la opción 3.
                case 4: EliminarTarea(); break;   // Llama al método EliminarTarea si el usuario elige la opción 4.
                case 5: return;   // Sale del bucle y termina el programa si el usuario elige la opción 5.
                default: Console.WriteLine("Opción no válida."); break;   // Muestra un mensaje de error si la opción no es válida.
            }
        }
    }

    // Método para agregar una nueva tarea a la lista.
    static void AgregarTarea()
    {
        // Solicita la descripción de la tarea al usuario.
        Console.Write("Descripción: ");
        string descripcion = Console.ReadLine();   // Lee la descripción ingresada por el usuario.

        // Solicita la fecha límite de la tarea al usuario (opcional).
        Console.Write("Fecha límite (opcional, formato yyyy-MM-dd): ");
        DateTime? fechaLimite = DateTime.TryParse(Console.ReadLine(), out DateTime fecha) ? (DateTime?)fecha : null;   
        // Intenta convertir la entrada a una fecha. Si es válida, se asigna a la variable fechaLimite; si no, se asigna null.

        // Crea una nueva tarea con la descripción y la fecha límite ingresadas por el usuario.
        Tarea nuevaTarea = new Tarea(descripcion, fechaLimite);

        // Agrega la nueva tarea a la lista de tareas.
        tareas.Add(nuevaTarea);

        // Muestra un mensaje indicando que la tarea ha sido agregada.
        Console.WriteLine("Tarea agregada.");
    }

    // Método para listar todas las tareas en la consola.
    static void ListarTareas()
    {
        // Recorre la lista de tareas.
        for (int i = 0; i < tareas.Count; i++)
        {
            var tarea = tareas[i];   // Obtiene la tarea en la posición i.

            // Determina el estado de la tarea (completada o pendiente).
            string estado = tarea.Completada ? "[Completada]" : "[Pendiente]";

            // Determina la fecha límite o muestra un mensaje si no tiene fecha límite.
            string fecha = tarea.FechaLimite.HasValue ? tarea.FechaLimite.Value.ToString("yyyy-MM-dd") : "Sin fecha límite";

            // Muestra la descripción, la fecha límite y el estado de la tarea en la consola.
            Console.WriteLine($"{i + 1}. {tarea.Descripcion} - {fecha} {estado}");
        }
    }

    // Método para marcar una tarea como completada.
    static void MarcarTareaComoCompletada()
    {
        // Solicita el número de tarea al usuario.
        Console.Write("Número de tarea: ");
        int indice = Convert.ToInt32(Console.ReadLine()) - 1;   // Convierte la entrada a un número entero y resta 1 para obtener el índice.

        // Verifica si el índice es válido (dentro del rango de la lista).
        if (indice >= 0 && indice < tareas.Count)
        {
            tareas[indice].MarcarComoCompletada();   // Marca la tarea correspondiente como completada.
            Console.WriteLine("Tarea marcada como completada.");   // Muestra un mensaje indicando que la tarea ha sido marcada como completada.
        }
        else
        {
            // Muestra un mensaje de error si el número de tarea es inválido.
            Console.WriteLine("Número de tarea inválido.");
        }
    }

    // Método para eliminar una tarea de la lista.
    static void EliminarTarea()
    {
        // Solicita el número de tarea al usuario.
        Console.Write("Número de tarea: ");
        int indice = Convert.ToInt32(Console.ReadLine()) - 1;   // Convierte la entrada a un número entero y resta 1 para obtener el índice.

        // Verifica si el índice es válido (dentro del rango de la lista).
        if (indice >= 0 && indice < tareas.Count)
        {
            tareas.RemoveAt(indice);   // Elimina la tarea correspondiente de la lista.
            Console.WriteLine("Tarea eliminada.");   // Muestra un mensaje indicando que la tarea ha sido eliminada.
        }
        else
        {
            // Muestra un mensaje de error si el número de tarea es inválido.
            Console.WriteLine("Número de tarea inválido.");
        }
    }
}
