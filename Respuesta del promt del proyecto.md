Para construir una arquitectura de agentes robusta basada en la capacidad **.agents** y desarrollar el CRUD de la tienda de videojuegos, seguiremos una secuencia lógica de ingeniería. 

Primero, estableceremos la estructura de la "habilidad" (Skill) para que el entorno reconozca las capacidades de automatización, y luego pasaremos al desarrollo técnico en Flutter.

---

## Fase 1: Configuración de Capacidades (.agents)

Para que tu entorno de trabajo sea inteligente, creamos la carpeta raíz de habilidades. Esto permite que los IDEs compatibles (como Antigravity) entiendan las reglas de diseño, código y scraping.

### Estructura de Carpetas
Crea un directorio llamado `.agents` en la raíz de tu espacio de trabajo con la siguiente estructura:

* **SKILL.md**: El cerebro. Define qué pueden hacer los agentes.
* **scripts/**: Scripts de automatización (Python/Bash).
* **ejemplos/**: Snippets de código de referencia.
* **resources/**: Activos, iconos y esquemas de base de datos.

---

## Fase 2: Entorno y Prerrequisitos

Antes de codificar, validamos que las herramientas de comunicación con Google Cloud y Flutter estén listas.

<Steps>
{/* Reason: La secuencia de instalación y login es crítica; si se salta el login de Firebase o el pubspec, el código de la App fallará al compilar. */}
  <Step title="Verificación de Flutter" subtitle="Terminal">
    Ejecuta `flutter --version`. Si no lo tienes, descárgalo de [flutter.dev](https://docs.flutter.dev/get-started/install).
  </Step>
  <Step title="Instalación de CLI" subtitle="Automatización">
    Instala la herramienta de base ejecutando: `npm install -g flutterbase-cli` o sigue las instrucciones del repositorio oficial de tu organización.
  </Step>
  <Step title="Conectividad Firebase" subtitle="Autenticación">
    Ejecuta `firebase login` en tu terminal. Esto abrirá tu navegador para vincular tu cuenta de Google con el CLI.
  </Step>
  <Step title="Preparación de Firestore" subtitle="Consola Firebase">
    Crea un proyecto en la consola, activa **Firestore Database** en modo prueba y registra tu app (Android/iOS). Descarga el archivo `google-services.json` y colócalo en `android/app/`.
  </Step>
</Steps>

---

## Fase 3: Desarrollo del Proyecto "proyecvideojuegos"

### 1. Configuración del `pubspec.yaml`
Agrega las dependencias necesarias para conectar con Firestore y gestionar el estado.

```yaml
name: proyecvideojuegos
description: Tienda de Videojuegos Online CRUD
publish_to: 'none'
version: 1.0.0+1

environment:
  sdk: '>=3.0.0 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^2.15.0
  cloud_firestore: ^4.8.3
  cupertino_icons: ^1.0.2

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^2.0.0
```

### 2. Modelo de Datos (`lib/models/game_model.dart`)
Definimos qué es un "Videojuego" para nuestra tienda.

```dart
class Game {
  String id;
  String title;
  String genre;
  double price;

  Game({required this.id, required this.title, required this.genre, required this.price});

  Map<String, dynamic> toMap() {
    return {'title': title, 'genre': genre, 'price': price};
  }

  static Game fromMap(Map<String, dynamic> map, String id) {
    return Game(
      id: id,
      title: map['title'] ?? '',
      genre: map['genre'] ?? '',
      price: (map['price'] ?? 0.0).toDouble(),
    );
  }
}
```

### 3. Servicio Firestore (`lib/services/firebase_service.dart`)
Aquí reside la lógica del CRUD que el agente de código automatizará.

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/game_model.dart';

class FirebaseService {
  final CollectionReference gamesRef = FirebaseFirestore.instance.collection('videojuegos');

  // CREATE
  Future<void> addGame(Game game) => gamesRef.add(game.toMap());

  // READ
  Stream<List<Game>> getGames() {
    return gamesRef.snapshots().map((snapshot) =>
        snapshot.docs.map((doc) => Game.fromMap(doc.data() as Map<String, dynamic>, doc.id)).toList());
  }

  // UPDATE
  Future<void> updateGame(Game game) => gamesRef.doc(game.id).update(game.toMap());

  // DELETE
  Future<void> deleteGame(String id) => gamesRef.doc(id).delete();
}
```

### 4. Interfaz de Usuario: Pantalla CRUD (`lib/screens/home_screen.dart`)
Una UI limpia con navegación para gestionar la tienda.

```dart
import 'package:flutter/material.dart';
import '../services/firebase_service.dart';
import '../models/game_model.dart';

class HomeScreen extends StatelessWidget {
  final FirebaseService service = FirebaseService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Tienda Gamer .agents')),
      body: StreamBuilder<List<Game>>(
        stream: service.getGames(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          final games = snapshot.data!;
          return ListView.builder(
            itemCount: games.size,
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(games[index].title),
                subtitle: Text("${games[index].genre} - \$${games[index].price}"),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => service.deleteGame(games[index].id),
                ),
                onTap: () => _showForm(context, games[index]),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _showForm(context, null),
      ),
    );
  }

  void _showForm(BuildContext context, Game? game) {
    // Aquí se implementaría el modal con textfields para Título, Género y Precio
    // usando service.addGame o service.updateGame
  }
}
```

---

## Verificación de Funcionalidad
Una vez generado el código, ejecuta en tu terminal dentro de la carpeta `proyecvideojuegos`:

1.  `flutter pub get` (Para instalar dependencias).
2.  `flutter doctor` (Para asegurar que el emulador/dispositivo está listo).
3.  `flutter run` (Para lanzar la app).

> **Nota del Agente:** Recuerda que para el Scraping Skill dentro de `.agents`, deberás definir selectores específicos en `scripts/` si planeas traer precios automáticamente de otras tiendas.

<Elicitations message="¿En qué área del proyecto deseas profundizar ahora?">
  <Elicitation label="Completar el formulario UI del CRUD" query="Genera el código completo del modal de formulario (TextFields y validación) para agregar y editar videojuegos en Flutter." />
  <Elicitation label="Configurar el Skill de Scraping" query="¿Cómo configuro el archivo scripts/ dentro de .agents para extraer precios de videojuegos automáticamente?" />
</Elicitations>
