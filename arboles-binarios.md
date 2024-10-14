# Árboles Binarios

* InOrden: I - R - D
* PreOrden: R - I - D
* PostOrden: I - R - D

```csharp

public void Insertar(int edad)
{
    Nodo nuevo = new Nodo();
    nuevo.dato = edad;
    nuevo.izquierdo = null;
    nuevo.derecho = null;

    if (raiz == null)
    {
        raiz = nuevo;
    }
    else
    {
        Nodo anterior = null, valor = raiz;

        while (valor != null)
        {
            anterior = valor;

            if (edad < valor.dato)
                valor = valor.izquierdo;
            else
                valor = valor.derecho;
        }

        if (edad < anterior.dato)
            anterior.izquierdo = nuevo;
        else
            anterior.derecho = nuevo;
    }
}

private void ImprimirPre(Nodo valor)
{
    if (valor != null)
    {
        Console.Write(valor.dato + " ");
        ImprimirPre(valor.izquierdo);
        ImprimirPre(valor.derecho);
    }
}

public void ImprimirPre()
{
    ImprimirPre(raiz);
    Console.WriteLine();
}

static void ListarPreOrden()
{
    abb.ImprimirPre();
}

```
