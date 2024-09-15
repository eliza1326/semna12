# semana12
#creacion de un programa para una biblioteca mediante la creacion de la clase libro y de diversas funcionlidades para el uso de un programa que ayude al bibliotecario de manera eficiente el estado de un libo y en donde se encuentra.

n ejemplo de código en Python para un Sistema de Gestión de Biblioteca Digital que cumple con los requisitos que mencionaste:

python
Copiar código
# Clase Libro
class Libro:
    def __init__(self, titulo, autor, categoria, isbn):
        self.titulo = titulo
        self.autor = autor
        self.categoria = categoria
        self.isbn = isbn

    def __str__(self):
        return f"Libro: {self.titulo} | Autor: {self.autor} | Categoría: {self.categoria} | ISBN: {self.isbn}"

# Clase Usuario
class Usuario:
    def __init__(self, nombre, id_usuario):
        self.nombre = nombre
        self.id_usuario = id_usuario
        self.libros_prestados = []  # Lista de libros prestados

    def prestar_libro(self, libro):
        self.libros_prestados.append(libro)

    def devolver_libro(self, libro):
        self.libros_prestados.remove(libro)

    def __str__(self):
        return f"Usuario: {self.nombre} | ID: {self.id_usuario}"

# Clase Biblioteca
class Biblioteca:
    def __init__(self):
        self.libros = {}  # Diccionario con ISBN como clave y objeto Libro como valor
        self.usuarios = {}  # Diccionario con ID de usuario como clave y objeto Usuario como valor
        self.ids_usuarios = set()  # Conjunto para manejar IDs únicos de usuarios

    # Añadir libro a la biblioteca
    def agregar_libro(self, titulo, autor, categoria, isbn):
        if isbn not in self.libros:
            nuevo_libro = Libro(titulo, autor, categoria, isbn)
            self.libros[isbn] = nuevo_libro
            print(f"Libro '{titulo}' agregado a la biblioteca.")
        else:
            print(f"El libro con ISBN {isbn} ya está registrado.")

    # Quitar libro de la biblioteca
    def quitar_libro(self, isbn):
        if isbn in self.libros:
            libro = self.libros.pop(isbn)
            print(f"Libro '{libro.titulo}' eliminado de la biblioteca.")
        else:
            print(f"No se encontró el libro con ISBN {isbn}.")

    # Registrar un nuevo usuario
    def registrar_usuario(self, nombre, id_usuario):
        if id_usuario not in self.ids_usuarios:
            nuevo_usuario = Usuario(nombre, id_usuario)
            self.usuarios[id_usuario] = nuevo_usuario
            self.ids_usuarios.add(id_usuario)
            print(f"Usuario '{nombre}' registrado con ID {id_usuario}.")
        else:
            print(f"El usuario con ID {id_usuario} ya está registrado.")

    # Dar de baja un usuario
    def dar_de_baja_usuario(self, id_usuario):
        if id_usuario in self.usuarios:
            usuario = self.usuarios.pop(id_usuario)
            self.ids_usuarios.remove(id_usuario)
            print(f"Usuario '{usuario.nombre}' dado de baja.")
        else:
            print(f"No se encontró al usuario con ID {id_usuario}.")

    # Prestar libro a un usuario
    def prestar_libro(self, id_usuario, isbn):
        if id_usuario in self.usuarios and isbn in self.libros:
            usuario = self.usuarios[id_usuario]
            libro = self.libros[isbn]
            usuario.prestar_libro(libro)
            print(f"Libro '{libro.titulo}' prestado a {usuario.nombre}.")
        else:
            print("Usuario o libro no encontrado.")

    # Devolver libro a la biblioteca
    def devolver_libro(self, id_usuario, isbn):
        if id_usuario in self.usuarios and isbn in self.libros:
            usuario = self.usuarios[id_usuario]
            libro = self.libros[isbn]
            if libro in usuario.libros_prestados:
                usuario.devolver_libro(libro)
                print(f"Libro '{libro.titulo}' devuelto por {usuario.nombre}.")
            else:
                print(f"El usuario '{usuario.nombre}' no tiene prestado el libro '{libro.titulo}'.")
        else:
            print("Usuario o libro no encontrado.")

    # Buscar libro por título, autor o categoría
    def buscar_libro(self, criterio):
        encontrados = [libro for libro in self.libros.values() if criterio.lower() in libro.titulo.lower() or criterio.lower() in libro.autor.lower() or criterio.lower() in libro.categoria.lower()]
        if encontrados:
            for libro in encontrados:
                print(libro)
        else:
            print(f"No se encontraron libros que coincidan con '{criterio}'.")

    # Listar libros prestados por un usuario
    def listar_libros_prestados(self, id_usuario):
        if id_usuario in self.usuarios:
            usuario = self.usuarios[id_usuario]
            if usuario.libros_prestados:
                print(f"Libros prestados a {usuario.nombre}:")
                for libro in usuario.libros_prestados:
                    print(libro)
            else:
                print(f"El usuario '{usuario.nombre}' no tiene libros prestados.")
        else:
            print(f"No se encontró al usuario con ID {id_usuario}.")

# Ejemplo de uso del sistema

biblioteca = Biblioteca()

# Agregar libros
biblioteca.agregar_libro("1984", "George Orwell", "Ficción", "1234567890")
biblioteca.agregar_libro("Cien Años de Soledad", "Gabriel García Márquez", "Realismo Mágico", "0987654321")

# Registrar usuarios
biblioteca.registrar_usuario("Alice", 1)
biblioteca.registrar_usuario("Bob", 2)

# Prestar libros
biblioteca.prestar_libro(1, "1234567890")

# Listar libros prestados
biblioteca.listar_libros_prestados(1)

# Buscar libros
biblioteca.buscar_libro("Orwell")

# Devolver libros
biblioteca.devolver_libro(1, "1234567890")

# Quitar libro de la biblioteca
biblioteca.quitar_libro("1234567890")

# Dar de baja usuario
biblioteca.dar_de_baja_usuario(1)
