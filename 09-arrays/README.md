# Arreglos

Una coleccion de elementos de un mismo tipo es un arreglo. Como Golang es un lenguaje fuertemente tipado no es posible mezclar valores de diferentes tipos en un arreglo.

Un arreglo se define como `[size]type`: 

```go
var a [3]int
fmt.Println(a)

> [0 0 0]
```

En el ejemplo anterior se define un arreglo de enteros de 3 elementos. La salida es ceros por que es el valor por defecto del tipo de datos `int`.

El primer indice de un arreglo es `0` en vez de `1`. Se accede a los valores de un arreglo usando el numero del indice, y se asignan valores con el operador `=`.

```go
var a [3]int
a[0] = 1
a[1] = 2
a[2] = 3

fmt.Println(a)

> [1 2 3]
```

Un arreglo se puede inicializar en la declaracion:

```go
a := [3]int{1, 2, 3}
```

Se puede omitir el numero de elementos si se inicializa con valores en la declaracion:

```go
a := [...]int{1, 2, 3}
```

## Value Type

Un detalle importante a tener en cuenta es que un arreglo es un value type, esto quiere decir que cada vez que un arreglo es asignado a una nueva variable, se hace una copia del arreglo original. Si se cambian valores en la nueva variable, esto no se ve reflejado en la variable original.

```go
a := [...]string{"IRL", "IND", "US", "CAN"}
b := a

b[1] = "CHN"

fmt.Println("Original:", a)
fmt.Println("Copy    :", b)
```

Eso significa que cuando un arreglo se pasa como parametro de entrada a una funcion es pasado por valor. Por lo tanto cualquier cambio que se haga dentro de la funcion no afectara al arreglo original.

La funcion `len` se usa para conocer el tamaño de un arreglo:

```go
a := [...]int{1, 2, 3}
fmt.Println(len(a))

> 3
```

Una forma de interactuar con un arreglo es utilizar el keyword `range`. Este retorna el indice del arreglo y el valor. 

El siguiente ejemplo imprimen el indice y el valor de cada elemento del arreglo, y suma todos los valores.

```go
a := [...]int{1, 2, 3, 4, 5}
sum := 0
for i, v := range a {
    fmt.Println("Index:", i, " Value is:", v)
    sum += v
}
fmt.Println("Sum:", sum)

> 15
```

Se puede usar el `blank identifier` (`_`) si no nos interesa alguno de los valores que retorna el keyword `range`.

```go
_, v := range a
```

## Arreglos multidimensionales

Se pueden crear arreglos de mas de una dimension de la siguiente manera:

```go
a := [3][2]int
```

```go
a := [3][2]string{
    {"lion", "tiger"},
    {"cat", "dog"},
    {"pigeon", "peacock"},
}

fmt.Println(a)

> [[lion tiger] [cat dog] [pigeon peacock]]
```

Se puede imprimir el arreglo de diferente manera utilizando el iterador `for`:

```go
func printArray(a) {
    for _, v1 := range a {
        for _, v2 := range v1 {
            fmt.Printf("%s ", v2)
        }
        fmt.Println("")
    }
}
```

En el ejemplo anterior el primer `range` itera por las filas, y el segundo `range` itera por las columnas. Cada valor en una columna se imprime seguido de un espacio. Al final de una fila se imprime una nueva linea.

## Slices

Los arreglos tienen tamaño fijo, pre-alocan memoria, y son value types. Un slice es una forma flexible de acceder a una arreglo. Un slice no tienen datos, solo apunta a un arreglo.

Un slice vacio se crea asi:

```go
var sa []int
```

El valor de un slice vacio es `nil`

Un slice tambien se puede crear apuntando a un subset de valores de un arreglo:

```go
a := [...]int{1, 2, 3, 4, 5}
sa := a[1: 4]
fmt.Println(sa)

> [2 3 4]
```

En el ejemplo anterior, el slice `sa` apunta a los elementos del arreglo `a` desde los indices `1` a `3`. Cuando se especifica el rango de indices de un arreglo, el ultimo indice no se considera.

Como un slice es un reference type, modificar un valor en un elemento del slice modifica el arreglo original:

```go
a := [...]int{1, 2, 3, 4, 5}
sa := a[1: 4]

fmt.Println("Before:", a)
sa[0] = 22

fmt.Println("After:", a)

> Before: [1 2 3 4 5]
> After: [1 22 3 4 5]
```

Un slice tambien se puede crear utilizando la funcion `make`, especificando el tipo, y el tamaño, y opcionalmente la capacidad (que indica el maximo tamaño que el slice puede crecer):

```go
make([]type, length[, capacity])
```

Crear un slice con `make` inicializa todos sus valores con los valores por defecto del tipo del slice.

```go
i := make([]int, 5, 5)
fmt.Println(i)

> [0 0 0 0 0]
```

El tamaño de un slice se puede incrementar utilizando la funcion `append`. Esto crea un nuevo arreglo subyacente con el nuevo tamaño, y copia de todos los valores existentes en el arreglo original, mas los nuevos valores al final:

```go
append(destination, value1, value2, ...)
```

En vez de valores se puede indicar otro slice. El operador `...` se usa para expandir el slice en sus valores.

```go
sa := []int{1, 2, 3}
newSa := append([]int{}, sa...)

fmt.Println(newSa)

> [1, 2, 3]
```

En el ejemplo anterior la funcion `append` se usa para agregar 3 valores desde el slice `sa` a un slice vacio.

Un slice se pasa a una funcion como un arreglo. Modificar los valores de un slice dentro de una funcion, tambien modifica los valores del slice en el codigo llamador de dicha funcion.