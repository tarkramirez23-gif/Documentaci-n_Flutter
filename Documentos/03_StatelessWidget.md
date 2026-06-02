# StatelessWidget en Flutter

> Un widget que no cambia. Muestra información fija que no se actualiza mientras la app está corriendo.

---

## ¿Qué es un StatelessWidget?

Un `StatelessWidget` es un widget **sin estado**, lo que significa que una vez que se construye, su contenido **no cambia**. Es perfecto para mostrar información estática como:

- Textos fijos
- Íconos
- Imágenes
- Pantallas de bienvenida
- Tarjetas con información que no varía

> 📌 La palabra "state" significa "estado". Sin estado = sin cambios.

---

## Estructura básica

```dart
import 'package:flutter/material.dart';

class MiWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('Hola, soy un StatelessWidget');
  }
}
```

### Partes importantes:

| Parte | Descripción |
|---|---|
| `class MiWidget` | Nombre de tu widget (siempre en PascalCase) |
| `extends StatelessWidget` | Indica que es un widget sin estado |
| `build()` | Método que construye y devuelve la UI |
| `BuildContext context` | Información sobre la posición del widget en el árbol |

---

## Ejemplo completo

Una pantalla de bienvenida hecha con `StatelessWidget`:

```dart
import 'package:flutter/material.dart';

class PantallaBienvenida extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bienvenida'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(
              Icons.flutter_dash,
              size: 80,
              color: Colors.blue,
            ),
            SizedBox(height: 16),
            Text(
              '¡Bienvenido a Flutter!',
              style: TextStyle(
                fontSize: 24,
                fontWeight: FontWeight.bold,
              ),
            ),
            SizedBox(height: 8),
            Text(
              'Empieza a construir tu app',
              style: TextStyle(
                fontSize: 16,
                color: Colors.grey,
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## StatelessWidget con parámetros

Puedes pasarle datos a un `StatelessWidget` mediante el constructor:

```dart
class TarjetaUsuario extends StatelessWidget {
  // Parámetros que recibe el widget
  final String nombre;
  final String correo;

  // Constructor
  const TarjetaUsuario({
    required this.nombre,
    required this.correo,
  });

  @override
  Widget build(BuildContext context) {
    return Card(
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              nombre,
              style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
            ),
            SizedBox(height: 4),
            Text(correo),
          ],
        ),
      ),
    );
  }
}
```

### ¿Cómo usarlo?

```dart
// Pasar los datos al widget
TarjetaUsuario(
  nombre: 'Juan Pérez',
  correo: 'juan@correo.com',
)
```

---

## ¿Cuándo usar StatelessWidget?

✅ Úsalo cuando:
- La información que muestra **no cambia**
- No necesitas responder a acciones del usuario
- Es un componente visual reutilizable con datos fijos

❌ No lo uses cuando:
- El usuario interactúa y la pantalla debe **actualizarse**
- Necesitas un contador, formulario, o cualquier dato que cambie

---

## Resumen

```
StatelessWidget
    │
    └── build()
            │
            └── Devuelve la UI (widgets)
                No cambia una vez construido
```

> 💡 **Tip:** Si tu widget solo muestra información y no reacciona a nada, usa `StatelessWidget`. Es más simple y eficiente.

