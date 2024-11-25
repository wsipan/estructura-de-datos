# asdf

# PRIM
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Prim
{
    internal class Program
    {
        class Grafo
        {
            private int V;
            private int[,] Adyacencia;

            public Grafo(int V)
            {
                this.V = V;
                Adyacencia = new int[V, V];
            }

            public void AgregarArista(int i, int f, int dist)
            {
                Adyacencia[i, f] = dist;
                Adyacencia[f, i] = dist;
            }

            private int MinimoVertice(int[] distancias, bool[] visitado)
            {
                int min = int.MaxValue, minIndex = -1;

                for (int i = 0; i < V; i++)
                {
                    if (!visitado[i] && distancias[i] < min)
                    {
                        min = distancias[i];
                        minIndex = i;
                    }
                }

                return minIndex;
            }

            public void Listar_Distancias()
            {
                int[] padres = new int[V]; // Nodo predecesor
                int[] distancias = new int[V]; // Distancias mínimas
                bool[] visitado = new bool[V]; // Si el nodo ya está en el árbol de expansión

                // Inicializar todos los valores
                for (int i = 0; i < V; i++)
                {
                    distancias[i] = int.MaxValue;
                    visitado[i] = false;
                }

                // Empezar desde el nodo 0
                distancias[0] = 0;
                padres[0] = -1; // Nodo inicial no tiene padre

                for (int i = 0; i < V - 1; i++)
                {
                    int u = MinimoVertice(distancias, visitado);
                    visitado[u] = true;

                    for (int v = 0; v < V; v++)
                    {
                        if (Adyacencia[u, v] != 0 && !visitado[v] && Adyacencia[u, v] < distancias[v])
                        {
                            padres[v] = u;
                            distancias[v] = Adyacencia[u, v];
                        }
                    }
                }

                ImprimirResultados(padres, distancias);
            }

            private void ImprimirResultados(int[] padres, int[] distancias)
            {
                int pesoTotal = 0;

                Console.WriteLine("Arista\t\tPeso");
                for (int i = 1; i < V; i++)
                {
                    Console.WriteLine($"{padres[i]} - {i}\t\t{distancias[i]}");
                    pesoTotal += distancias[i]; // Sumar el peso de cada arista
                }

                Console.WriteLine($"\nPeso total del Árbol de Expansión Mínimo: {pesoTotal}");
            }
        }

        static void Main(string[] args)
        {
            Grafo g = new Grafo(11);

            g.AgregarArista(0,1,8); // J
            g.AgregarArista(0, 6, 9); // K
            g.AgregarArista(0,7,10); // C
            g.AgregarArista(0,8,6); // D
            g.AgregarArista(0,9,12); // A
            g.AgregarArista(0,10,3); // B
            g.AgregarArista(1,10,7); // E
            g.AgregarArista(1,2,10); // F
            g.AgregarArista(1,4,2); // I
            g.AgregarArista(2,10,5); // H
            g.AgregarArista(2,3,9); // G
            g.AgregarArista(3, 4, 13);
            g.AgregarArista(3,5, 12);
            g.AgregarArista(4, 6, 6);
            g.AgregarArista(4, 5, 10);
            g.AgregarArista(5,6,8);
            g.AgregarArista(6,7,7);
            g.AgregarArista(7,8,3);
            g.AgregarArista(8,9,10);
            g.AgregarArista(9,10,8);

            g.Listar_Distancias();

            Console.ReadKey();
        }
    }
}

# floyd

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Algoritmo_02
{
    internal class Program
    {
        public const int inf = 9999;

        private static void Visualizar_Grafo(int[,] distancia, int V)
        {
            Console.WriteLine("Distancia más corta entre par de vértices");

            for (int i = 0; i < V; i++)
            {
                for (int j = 0; j < V; j++)
                {
                    Console.Write($"{distancia[i,j]} ");
                }
                Console.WriteLine();
            }

        }

        public static void Floyd_Warshall(int[,] grafo, int V)
        {
            int[,] distancia = new int[V, V];

            for(int i = 0; i < V; i++)
            {
                for (int j = 0;j < V; j++)
                    distancia[i, j] = grafo[i, j];
            }

            for(int k = 0; k < V; k++)
            {
                for(int i = 0; i < V; i++)
                {
                    for(int j = 0; j < V; j++)
                    {
                        if (distancia[i, k] + distancia[k, j] < distancia[i, j])
                        {
                            distancia[i, j] = distancia[i, k] + distancia[k, j];
                        }
                    }
                }
            }

            Visualizar_Grafo(distancia, V);
        }

        static void Main(string[] args)
        {
            Console.WriteLine("Floyd_Warshall");
            int[,] grafo = {
                {0,10,inf,6,inf},
                {inf,0,3,5,inf},
                {inf,inf,0,inf,3},
                {4,inf,2,0,inf},
                {inf,inf,inf,5,0},
            };

            Floyd_Warshall(grafo, 5);
            Console.ReadKey();
        }
    }
}

```

# dijkstra

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Algoritmo_01
{
    internal class Program
    {
        static int V = 9;
        
        int minDistancia(int[] distancia, bool[] arbol)
        {
            int min = 9999, min_index = -1;

            for (int v = 0; v < V; v++)
            {
                if (arbol[v] == false && distancia[v] <= min)
                {
                    min = distancia[v];
                    min_index = v;
                }
            }

            return min_index;
        }

        void Matriz_Distancias(int[] distancias)
        {
            Console.WriteLine("Vértice\tDistancia");
            for (int i = 0; i < V; i++)
            {
                Console.Write($"{i}\t{distancias[i]}\n");
            }
        }

        void Dijkstra(int[,] grafo, int origen)
        {
            int[] dist = new int[V];
            bool[] arbol = new bool[V];

            for(int i = 0; i < V; i++)
            {
                dist[i] = 9999;
                arbol[i] = false;
            }

            dist[origen] = 0;


            for (int count = 0; count < V - 1; count++)
            {
                int u = minDistancia(dist, arbol);
                arbol[u] = true;

                for (int v = 0; v < V; v++)
                {
                    if (! arbol[v] && grafo[u, v] != 0 && dist[u] != 9999 && dist[u] + grafo[u, v] < dist[v])
                    {
                        dist[v] = dist[u] + grafo[u, v];
                    }
                }
            }

            Matriz_Distancias(dist);

        }

        static void Main(string[] args)
        {
            int[,] grafo = new int[,] {
                {0,4,0,0,0,0,0,8,0},    // 0
                {4,0,8,0,0,0,0,11,0},   // 1
                {0,8,0,7,0,4,0,0,2},    // 2
                {0,0,7,0,9,14,0,0,0},   // 3
                {0,0,0,9,0,10,0,0,0},   // 4
                {0,0,4,14,10,0,2,0,0},  // 5
                {0,0,0,0,0,2,0,1,6},    // 6
                {8,11,0,0,0,0,1,0,7},   // 7
                {0,0,2,0,0,0,6,7,0},    // 8
            };

            Program O = new Program();
            O.Dijkstra(grafo, 0);
            Console.ReadKey();
        }
    }
}
```

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
