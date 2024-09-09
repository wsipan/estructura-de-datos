# Lista Enlazada Simple

- Clase: Nodo
```csharp
internal class Nodo
{
    private int dato;
    private Nodo siguiente;

    public int Dato { get => dato; set => dato = value; }
    internal Nodo Siguiente { get => siguiente; set => siguiente = value; }
}
```

- Clase: Lista

```csharp
internal class Lista
{
    private Nodo primero;
    private Nodo ultimo;

    public Lista()
    {
        primero = null;
        ultimo = null;
    }

    // Comprobar si la lista está vacía
    public bool Lista_Vacia()
    {
        if (primero == null)
        {
            return true;
        }

        return false;
    }

    // Método para Insertar
    public void Insertar(int valor)
    {
        Nodo nuevo = new Nodo();

        // Asignar el valor ingresado
        nuevo.Dato = valor;

        if (primero == null)
        {
            primero = nuevo;
            primero.Siguiente = null;
            ultimo = nuevo;
        }
        else
        {
            ultimo.Siguiente = nuevo;
            nuevo.Siguiente = null;
            ultimo = nuevo;
        }
    }

    // Método para Modificar
    public bool Modificar(int valor, int valor_nuevo)
    {
        if ( ! Lista_Vacia())
        {
            Nodo actual = new Nodo();
            actual = primero;

            while (actual != null)
            {
                if (actual.Dato == valor)
                {
                    actual.Dato = valor_nuevo;
                    return true;
                }

                actual = actual.Siguiente;
            }
        }

        return false;
    }

    // Método para Buscar
    public bool Buscar(int valor)
    {
        if ( ! Lista_Vacia())
        {
            Nodo actual = new Nodo();
            actual = primero;

            while (actual != null)
            {
                if (actual.Dato == valor) return true;

                actual = actual.Siguiente;
            }
        }

        return false;
    }

    // Método para Eliminar
    public bool Eliminar(int valor)
    {
        if ( ! Lista_Vacia() )
        {
            Nodo actual = new Nodo();
            actual = primero;
            Nodo anterior = new Nodo();
            anterior = null;

            while ( actual != null)
            {
                if (actual.Dato == valor)
                {

                    if (actual == primero)
                    {
                        primero = primero.Siguiente;
                    }
                    else if ( actual == ultimo)
                    {
                        anterior.Siguiente = null;
                        ultimo = anterior;
                    }
                    else
                    {
                        anterior.Siguiente = actual.Siguiente;
                    }

                    return true;
                }

                anterior = actual;
                actual = actual.Siguiente;
            }

        }

        return false;
    }

    // Método para Listar
    public void Listar(ListBox lista)
    {
        if ( ! Lista_Vacia() )
        {
            Nodo actual = new Nodo();
            actual = primero;

            // Limpiar la lista
            lista.Items.Clear();

            while (actual != null)
            {
                lista.Items.Add( actual.Dato.ToString() );

                actual = actual.Siguiente;
            }
        }
    }

}
```
