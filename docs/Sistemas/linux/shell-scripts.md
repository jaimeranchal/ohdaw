# Cómo crear un script en shell

Un ejemplo sencillo que imprime `Hola mundo!` en la consola:

1. Abrimos un editor de texto:
    ```shell
    vim hola.sh
    ```
2. Escribimos lo siguiente:
    ```bash
    #!/bin/bash
    # Imprime el mensaje "Hola mundo!"
    echo "Hola Mundo"!
    ```

Veamos el contenido línea a línea:

- `#!/bin/bash`: es la **cabecera shebang** e indica qué programa se usará para interpretar el script. En este caso contiene la ruta al ejecutable de bash (`/bin/bash`). Otros ejemplos serían:
    - _Python_: `#!/usr/bin/python3`
    - _Perl_: `#!/usr/bin/perl`
- **Comentarios**: empiezan con un signo de almohadilla (`#`).
- **Comando(s)**: en este caso, `echo`; son las acciones propiamente dichas, lo que queremos ejecutar.

A continuación hay que hacer el archivo **ejecutable**:

```shell
chmod +x hola.sh
```

Y finalmente, ejecutarlo:

```shell
bash hola.sh
# o alternativamente
./hola.sh
```

## Sentencias condicionales

### Condición simple: `if`

=== "Sintaxis"
    ```
    if condicion
    then
        sentencia
    fi
    ```
=== "Ejemplo"
    ```bash
    #!/bin/bash
    echo 'Introduce una nota'
    read x

    if [[ $x == 7 ]]; then
        echo 'Buen trabajo!'
    fi
    ```

!!! note "Operadores de comparación"
    - `-eq`: igual a
    - `-ne`: diferente de
    - `-lt`: menor que
    - `-gt`: mayor que
    - `-le`: menor o igual que
    - `-ge`: mayor o igual que

    ```bash
    if [[ $x -lt 50 ]]; then
        echo 'Trabaja más duro!'
    fi
    ```

### Dos condiciones: `if-else`

=== "Sintaxis"
    ```
    if condicion
    then
        sentencia1
    else
        sentencia2
    fi
    ```
=== "Ejemplo"
    ```bash
    #!/bin/bash
    echo 'Introduce una nota'
    read x

    if [[ $x -ge 7 ]]; then
        echo 'Buen trabajo, has aprobado!'
    else
        echo 'Has suspendido'
    fi
    ```

### Múltiples condiciones: `if-elif-else`

=== "Sintaxis"
    ```
    if condicion
    then
        sentencia1
    elif condicion2
    then
        sentencia2
    else
        sentencia3
    fi
    ```
=== "Ejemplo"
    ```bash
    #!/bin/bash
    echo 'Introduce una nota'
    read x

    if [[ $x -eq 9 ]];
    then
        echo "Has ganado el primer premio"
    elif [[ $x -eq 6 ]];
    then
        echo "Has ganado el segundo puesto"
    elif [[ $x -eq 3 ]];
    then
        echo "Has ganado el tercer puesto"
    else
        echo "Prueba otra vez"
    fi
    ```

## Operadores lógicos (AND, OR)

=== "AND"
    ```bash
    #!/bin/bash

    echo 'Please Enter your user_id'
    read user_id

    echo 'Please Enter your tag_no'
    read tag_id

    if [[ ($user_id == “tecmint” && $tag_id -eq 3990) ]];
    then
      echo “Login successful”
    else
      echo “Login failure”
    fi
    ```
=== "OR"
    ```bash
    #!/bin/bash

    echo 'Please enter a random number'
    read number

    if [[ (number -eq 55 || number -eq 80) ]];
    then
     echo 'Congratulations! You’ve won'
    else
     echo 'Sorry, try again'
    fi
    ```

## Bucles

### While

=== "Sintaxis"
    ```
    while <prueba>
    do
        comandos
    done
    ```
=== "Ejemplo"
    ```bash
    #!/bin/bash
    # Imprime todos los números entre 1 y 10
    contador=1
    while [ $contador -le 10 ]
     do
    echo $contador
     ((contador++))
    done
    ```

### For

=== "Sintaxis"
    ```
    for var in 1 2 3 4 5 n
    do
        comando1
        comando2
    done
    ```
=== "Ejemplo"
    ```bash
    #!/bin/bash
    # Imprime todos los números entre 1 y 10

    for num in {1..10}
    do
      echo $num
    done
    ```

## Parámetros posicionales

Un parámetro posicional es un tipo especial de **variable**, identificada por un número, que toma el valor de los argumentos que se indican en la línea de órdenes al ejecutarlo. Tras el nombre de un script se pueden añadir valores, cadenas de texto o números separados por espacios, es decir, parámetros posicionales del programa de shell, a los que se puede acceder utilizando estas variables.

=== "Script"
    ```bash
    #!/bin/bash
    # miNombre.sh
    echo "El nombre del script es: " $0
    echo "Mi nombre es: " $1
    echo "Mi apellido es: " $2
    ```
=== "Ejecución"
    ```shell
    ./miNombre.sh Jaime Gómez
    # Imprime:
    El nombre del script es: miNombre
    Mi nombre es: Jaime
    Mi apellido es: Gómez
    ```

!!! info "Valores por defecto y límites"
    La variable `$0` siempre toma el nombre del script, y a partir de ahí, las variables toman el valor de los argumentos.

    Más allá de la variable `$9`, el número debe escribirse entre llaves: `${10}, ${11}, ${12}...`.

## Códigos de salida

Cualquier comando que se ejecute en la consola tiene un código de salida que indica si ha funcionado correctamente o ha dado **errores**.

- `0`: ejecutado sin fallos
- `1...255`: fallo
    - `1`: error _general_ (falta de permisos p.e.)
    - `2`: uso incorrecto del comando / variable de shell
    - `127`: comando ilegal (= comando no encontrado, muchas veces).

## Procesar la salida de un comando dentro de un script

Se puede almacenar la salida de un comando en una variable para usarla después:

```bash
variable = $(comando)
# o también
variable = $(/ruta/al/comando)
# o también
variable = $(comando argumento1 argumento2...)
```

=== "Ejemplo 1: fecha"
    ```bash
    #!/bin/bash
    hoy = $(date)
    echo "Hoy es $hoy"
    ```
=== "Ejemplo 2: usuarios con login"
    ```bash
    #!/bin/bash
    usuarios_login = $(cat /etc/passwd | grep /bin/bash | cut -c 1-10)
    echo "Esta es la lista de usuarios con login: "
    echo $usuarios_login
    ```
    

