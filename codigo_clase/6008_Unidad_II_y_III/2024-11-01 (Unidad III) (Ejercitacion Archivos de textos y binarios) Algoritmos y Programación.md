---
Profesor: Lucas Matias Frias
Meet: https://meet.google.com/iwh-byuw-kzu
Cuadernillo:
---

## [[00 - 6008 - Algoritmos y Programación|Ir al Índice]]

# Resumen

- Lo visto en esta clase es crear, leer, y modificar archivos de texto y binarios

Para ello dentro de `pickle` importamos las libresias `dumps` y `load` 

`dumps` nos va a servir para pushear las que queremos hacer en archivos binarios
`loads` Nos va a servir para cargar archivos binarios 
# Ejercicio hecho en clase

```python
from pickle import dumps, load

class Curso:
    def __init__(self, cod, nombre, desc, prof, nivel, horas, categoria):
        self.codigo = int(cod)
        self.nombre = nombre
        self.descripcion = desc
        self.profesor = prof
        self.nivel = nivel
        self.horas_duracion = float(horas)
        self.categoria = categoria

    def __repr__(self):
        return (f"══════════════════════════════════════════\n"
                f"         Curso: {self.nombre} ({self.codigo})\n"
                f"══════════════════════════════════════════\n"
                f"  Descripción:  {self.descripcion}\n"
                f"  Profesor:     {self.profesor}\n"
                f"  Nivel:        {self.nivel}\n"
                f"  Duración:     {self.horas_duracion} horas\n"
                f"  Categoría:    {self.categoria}\n"
                f"══════════════════════════════════════════\n")

    def obtener_resumen(self) -> str:
        return f"Código: {self.codigo} - Curso: {self.nombre}"

class GestorDeCursos:
    def __init__(self):
        self.limite_de_cursos = 10
        self.cursos: list[Curso] = []

    def __str__(self):
        if len(self.cursos) > 0:
            return str(self.cursos)
        else:
            return "No hay cursos cargados actualmente"

    def agregar_curso(self, nuevo_curso: Curso):
        if len(self.cursos) < self.limite_de_cursos:
            self.cursos.append(nuevo_curso)
        else:
            print("La lista de cursos ya se encuentra llena")

    def eliminar_curso(self, curso_a_eliminar: Curso):
        self.cursos.remove(curso_a_eliminar)

    def mostrar_todos(self):
        if self.cursos:
            print("Listando cursos disponibles")
            for curso in self.cursos:
                print(curso)
        else:
            print("No hay cursos cargados hasta el momento")

    def mostrar_simplificado(self):
        for curso in self.cursos:
            print(curso.obtener_resumen())

    def buscar_por_codigo(self, codigo_a_buscar:int)->Curso:
        for curso in self.cursos:
            if curso.codigo == codigo_a_buscar:
                return curso
        return None

    def obtener_nuevo_codigo(self):
        return len(self.cursos) + 1

gestor_de_cursos = GestorDeCursos()

def cargar_desde_csv():
    global gestor_de_cursos
    with open('cursos_precarga.csv', 'rt', encoding='utf-8') as csv_file:
        all_lines = csv_file.readlines()
        read_lines = 0
        for line in all_lines:
            if read_lines != 0:
                rows = line.split(',')
                nuevo_curso = Curso(rows[0], rows[1], rows[2], rows[3], rows[4], rows[5], rows[6])
                gestor_de_cursos.agregar_curso(nuevo_curso)
            read_lines += 1

def escribir_en_binario():
    global gestor_de_cursos
    with open('cursos.bin','wb') as bin_file:
        bin_file.write(dumps(gestor_de_cursos.cursos))

def cargar_desde_binario():
    global gestor_de_cursos
    with open('cursos.bin','rb') as bin_file:
        gestor_de_cursos.cursos=load(bin_file)

def mezclar_por_horas_duracion(lista, inicio, medio, fin):
    # Crea listas temporales para las mitades
    izquierda = lista[inicio:medio + 1]
    derecha = lista[medio + 1:fin + 1]

    i = j = 0  # Índices para las mitades
    k = inicio  # Índice para la lista original

    # Mezcla las dos mitades
    while i < len(izquierda) and j < len(derecha):
        if izquierda[i].horas_duracion <= derecha[j].horas_duracion:
            lista[k] = izquierda[i]
            i += 1
        else:
            lista[k] = derecha[j]
            j += 1
        k += 1

    # Copia los elementos restantes de la mitad izquierda, si los hay
    while i < len(izquierda):
        lista[k] = izquierda[i]
        i += 1
        k += 1

    # Copia los elementos restantes de la mitad derecha, si los hay
    while j < len(derecha):
        lista[k] = derecha[j]
        j += 1
        k += 1

def merge_sort_por_horas_duracion(lista, inicio, fin):
    # Caso base: si la lista tiene 0 o 1 elementos
    if inicio < fin:
        medio = (inicio + fin) // 2  # Encuentra el punto medio
        # Ordena la mitad izquierda
        merge_sort_por_horas_duracion(lista, inicio, medio)
        # Ordena la mitad derecha
        merge_sort_por_horas_duracion(lista, medio + 1, fin)

        # Mezcla las dos mitades ordenadas
        mezclar_por_horas_duracion(lista, inicio, medio, fin)

def mezclar_por_codigo(lista, inicio, medio, fin):
    # Crea listas temporales para las mitades
    izquierda = lista[inicio:medio + 1]
    derecha = lista[medio + 1:fin + 1]

    i = j = 0  # Índices para las mitades
    k = inicio  # Índice para la lista original

    # Mezcla las dos mitades
    while i < len(izquierda) and j < len(derecha):
        if izquierda[i].codigo <= derecha[j].codigo:
            lista[k] = izquierda[i]
            i += 1
        else:
            lista[k] = derecha[j]
            j += 1
        k += 1

    # Copia los elementos restantes de la mitad izquierda, si los hay
    while i < len(izquierda):
        lista[k] = izquierda[i]
        i += 1
        k += 1

    # Copia los elementos restantes de la mitad derecha, si los hay
    while j < len(derecha):
        lista[k] = derecha[j]
        j += 1
        k += 1

def merge_sort_por_codigo(lista, inicio, fin):
    # Caso base: si la lista tiene 0 o 1 elementos
    if inicio < fin:
        medio = (inicio + fin) // 2  # Encuentra el punto medio
        # Ordena la mitad izquierda
        merge_sort_por_codigo(lista, inicio, medio)
        # Ordena la mitad derecha
        merge_sort_por_codigo(lista, medio + 1, fin)

        # Mezcla las dos mitades ordenadas
        mezclar_por_codigo(lista, inicio, medio, fin)

def ordenar_por_merge_sort_codigo():
    global gestor_de_cursos
    todos_los_cursos = gestor_de_cursos.cursos
    merge_sort_por_codigo(todos_los_cursos, 0, len(todos_los_cursos) - 1)

def ordenar_por_merge_sort_horas_duracion():
    global gestor_de_cursos
    todos_los_cursos = gestor_de_cursos.cursos
    merge_sort_por_horas_duracion(todos_los_cursos, 0, len(todos_los_cursos) - 1)

def buscar_curso_por_codigo_y_mostrar_categoria():
    codigo_a_buscar = int(input("ingrese el código a buscar: "))
    curso_encontrado = None
    for curso in gestor_de_cursos.cursos:
        if curso.codigo == codigo_a_buscar:
            curso_encontrado = curso
    if curso_encontrado:
        print(f"La categoría del curso con código {codigo_a_buscar} es {curso_encontrado.categoria}")
    else:
        print(f"No existe un curso con código {codigo_a_buscar}")

def mostrar_todos_los_cursos():
    global gestor_de_cursos
    gestor_de_cursos.mostrar_todos()

def mostrar_curso_por_codigo():
    global gestor_de_cursos
    codigo_a_buscar = int(input("Ingrese el código del curso que desea ver: "))
    resultado=gestor_de_cursos.buscar_por_codigo(codigo_a_buscar)
    if resultado:
        print(resultado)
    else:
        print(f"No se econtro el curso con código {codigo_a_buscar}")

def crear_nuevo_curso():
    global gestor_de_cursos
    nuevo_codigo = gestor_de_cursos.obtener_nuevo_codigo()
    nuevo_nombre = input("Ingrese en nombre del curso: ")
    nueva_desc = input("Ingrese la descripción del curso: ")
    nuevo_prof = input("Ingrese el nombre del profesor del curso: ")
    nuevo_nivel = input("Ingrese el nivel del curso: ")
    nuevas_horas = float(input("Ingrese las horas de duración del curso: "))
    nueva_categoria = input("Ingrese la categoria del curso: ")
    curso = Curso(nuevo_codigo,nuevo_nombre,nueva_desc, nuevo_prof, nuevo_nivel, nuevas_horas, nueva_categoria)
    gestor_de_cursos.agregar_curso(curso)
    escribir_en_binario()

def actualizar_curso_existente():
    global gestor_de_cursos
    print("Cursos disponibles:")
    gestor_de_cursos.mostrar_simplificado()
    curso_elegido=int(input("Elige el código curso que quieres modificar: "))
    curso_encontrado = gestor_de_cursos.buscar_por_codigo(curso_elegido)
    if curso_encontrado:
        nuevo_nombre = input("Ingrese el nuevo nombre del curso (Enter para manter el actual): ")
        nueva_desc = input("Ingrese la nueva descripción del curso (Enter para manter el actual): ")
        nuevo_prof = input("Ingrese el nuevo nombre del profesor del curso (Enter para manter el actual): ")
        nuevo_nivel = input("Ingrese el nuevo nivel del curso (Enter para manter el actual): ")
        nuevas_horas = float(input("Ingrese las nuevas horas de duración del curso (Enter para manter el actual): "))
        nueva_categoria = input("Ingrese la nueva categoria del curso (Enter para manter el actual): ")
        if nuevo_nombre:
            curso_encontrado.nombre=nuevo_nombre
        if nueva_desc:
            curso_encontrado.descripcion=nueva_desc
        if nuevo_prof:
            curso_encontrado.profesor=nuevo_prof
        if nuevo_nivel:
            curso_encontrado.nivel=nuevo_nivel
        if nuevas_horas:
            curso_encontrado.horas_duracion=nuevas_horas
        if nueva_categoria:
            curso_encontrado.categoria=nueva_categoria
        escribir_en_binario()
        print("El curso se ha actualizado correctamente")
        print(curso_encontrado)
    else:
        print(f"No se encontro el curso con código '{curso_elegido}'")

def eliminar_curso_existente():
    global gestor_de_cursos
    print("Cursos disponibles:")
    gestor_de_cursos.mostrar_simplificado()
    curso_elegido = int(input("Elige el código curso que quieres eliminar: "))
    curso_encontrado = gestor_de_cursos.buscar_por_codigo(curso_elegido)
    if curso_encontrado:
        confirmar = input(f"Confirma que desea eliminar el curso:\n{curso_encontrado}\nResponder Si/no: ")
        if confirmar.lower() in ('sí','si','s'):
            gestor_de_cursos.eliminar_curso(curso_encontrado)
            escribir_en_binario()
            print("Se ha eliminado correctamente el curso")
        else:
            print("Eliminación cancelada!")
    else:
        print(f"No se encontro el curso con código '{curso_elegido}'")

def menu_principal():
    while True:
        print("---- MENU PRINCIPAL -----")
        opcion = input("ingrese la opcion seleccionada: ")
        if opcion == "crear":
            crear_nuevo_curso()
        elif opcion == "mostrar todo":
            mostrar_todos_los_cursos()
        elif opcion == "mostrar uno":
            mostrar_curso_por_codigo()
        elif opcion == "actualizar":
            actualizar_curso_existente()
        elif opcion == "eliminar":
            eliminar_curso_existente()
        elif opcion == "salir":
            print("Muchas gracias por usar el programa.")
            break
        else:
            print("La opción ingresada no es válida")

cargar_desde_binario()
menu_principal()
```


