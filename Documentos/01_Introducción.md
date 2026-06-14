
# 1. Introducción a Flutter.

## ¿Qué es Flutter?

Flutter es un framework de código abierto creado por Google que permite desarrollar aplicaciones nativas para **Android, iOS, Web, Windows, macOS y Linux** desde un único código base escrito en **Dart**.

A diferencia de otros frameworks, Flutter no usa componentes nativos del sistema operativo. En su lugar, **dibuja cada píxel de la interfaz** usando su propio motor gráfico (Skia / Impeller), lo que garantiza consistencia visual en todas las plataformas.

---

## ¿Por qué Flutter + Chrome?

Durante el desarrollo, ejecutar la app en **Google Chrome** permite:

- Ver cambios en tiempo real con **Hot Reload**
- Inspeccionar el árbol de widgets con las DevTools
- No necesitar un emulador Android/iOS configurado
- Ciclos de desarrollo más rápidos

```bash
# Ejecutar en Chrome
flutter run -d chrome
```

---

## Arquitectura de Flutter.

Flutter se organiza en tres capas principales:

```
┌────────────────────────────┐
│        Tu App (Dart)       │  ← Código que tú escribes
├────────────────────────────┤
│     Framework de Flutter   │  ← Widgets, animaciones, routing
├────────────────────────────┤
│      Motor (C++ / Skia)    │  ← Renderizado gráfico
├────────────────────────────┤
│   Sistema Operativo / Web  │  ← Android, iOS, Chrome, etc.
└────────────────────────────┘
```

---

## Todo en Flutter es un Widget

El concepto más importante de Flutter es que **todo es un widget**:

- Un texto → `Text`
- Un botón → `ElevatedButton`
- Un margen → `Padding`
- Incluso la pantalla completa → `Scaffold`

Los widgets se anidan unos dentro de otros formando un **árbol de widgets**:

```dart
void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Mi Primera App')),
        body: Center(
          child: Text('¡Hola, Flutter!'),
        ),
      ),
    ),
  );
}
```

---

## Tipos de Widgets

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `StatelessWidget` | No cambia después de construirse | `Text`, `Icon`, `Card` |
| `StatefulWidget` | Puede cambiar en respuesta a eventos | `Checkbox`, `TextField` |
| Widgets de layout | Organizan otros widgets en pantalla | `Row`, `Column`, `Stack` |
| Widgets de material | Siguen el diseño Material Design | `AppBar`, `FloatingActionButton` |

---

## Estructura de un proyecto Flutter

Al crear un proyecto con `flutter create`, obtienes:

```
mi_app/
├── lib/
│   └── main.dart        ← Punto de entrada de la app
├── web/                 ← Archivos para la versión web (Chrome)
├── android/             ← Código nativo Android
├── ios/                 ← Código nativo iOS
├── pubspec.yaml         ← Dependencias y configuración
└── test/                ← Tests automatizados
```

El archivo más importante al empezar es `lib/main.dart`.

---

> 💡 **Nota:** En esta documentación todo el código está probado ejecutándose en **Google Chrome** como dispositivo de destino.

---

