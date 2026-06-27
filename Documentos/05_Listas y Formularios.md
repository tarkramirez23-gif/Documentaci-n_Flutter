# Listas y Formularios en Flutter.

> Aprende a mostrar colecciones de datos y a capturar información del usuario de forma estructurada.

---

## Parte 1: Listas.

### ¿Qué es una lista en Flutter?

Una lista muestra múltiples elementos de forma ordenada y desplazable. En Flutter el widget principal para esto es `ListView`.

---

### ListView básico

```dart
ListView(
  children: [
    ListTile(title: Text('Elemento 1')),
    ListTile(title: Text('Elemento 2')),
    ListTile(title: Text('Elemento 3')),
  ],
)
```

---

### ListView.builder — para listas dinámicas.

Cuando tienes muchos elementos o una lista que viene de datos reales, usa `ListView.builder`. Es más eficiente porque solo construye los elementos visibles.

```dart
import 'package:flutter/material.dart';

class PantallaLista extends StatelessWidget {
  final List<String> frutas = [
    'Manzana', 'Banana', 'Naranja', 'Mango', 'Uva',
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Mi Lista')),
      body: ListView.builder(
        itemCount: frutas.length,
        itemBuilder: (context, index) {
          return ListTile(
            leading: Icon(Icons.circle),
            title: Text(frutas[index]),
            subtitle: Text('Índice: $index'),
          );
        },
      ),
    );
  }
}
```

| Propiedad | Descripción |
|---|---|
| `itemCount` | Número total de elementos |
| `itemBuilder` | Función que construye cada elemento |
| `index` | Posición actual del elemento (0, 1, 2...) |

---

### Lista con objetos

```dart
class Producto {
  final String nombre;
  final double precio;
  Producto({required this.nombre, required this.precio});
}

class ListaProductos extends StatelessWidget {
  final List<Producto> productos = [
    Producto(nombre: 'Laptop', precio: 1500.00),
    Producto(nombre: 'Mouse', precio: 25.00),
    Producto(nombre: 'Teclado', precio: 45.00),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Productos')),
      body: ListView.builder(
        itemCount: productos.length,
        itemBuilder: (context, index) {
          final producto = productos[index];
          return Card(
            margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
            child: ListTile(
              title: Text(producto.nombre),
              trailing: Text(
                'S/ ${producto.precio.toStringAsFixed(2)}',
                style: TextStyle(fontWeight: FontWeight.bold, color: Colors.green),
              ),
            ),
          );
        },
      ),
    );
  }
}
```

---

## Parte 2: Formularios

### ¿Qué es un Form en Flutter?

El widget `Form` agrupa campos de texto (`TextFormField`) y permite **validarlos todos a la vez** antes de procesar los datos.

> 📌 La diferencia entre `TextField` y `TextFormField` es que el segundo funciona dentro de un `Form` y permite agregar validaciones.

---

### Estructura básica de un Form

```dart
final _formKey = GlobalKey<FormState>();

Form(
  key: _formKey,
  child: Column(
    children: [
      TextFormField(
        decoration: InputDecoration(labelText: 'Nombre'),
        validator: (valor) {
          if (valor == null || valor.isEmpty) {
            return 'Este campo es obligatorio';
          }
          return null; // null = válido
        },
      ),
      ElevatedButton(
        onPressed: () {
          if (_formKey.currentState!.validate()) {
            print('Formulario válido');
          }
        },
        child: Text('Enviar'),
      ),
    ],
  ),
)
```

---

### Formulario completo con validaciones

```dart
import 'package:flutter/material.dart';

class FormularioRegistro extends StatefulWidget {
  @override
  State<FormularioRegistro> createState() => _FormularioRegistroState();
}

class _FormularioRegistroState extends State<FormularioRegistro> {
  final _formKey = GlobalKey<FormState>();
  final _nombreController = TextEditingController();
  final _correoController = TextEditingController();
  final _passwordController = TextEditingController();

  void _enviar() {
    if (_formKey.currentState!.validate()) {
      print('Nombre: ${_nombreController.text}');
      print('Correo: ${_correoController.text}');
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Registro')),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: Form(
          key: _formKey,
          child: Column(
            children: [
              TextFormField(
                controller: _nombreController,
                decoration: InputDecoration(
                  labelText: 'Nombre completo',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.person),
                ),
                validator: (valor) {
                  if (valor == null || valor.isEmpty) {
                    return 'Por favor ingresa tu nombre';
                  }
                  return null;
                },
              ),
              SizedBox(height: 16),
              TextFormField(
                controller: _correoController,
                decoration: InputDecoration(
                  labelText: 'Correo electrónico',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.email),
                ),
                keyboardType: TextInputType.emailAddress,
                validator: (valor) {
                  if (valor == null || valor.isEmpty) {
                    return 'Por favor ingresa tu correo';
                  }
                  if (!valor.contains('@')) {
                    return 'Ingresa un correo válido';
                  }
                  return null;
                },
              ),
              SizedBox(height: 16),
              TextFormField(
                controller: _passwordController,
                decoration: InputDecoration(
                  labelText: 'Contraseña',
                  border: OutlineInputBorder(),
                  prefixIcon: Icon(Icons.lock),
                ),
                obscureText: true,
                validator: (valor) {
                  if (valor == null || valor.isEmpty) {
                    return 'Por favor ingresa una contraseña';
                  }
                  if (valor.length < 6) {
                    return 'Mínimo 6 caracteres';
                  }
                  return null;
                },
              ),
              SizedBox(height: 24),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: _enviar,
                  style: ElevatedButton.styleFrom(
                    padding: EdgeInsets.symmetric(vertical: 16),
                  ),
                  child: Text('Registrarse', style: TextStyle(fontSize: 16)),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

---

## Lista + Formulario combinados

Un formulario que agrega elementos a una lista en tiempo real:

```dart
class ListaConFormulario extends StatefulWidget {
  @override
  State<ListaConFormulario> createState() => _ListaConFormularioState();
}

class _ListaConFormularioState extends State<ListaConFormulario> {
  final _controller = TextEditingController();
  final List<String> _elementos = [];

  void _agregar() {
    if (_controller.text.isNotEmpty) {
      setState(() {
        _elementos.add(_controller.text);
        _controller.clear();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Mi Lista')),
      body: Column(
        children: [
          Padding(
            padding: EdgeInsets.all(16),
            child: Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _controller,
                    decoration: InputDecoration(
                      labelText: 'Nuevo elemento',
                      border: OutlineInputBorder(),
                    ),
                  ),
                ),
                SizedBox(width: 8),
                ElevatedButton(
                  onPressed: _agregar,
                  child: Icon(Icons.add),
                ),
              ],
            ),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: _elementos.length,
              itemBuilder: (context, index) {
                return ListTile(
                  leading: Icon(Icons.check_circle_outline),
                  title: Text(_elementos[index]),
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

---

## Resumen

| Widget | Para qué sirve |
|---|---|
| `ListView` | Lista de elementos fijos |
| `ListView.builder` | Lista dinámica o con muchos elementos |
| `ListTile` | Elemento estándar de lista |
| `Form` | Agrupa y valida campos de texto |
| `TextFormField` | Campo de texto con validación |
| `GlobalKey<FormState>` | Controla y valida el formulario |

---

> 💡 **Tip:** Siempre libera los controladores cuando el widget desaparezca:

```dart
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```


