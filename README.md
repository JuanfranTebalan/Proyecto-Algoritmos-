# Proyecto-Algoritmos-
Se presentan 2 Ejercicios hechos en C++ y Python que permite al usuario crear, guardar y buscar datos donde puede ingresar: nombre, codigo, proveedor existencia, precio y estado de descuento 
cpp
#include <iostream>
#include <fstream>
#include <vector>
#include <string>

using namespace std;

struct Producto {
    string nombre;
    string codigo;
    float precio;
    string proveedor;
    int existencia;
    char estado;
    float descuento;
};

// Función para agregar un producto al archivo
void agregarProducto(vector<Producto>& productos, ofstream& archivo) {
    Producto producto;
    cout << "Ingrese el nombre del producto: ";
    cin >> producto.nombre;
    cout << "Ingrese el código del producto: ";
    cin >> producto.codigo;
    // Verificar si el código ya existe
    for (const Producto& p : productos) {
        if (p.codigo == producto.codigo) {
            cout << "El código de producto ya existe." << endl;
            return;
        }
    }
    cout << "Ingrese el precio del producto: ";
    cin >> producto.precio;
    cout << "Ingrese el proveedor: ";
    cin >> producto.proveedor;
    cout << "Ingrese la existencia del producto: ";
    cin >> producto.existencia;
    cout << "Ingrese el estado (A = Aprobado, R = Reprobado): ";
    cin >> producto.estado;
    cout << "Ingrese el descuento del producto: ";
    cin >> producto.descuento;
    
    productos.push_back(producto);
    archivo << producto.nombre << " " << producto.codigo << " " << producto.precio << " " << producto.proveedor << " " << producto.existencia << " " << producto.estado << " " << producto.descuento << endl;
    cout << "Producto agregado con éxito." << endl;
}
Función para buscar un producto por código o nombre
void buscarProducto(const vector<Producto>& productos, const string& clave) {
    for (const Producto& producto : productos) {
        if (producto.codigo == clave || producto.nombre.find(clave) != string::npos) {
            cout << "Nombre: " << producto.nombre << endl;
            cout << "Código: " << producto.codigo << endl;
            cout << "Precio: " << producto.precio << endl;
            cout << "Proveedor: " << producto.proveedor << endl;
            cout << "Existencia: " << producto.existencia << endl;
            cout << "Estado: " << producto.estado << endl;
            cout << "Descuento: " << producto.descuento << endl;
            return;
        }
    }
    cout << "Producto no encontrado." << endl;
}

int main() {
    vector<Producto> productos;
    ifstream archivoEntrada("productos.txt");
    string nombreArchivo = "productos.txt";
    if (!archivoEntrada) {
        ofstream archivoSalida(nombreArchivo);
        archivoSalida.close();
    } else {
        string nombre, codigo, proveedor, estado;
        float precio, descuento;
        int existencia;
        while (archivoEntrada >> nombre >> codigo >> precio >> proveedor >> existencia >> estado >> descuento) {
            Producto producto = {nombre, codigo, precio, proveedor, existencia, estado[0], descuento};
            productos.push_back(producto);
        }
        archivoEntrada.close();
    }
    
    int opcion;
    while (true) {
        cout << "Menú:" << endl;
        cout << "1. Agregar producto" << endl;
        cout << "2. Buscar producto" << endl;
        cout << "3. Salir" << endl;
        cout << "Seleccione una opción: ";
        cin >> opcion;
        
        if (opcion == 1) {
            ofstream archivoSalida(nombreArchivo, ios::app);
            agregarProducto(productos, archivoSalida);
            archivoSalida.close();
        } else if (opcion == 2) {
            string clave;
            cout << "Ingrese el código o nombre del producto: ";
            cin >> clave;
            buscarProducto(productos, clave);
        } else if (opcion == 3) {
            break;
        } else {
            cout << "Opción no válida." << endl;
        }
    }
    
    return 0;
}py

import os

class Producto:
    def __init__(self, nombre, codigo, precio, proveedor, existencia, estado, descuento):
        self.nombre = nombre
        self.codigo = codigo
        self.precio = precio
        self.proveedor = proveedor
        self.existencia = existencia
        self.estado = estado
        self.descuento = descuento

Función para agregar un producto al archivo
def agregar_producto(productos, archivo):
    nombre = input("Ingrese el nombre del producto: ")
    codigo = input("Ingrese el código del producto: ")
    
    # Verificar si el código ya existe
    for producto in productos:
        if producto.codigo == codigo:
            print("El código de producto ya existe.")
            return
    
    precio = float(input("Ingrese el precio del producto: "))
    proveedor = input("Ingrese el proveedor: ")
    existencia = int(input("Ingrese la existencia del producto: "))
    estado = input("Ingrese el estado (A = Aprobado, R = Reprobado): ").upper()
    descuento = float(input("Ingrese el descuento del producto: "))
    
    nuevo_producto = Producto(nombre, codigo, precio, proveedor, existencia, estado, descuento)
    productos.append(nuevo_producto)
    
    with open(archivo, 'a') as f:
        f.write(f"{nombre} {codigo} {precio} {proveedor} {existencia} {estado} {descuento}\n")
    
    print("Producto agregado con éxito.")

Función para buscar un producto por código o nombre
def buscar_producto(productos, clave):
    for producto in productos:
        if producto.codigo == clave or clave in producto.nombre:
            print("Nombre:", producto.nombre)
            print("Código:", producto.codigo)
            print("Precio:", producto.precio)
            print("Proveedor:", producto.proveedor)
            print("Existencia:", producto.existencia)
            print("Estado:", producto.estado)
            print("Descuento:", producto.descuento)
            return
    print("Producto no encontrado.")

def main():
    archivo = "productos.txt"
    productos = []

    if os.path.exists(archivo):
        with open(archivo, 'r') as f:
            for line in f:
                nombre, codigo, precio, proveedor, existencia, estado, descuento = line.strip().split()
                productos.append(Producto(nombre, codigo, float(precio), proveedor, int(existencia), estado, float(descuento)))

    while True:
        print("Menú:")
        print("1. Agregar producto")
        print("2. Buscar producto")
        print("3. Salir")
        opcion = input("Seleccione una opción: ")
        
        if opcion == "1":
            agregar_producto(productos, archivo)
        elif opcion == "2":
            clave = input("Ingrese el código o nombre del producto: ")
            buscar_producto(productos, clave)
        elif opcion == "3":
            break
        else:
            print("Opción no válida.")

if __name__ == "__main__":
    main()

    # Aplicación de Gestión de Productos

Este programa es una aplicación de consola en C++ que permite gestionar una lista de productos. Proporciona las siguientes funcionalidades:

1. **Agregar Producto**: Permite al usuario ingresar datos de un producto, como nombre, código, precio, proveedor, existencia, estado y descuento. Verifica si el código del producto ya existe en la lista antes de agregarlo. 

2. **Buscar Producto**: Permite al usuario buscar un producto por su código o nombre. Si se encuentra un producto que coincide con el código o contiene el nombre ingresado, se muestran sus detalles.

3. **Modificar Datos de un Producto**: Permite al usuario modificar los datos de un producto existente, excepto el código.

4. **Cargar Datos**: Al iniciar la aplicación, verifica si el archivo "productos.txt" existe y, si es así, carga los datos de productos almacenados en el archivo en una lista de productos. Esto asegura que los datos previamente guardados estén disponibles para su manipulación.

## Uso

El programa muestra un menú con opciones
