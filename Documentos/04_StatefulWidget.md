# StatefulWidget en Flutter.

> Un widget que sí puede cambiar. Cuando el estado se actualiza, la pantalla se redibuja automáticamente.

---

## ¿Qué es un StatefulWidget?

Un `StatefulWidget` es un widget **con estado**, lo que significa que puede **cambiar y actualizarse** mientras la app está en uso. Es ideal para:

- Contadores
- Formularios
- Toggles (activar/desactivar)
- Listas que se modifican
- Cualquier cosa que reaccione a acciones del usuario

> 📌 La diferencia clave con `StatelessWidget`: aquí la pantalla **se puede redibujar** cuando los datos cambian.

---

## Estructura básica

Un `StatefulWidget` siempre se compone de **dos clases**:

```dart
import 'package:flutter/material.dart';

// Clase 1: el widget
class MiWidget extends StatefulWidget {
  @override
  State<MiWidget> createState() => _MiWidgetState();
}

// Clase 2: el estado del widget
class _MiWidgetState extends State<MiWidget> {
  @override
  Widget build(BuildContext context) {
    return Text('Hola, soy un StatefulWidget');
  }
}
```

### Partes importantes:

| Parte | Descripción |
|---|---|
| `StatefulWidget` | El widget en sí (no cambia) |
| `createState()` | Crea el estado asociado |
| `State<MiWidget>` | Aquí viven los datos que cambian |
| `setState()` | Método que actualiza la UI cuando un dato cambia |

---

## El método setState()

`setState()` es el método más importante. Le dice a Flutter que **algo cambió** y que debe **redibujar el widget**.

```dart
// ❌ Sin setState — la pantalla NO se actualiza
_contador = _contador + 1;

// ✅ Con setState — la pantalla SÍ se actualiza
setState(() {
  _contador = _contador + 1;
});
```

---

## Ejemplo completo — Contador

El ejemplo clásico de Flutter:

```dart
import 'package:flutter/material.dart';

class PantallaContador extends StatefulWidget {
  @override
  State<PantallaContador> createState() => _PantallaContadorState();
}

class _PantallaContadorState extends State<PantallaContador> {
  // Variable que guarda el estado
  int _contador = 0;

  void _incrementar() {
    setState(() {
      _contador++; // actualiza el valor y redibuja
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Contador')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text('Has presionado el botón:'),
            Text(
              '$_contador',
              style: TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementar,
        child: Icon(Icons.add),
      ),
    );
  }
}
```

---

## Ejemplo 2 — Toggle (activar/desactivar)

```dart
class PantallaToggle extends StatefulWidget {
  @override
  State<PantallaToggle> createState() => _PantallaToggleState();
}

class _PantallaToggleState extends State<PantallaToggle> {
  bool _activado = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Toggle')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              _activado ? '✅ Activado' : '❌ Desactivado',
              style: TextStyle(fontSize: 24),
            ),
            SizedBox(height: 24),
            Switch(
              value: _activado,
              onChanged: (valor) {
                setState(() {
                  _activado = valor;
                });
              },
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## StatelessWidget vs StatefulWidget

| | StatelessWidget | StatefulWidget |
|---|---|---|
| ¿Cambia la UI? | ❌ No | ✅ Sí |
| ¿Tiene setState? | ❌ No | ✅ Sí |
| ¿Cuántas clases? | 1 | 2 |
| Uso típico | Pantallas estáticas | Formularios, contadores, listas |

---

## Ciclo de vida básico

```
createState()         → se crea el estado
    │
initState()           → se ejecuta una vez al iniciar (opcional)
    │
build()               → construye la UI
    │
setState() llamado    → redibuja la UI
    │
dispose()             → se limpia cuando el widget desaparece (opcional)
```

---

> 💡 **Tip:** Las variables que van dentro de `setState()` deben declararse en la clase `_MiWidgetState`, no dentro del `build()`. De lo contrario se resetean cada vez que la UI se redibuja.


