# Diseño de soluciones integrando múltiples estructuras de datos

## Propósito de la actividad
Analizar problemas de programación que requieren manipular distintos tipos de información, proponiendo soluciones en las que se integren dos o más estructuras de datos de forma coherente, justificada y eficiente.

## Instrucciones para el estudiante
A continuación se presentan dos problemas prácticos. Para cada problema, deberás:

1. Describir brevemente el propósito del sistema propuesto.
2. Identificar las estructuras de datos que utilizarías (por ejemplo: listas, pilas, colas, árboles, arreglos, etc.).  
3. Explicar cómo se integran entre sí dichas estructuras y qué función cumple cada una dentro del sistema.  
4. Justificar la elección de cada estructura en función de las operaciones necesarias (inserción, eliminación, búsqueda, recorrido, ordenamiento, etc.).  
5. (Opcional) Representar el diseño mediante un diagrama o pseudocódigo para ilustrar la lógica general.

*Se evaluará la claridad de la propuesta, la coherencia en la integración de estructuras y la justificación técnica.*

---

## Problema 1: Sistema de gestión de biblioteca digital

Una biblioteca digital desea implementar un sistema que permita:

- Registrar libros disponibles y usuarios que los solicitan.  
- Almacenar la información de los libros (título, autor, categoría, estado).  
- Permitir búsquedas por título o categoría.  
- Llevar control de los préstamos, de forma que los usuarios sean atendidos por orden de solicitud.  
- Mantener un historial de los préstamos realizados.

### Indicaciones
Diseña una propuesta de solución que combine al menos dos estructuras de datos, por ejemplo:

- Una lista enlazada o árbol binario para almacenar y buscar libros.  
- Una cola (queue) para manejar el orden de solicitudes de préstamo.  
- Otras estructuras que consideres necesarias.

Explica cómo interactúan las estructuras entre sí para mantener la información organizada y funcional.

---

## SOLUCIÓN AL PROBLEMA 1: Sistema de Gestión de Biblioteca Digital

### 1. Propósito del Sistema

El sistema de gestión de biblioteca digital tiene como objetivo principal administrar de manera eficiente el inventario de libros, gestionar las solicitudes de préstamo de los usuarios mediante un sistema de turnos justo (FIFO), y mantener un registro histórico completo de todas las transacciones de préstamo realizadas. Esto permite una gestión organizada, trazable y escalable de los recursos bibliográficos.

### 2. Estructuras de Datos Utilizadas

El sistema integra las siguientes estructuras de datos:

#### a) **Árbol Binario de Búsqueda (BST)** - Para el catálogo de libros
- **Propósito**: Almacenar y organizar todos los libros disponibles en la biblioteca.
- **Criterio de ordenamiento**: Por título del libro (orden alfabético).
- **Ventajas**: Búsqueda eficiente O(log n) en promedio, inserción y eliminación ordenadas.

#### b) **Tabla Hash (Diccionario)** - Para búsqueda por categoría
- **Propósito**: Indexar los libros por categoría para búsquedas rápidas.
- **Clave**: Nombre de la categoría (ej: "Ficción", "Ciencia", "Historia").
- **Valor**: Lista de referencias a los nodos del árbol que pertenecen a esa categoría.
- **Ventajas**: Búsqueda O(1) por categoría.

#### c) **Cola (Queue)** - Para solicitudes de préstamo
- **Propósito**: Gestionar las solicitudes de préstamo pendientes.
- **Tipo**: Cola FIFO (First In, First Out).
- **Ventajas**: Garantiza equidad en el orden de atención, operaciones O(1) para encolar y desencolar.

#### d) **Lista Enlazada Doble** - Para historial de préstamos
- **Propósito**: Mantener un registro cronológico de todos los préstamos realizados.
- **Ventajas**: Inserción eficiente al final O(1), recorrido bidireccional, no requiere tamaño predefinido.

#### e) **Tabla Hash (Diccionario)** - Para registro de usuarios
- **Propósito**: Almacenar información de usuarios registrados.
- **Clave**: ID o cédula del usuario.
- **Valor**: Objeto Usuario (nombre, correo, libros prestados actualmente, etc.).
- **Ventajas**: Búsqueda y actualización O(1).

### 3. Integración de las Estructuras

Las estructuras de datos interactúan de la siguiente manera:

```
┌─────────────────────────────────────────────────────────────┐
│                    SISTEMA DE BIBLIOTECA                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────┐         ┌──────────────────┐         │
│  │  Árbol BST       │◄────────│  Hash Categorías │         │
│  │  (por título)    │         │  (índice rápido) │         │
│  └────────┬─────────┘         └──────────────────┘         │
│           │                                                  │
│           │ Referencia                                       │
│           ▼                                                  │
│  ┌──────────────────┐                                       │
│  │  Nodo Libro      │                                       │
│  │  - título        │                                       │
│  │  - autor         │                                       │
│  │  - categoría     │                                       │
│  │  - estado        │──────┐                                │
│  └──────────────────┘      │                                │
│                             │                                │
│                             ▼                                │
│  ┌──────────────────┐   ┌──────────────────┐               │
│  │  Cola Préstamos  │   │  Hash Usuarios   │               │
│  │  (FIFO)          │   │  (por ID)        │               │
│  │                  │   │                  │               │
│  │ [User1, Libro A] │   │ User1 -> datos   │               │
│  │ [User2, Libro B] │   │ User2 -> datos   │               │
│  │ [User3, Libro C] │   │ User3 -> datos   │               │
│  └────────┬─────────┘   └──────────────────┘               │
│           │                                                  │
│           │ Procesar solicitud                               │
│           ▼                                                  │
│  ┌──────────────────────────────────────┐                   │
│  │  Lista Enlazada Doble - Historial    │                   │
│  │  ┌───────┐   ┌───────┐   ┌───────┐  │                   │
│  │  │Préstamo│◄─►│Préstamo│◄─►│Préstamo│ │                   │
│  │  │   1    │   │   2    │   │   3    │ │                   │
│  │  └───────┘   └───────┘   └───────┘  │                   │
│  └──────────────────────────────────────┘                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Flujo de operaciones:**

1. **Registro de libros**: Los libros se insertan en el BST por título y simultáneamente se indexan en la tabla hash de categorías.

2. **Búsqueda de libros**:
   - Por título: Se busca directamente en el BST - O(log n)
   - Por categoría: Se consulta la tabla hash de categorías - O(1) + O(k) donde k es el número de libros en esa categoría

3. **Solicitud de préstamo**:
   - El usuario solicita un libro
   - Se verifica disponibilidad en el BST
   - Si está disponible, se encola la solicitud en la Cola de Préstamos
   - Se almacena referencia al usuario y al libro

4. **Procesamiento de préstamos**:
   - Se desencola la primera solicitud (FIFO)
   - Se actualiza el estado del libro en el BST (disponible → prestado)
   - Se actualiza la información del usuario en la tabla hash de usuarios
   - Se registra la transacción en la lista enlazada de historial

5. **Devolución de libros**:
   - Se actualiza el estado del libro en el BST (prestado → disponible)
   - Se actualiza la lista de libros del usuario
   - Se registra la devolución en el historial

### 4. Justificación de cada Estructura

#### **Árbol Binario de Búsqueda (BST)**
- **Operaciones necesarias**: Inserción, búsqueda por título, eliminación, recorrido in-order.
- **Justificación**: 
  - Mantiene los libros ordenados alfabéticamente por título
  - Búsqueda eficiente: O(log n) en promedio
  - Permite listar todos los libros en orden alfabético con recorrido in-order
  - Facilita operaciones de rango (ej: buscar libros entre "A" y "M")

#### **Tabla Hash para Categorías**
- **Operaciones necesarias**: Inserción, búsqueda por categoría.
- **Justificación**:
  - Acceso inmediato a todos los libros de una categoría: O(1)
  - Complementa al BST permitiendo búsquedas por un criterio diferente
  - Ideal para filtros rápidos en la interfaz de usuario
  - Escalable: agregar nuevas categorías no afecta el rendimiento

#### **Cola (Queue) para Solicitudes**
- **Operaciones necesarias**: Encolar solicitud, desencolar (procesar) solicitud, consultar siguiente.
- **Justificación**:
  - FIFO garantiza equidad: quien solicita primero, es atendido primero
  - Operaciones O(1) para encolar y desencolar
  - Refleja el comportamiento natural de una fila de espera
  - Evita conflictos cuando múltiples usuarios solicitan el mismo libro

#### **Lista Enlazada Doble para Historial**
- **Operaciones necesarias**: Inserción al final, recorrido hacia adelante/atrás, búsqueda.
- **Justificación**:
  - Crecimiento dinámico sin necesidad de redimensionar
  - Inserción al final O(1) con puntero tail
  - Navegación bidireccional útil para consultas cronológicas
  - Mantiene el orden temporal de las transacciones
  - Permite agregar filtros y búsquedas sin reestructurar

#### **Tabla Hash para Usuarios**
- **Operaciones necesarias**: Inserción, búsqueda por ID, actualización de datos.
- **Justificación**:
  - Acceso instantáneo a información del usuario: O(1)
  - Actualización rápida de libros prestados actualmente
  - Validación eficiente de límite de préstamos por usuario
  - Escalable para miles de usuarios sin degradación de rendimiento

### 5. Pseudocódigo Representativo

```pseudocode
// Estructura de Datos Principal
CLASE BibliotecaDigital:
    arbolLibros: ArbolBST<Libro>
    categoriasIndex: HashMap<String, Lista<Libro>>
    colaSolicitudes: Cola<Solicitud>
    historialPrestamos: ListaEnlazadaDoble<Prestamo>
    usuarios: HashMap<String, Usuario>

// Operación: Registrar un libro
FUNCIÓN registrarLibro(titulo, autor, categoria):
    libro = NUEVO Libro(titulo, autor, categoria, DISPONIBLE)
    arbolLibros.insertar(libro)  // O(log n)
    
    SI NO categoriasIndex.contiene(categoria):
        categoriasIndex[categoria] = NUEVA Lista()
    FIN SI
    
    categoriasIndex[categoria].agregar(libro)  // O(1)
FIN FUNCIÓN

// Operación: Buscar libro por título
FUNCIÓN buscarPorTitulo(titulo):
    RETORNAR arbolLibros.buscar(titulo)  // O(log n)
FIN FUNCIÓN

// Operación: Buscar libros por categoría
FUNCIÓN buscarPorCategoria(categoria):
    SI categoriasIndex.contiene(categoria):
        RETORNAR categoriasIndex[categoria]  // O(1)
    SINO:
        RETORNAR Lista vacía
    FIN SI
FIN FUNCIÓN

// Operación: Solicitar préstamo
FUNCIÓN solicitarPrestamo(idUsuario, tituloLibro):
    usuario = usuarios.obtener(idUsuario)  // O(1)
    libro = buscarPorTitulo(tituloLibro)   // O(log n)
    
    SI libro.estado == DISPONIBLE:
        solicitud = NUEVA Solicitud(usuario, libro, FECHA_ACTUAL)
        colaSolicitudes.encolar(solicitud)  // O(1)
        RETORNAR "Solicitud encolada exitosamente"
    SINO:
        RETORNAR "Libro no disponible"
    FIN SI
FIN FUNCIÓN

// Operación: Procesar siguiente préstamo
FUNCIÓN procesarSiguientePrestamo():
    SI colaSolicitudes.estaVacia():
        RETORNAR "No hay solicitudes pendientes"
    FIN SI
    
    solicitud = colaSolicitudes.desencolar()  // O(1)
    libro = solicitud.libro
    usuario = solicitud.usuario
    
    libro.estado = PRESTADO
    usuario.librosPrestados.agregar(libro)
    
    prestamo = NUEVO Prestamo(usuario, libro, FECHA_ACTUAL)
    historialPrestamos.agregarAlFinal(prestamo)  // O(1)
    
    RETORNAR "Préstamo procesado para " + usuario.nombre
FIN FUNCIÓN

// Operación: Devolver libro
FUNCIÓN devolverLibro(idUsuario, tituloLibro):
    usuario = usuarios.obtener(idUsuario)  // O(1)
    libro = buscarPorTitulo(tituloLibro)   // O(log n)
    
    libro.estado = DISPONIBLE
    usuario.librosPrestados.eliminar(libro)
    
    devolucion = NUEVO Prestamo(usuario, libro, FECHA_ACTUAL, DEVUELTO)
    historialPrestamos.agregarAlFinal(devolucion)  // O(1)
    
    RETORNAR "Libro devuelto exitosamente"
FIN FUNCIÓN

// Operación: Consultar historial
FUNCIÓN consultarHistorial(filtro OPCIONAL):
    RETORNAR historialPrestamos.recorrer(filtro)
FIN FUNCIÓN
```

### 6. Análisis de Complejidad

| Operación | Complejidad Temporal | Justificación |
|-----------|---------------------|---------------|
| Registrar libro | O(log n) | Inserción en BST + O(1) en hash |
| Buscar por título | O(log n) | Búsqueda en BST balanceado |
| Buscar por categoría | O(1) + O(k) | Acceso a hash + recorrer k libros |
| Solicitar préstamo | O(log n) | Búsqueda en BST + encolar O(1) |
| Procesar préstamo | O(1) | Desencolar + insertar en lista |
| Devolver libro | O(log n) | Búsqueda en BST + insertar O(1) |
| Consultar historial | O(h) | Recorrer h elementos del historial |

**Donde:**
- n = número total de libros
- k = número de libros en una categoría específica
- h = número de registros en el historial

### 7. Ventajas de esta Integración

1. **Eficiencia**: Cada estructura está optimizada para las operaciones que realiza.
2. **Escalabilidad**: El sistema puede crecer sin degradación significativa del rendimiento.
3. **Flexibilidad**: Permite búsquedas por múltiples criterios (título, categoría).
4. **Equidad**: La cola FIFO garantiza un sistema justo de préstamos.
5. **Trazabilidad**: El historial completo permite auditorías y estadísticas.
6. **Mantenibilidad**: Cada estructura tiene una responsabilidad clara y definida.

### 8. Consideraciones de Implementación

- **Balanceo del BST**: Se recomienda usar un AVL o Red-Black Tree para garantizar O(log n) en el peor caso.
- **Persistencia**: El historial puede archivarse periódicamente para optimizar memoria.
- **Concurrencia**: Implementar locks para evitar race conditions en solicitudes simultáneas.
- **Índices adicionales**: Se pueden agregar índices por autor o ISBN según necesidad.

---

## Problema 2: Sistema de control de pedidos en una cafetería

Una cafetería necesita un sistema para administrar los pedidos de los clientes. El sistema debe:

- Registrar los pedidos conforme llegan.  
- Permitir consultar el estado de cada pedido (pendiente, en preparación, listo, entregado).  
- Mantener una lista de productos disponibles.  
- Permitir deshacer o rehacer acciones recientes, como cancelar o reactivar pedidos.

### Indicaciones
Propón una solución que integre varias estructuras de datos, por ejemplo:

- Una cola (queue) para el flujo de pedidos pendientes.  
- Una pila (stack) para manejar las acciones de deshacer/rehacer.  
- Una lista o arreglo para el catálogo de productos disponibles.

Explica cómo se relacionan las estructuras entre sí y qué operaciones se realizan sobre cada una.

---

## SOLUCIÓN AL PROBLEMA 2: Sistema de Control de Pedidos en una Cafetería

### 1. Propósito del Sistema

El sistema de control de pedidos tiene como objetivo principal gestionar eficientemente el flujo de pedidos en una cafetería, desde su registro hasta su entrega, manteniendo un control preciso del estado de cada pedido, permitiendo modificaciones controladas mediante un sistema de deshacer/rehacer, y administrando el inventario de productos disponibles. Esto garantiza un servicio ordenado, trazable y con capacidad de corrección de errores humanos.

### 2. Estructuras de Datos Utilizadas

El sistema integra las siguientes estructuras de datos:

#### a) **Cola de Prioridad (Priority Queue)** - Para pedidos pendientes
- **Propósito**: Gestionar los pedidos pendientes con priorización opcional.
- **Tipo**: Cola FIFO con soporte para prioridad (pedidos urgentes, pedidos normales).
- **Ventajas**: Procesamiento ordenado O(log n), permite atender primero pedidos urgentes.

#### b) **Tabla Hash (Diccionario)** - Para consulta rápida de pedidos
- **Propósito**: Acceso inmediato a cualquier pedido por su ID único.
- **Clave**: ID del pedido (número incremental o UUID).
- **Valor**: Objeto Pedido con toda su información (productos, estado, cliente, timestamp).
- **Ventajas**: Búsqueda y actualización O(1), ideal para consultar estado de pedidos.

#### c) **Pila (Stack) para Deshacer** - Para acciones reversibles
- **Propósito**: Almacenar el historial de acciones realizadas para poder deshacerlas.
- **Tipo**: Pila LIFO (Last In, First Out).
- **Ventajas**: Permite deshacer acciones en orden inverso O(1).

#### d) **Pila (Stack) para Rehacer** - Para acciones rehacibles
- **Propósito**: Almacenar acciones que fueron deshechas para poder rehacerlas.
- **Tipo**: Pila LIFO.
- **Ventajas**: Permite rehacer acciones previamente deshechas O(1).

#### e) **Arreglo Dinámico (Lista)** - Para catálogo de productos
- **Propósito**: Mantener la lista de productos disponibles con su información.
- **Contenido**: Objetos Producto (nombre, precio, disponibilidad, categoría).
- **Ventajas**: Acceso directo por índice O(1), fácil iteración para mostrar menú.

#### f) **Lista Enlazada** - Para cola de preparación
- **Propósito**: Gestionar pedidos que están siendo preparados en la cocina.
- **Ventajas**: Inserción/eliminación eficiente O(1), flexibilidad para reordenar.

#### g) **Arreglo Circular (Circular Buffer)** - Para pedidos listos
- **Propósito**: Buffer temporal de pedidos listos para entregar.
- **Tamaño**: Fijo (ej: 20 pedidos), para optimizar memoria.
- **Ventajas**: Operaciones O(1), gestión eficiente de espacio limitado.

### 3. Integración de las Estructuras

Las estructuras de datos interactúan de la siguiente manera:

```
┌──────────────────────────────────────────────────────────────────┐
│              SISTEMA DE CONTROL DE PEDIDOS - CAFETERÍA            │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌─────────────────────┐                                         │
│  │ Arreglo Productos   │                                         │
│  │ [0] Café Express    │                                         │
│  │ [1] Cappuccino      │──────┐                                  │
│  │ [2] Croissant       │      │ Referencia                       │
│  │ [3] Sandwich        │      │                                  │
│  └─────────────────────┘      │                                  │
│                               ▼                                  │
│  ┌────────────────────────────────────────┐                      │
│  │     NUEVO PEDIDO (referencias)         │                      │
│  │  ID: 101                               │                      │
│  │  Productos: [0, 1] (Café + Cappuccino) │                      │
│  │  Estado: PENDIENTE                     │                      │
│  └───────────┬────────────────────────────┘                      │
│              │                                                    │
│              ▼                                                    │
│  ┌──────────────────────┐      ┌─────────────────────┐          │
│  │  Cola Prioridad      │      │  Hash Pedidos       │          │
│  │  (Pendientes)        │      │  (Consulta rápida)  │          │
│  │                      │      │                     │          │
│  │   P101 (urgente)     │◄────►│  101 → Pedido Obj   │          │
│  │   P102 (normal)      │      │  102 → Pedido Obj   │          │
│  │   P103 (normal)      │      │  103 → Pedido Obj   │          │
│  └──────────┬───────────┘      │  104 → Pedido Obj   │          │
│             │                  └─────────────────────┘          │
│             │ Procesar                                           │
│             ▼                                                    │
│  ┌──────────────────────┐                                       │
│  │  Lista Enlazada      │                                       │
│  │  (En Preparación)    │                                       │
│  │  ┌─────┐   ┌─────┐  │                                       │
│  │  │ P101│──►│ P103│  │                                       │
│  │  └─────┘   └─────┘  │                                       │
│  └──────────┬───────────┘                                       │
│             │ Completar                                          │
│             ▼                                                    │
│  ┌──────────────────────┐                                       │
│  │  Buffer Circular     │                                       │
│  │  (Listos)            │                                       │
│  │  ┌───┬───┬───┬───┐  │                                       │
│  │  │101│   │103│   │  │                                       │
│  │  └───┴───┴───┴───┘  │                                       │
│  │   ▲head    ▲tail    │                                       │
│  └──────────┬───────────┘                                       │
│             │ Entregar                                           │
│             ▼                                                    │
│       ESTADO: ENTREGADO                                          │
│                                                                   │
│  ┌──────────────────────────────────────────────────┐           │
│  │        SISTEMA DESHACER/REHACER                   │           │
│  │                                                    │           │
│  │  ┌─────────────────┐      ┌─────────────────┐   │           │
│  │  │ Pila Deshacer   │      │ Pila Rehacer    │   │           │
│  │  │                 │      │                 │   │           │
│  │  │ ┌───────────┐   │      │ ┌───────────┐   │   │           │
│  │  │ │ Cancelar  │   │      │ │           │   │   │           │
│  │  │ │ P104      │   │      │ │           │   │   │           │
│  │  │ ├───────────┤   │      │ └───────────┘   │   │           │
│  │  │ │ Modificar │   │      │                 │   │           │
│  │  │ │ P102      │   │      │                 │   │           │
│  │  │ ├───────────┤   │      │                 │   │           │
│  │  │ │ Crear     │   │      │                 │   │           │
│  │  │ │ P103      │   │      │                 │   │           │
│  │  │ └───────────┘   │      │                 │   │           │
│  │  └─────────────────┘      └─────────────────┘   │           │
│  └──────────────────────────────────────────────────┘           │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

**Flujo de operaciones:**

1. **Registro de pedido**:
   - Se crea un nuevo pedido con ID único
   - Se agregan referencias a productos del catálogo
   - Se inserta en la cola de prioridad (pendientes)
   - Se indexa en la tabla hash para consultas rápidas
   - Se registra la acción en la pila de deshacer

2. **Preparación de pedido**:
   - Se desencola de pendientes (según prioridad)
   - Se actualiza estado a "EN_PREPARACION"
   - Se agrega a la lista enlazada de preparación
   - Se registra la acción en la pila de deshacer

3. **Pedido listo**:
   - Se elimina de la lista de preparación
   - Se actualiza estado a "LISTO"
   - Se inserta en el buffer circular de pedidos listos
   - Se registra la acción en la pila de deshacer

4. **Entrega de pedido**:
   - Se extrae del buffer circular
   - Se actualiza estado a "ENTREGADO"
   - Permanece en la tabla hash para historial
   - Se registra la acción en la pila de deshacer

5. **Deshacer acción**:
   - Se extrae la última acción de la pila de deshacer
   - Se revierte el cambio correspondiente
   - Se mueve la acción a la pila de rehacer
   - Se actualizan las estructuras afectadas

6. **Rehacer acción**:
   - Se extrae la última acción de la pila de rehacer
   - Se vuelve a aplicar el cambio
   - Se mueve la acción a la pila de deshacer

### 4. Justificación de cada Estructura

#### **Cola de Prioridad para Pedidos Pendientes**
- **Operaciones necesarias**: Insertar pedido, extraer siguiente pedido, consultar siguiente.
- **Justificación**:
  - FIFO para pedidos normales, pero permite priorizar pedidos urgentes
  - Inserción O(log n), extracción O(log n)
  - Simula el comportamiento real de una fila con excepciones
  - Implementable con heap binario para eficiencia
  - Permite atender pedidos VIP o para llevar con mayor prioridad

#### **Tabla Hash para Consulta de Pedidos**
- **Operaciones necesarias**: Buscar pedido por ID, actualizar estado, consultar detalles.
- **Justificación**:
  - Acceso instantáneo O(1) a cualquier pedido
  - Permite consultar estado sin recorrer colas
  - Esencial para atención al cliente (¿está listo mi pedido #123?)
  - Mantiene historial completo incluso de pedidos entregados
  - Facilita reportes y estadísticas

#### **Pilas para Deshacer/Rehacer**
- **Operaciones necesarias**: Push (registrar acción), Pop (deshacer/rehacer), Peek (consultar última).
- **Justificación**:
  - LIFO es natural para deshacer: última acción primero
  - Operaciones O(1) para push/pop
  - Manejo de errores humanos (pedido ingresado mal, cancelación accidental)
  - Dos pilas implementan patrón Command completo
  - Mejora experiencia de usuario permitiendo correcciones

#### **Arreglo Dinámico para Catálogo de Productos**
- **Operaciones necesarias**: Listar productos, buscar por índice, actualizar disponibilidad.
- **Justificación**:
  - Acceso directo O(1) por índice
  - El menú cambia poco, no requiere inserciones frecuentes
  - Iteración simple y eficiente para mostrar menú
  - Menor overhead que estructuras más complejas
  - Los pedidos referencian productos por índice (eficiente)

#### **Lista Enlazada para Preparación**
- **Operaciones necesarias**: Agregar pedido, eliminar pedido completado, reordenar.
- **Justificación**:
  - Inserción/eliminación O(1) sin desplazamientos
  - Flexibilidad para reordenar si la cocina lo necesita
  - Tamaño variable según carga de trabajo
  - Permite iterar para mostrar pedidos en preparación
  - No requiere capacidad predefinida

#### **Buffer Circular para Pedidos Listos**
- **Operaciones necesarias**: Agregar pedido listo, retirar para entrega.
- **Justificación**:
  - Espacio limitado refleja realidad (mostrador finito)
  - Operaciones O(1) para insertar/extraer
  - Uso eficiente de memoria con tamaño fijo
  - Evita acumulación infinita de pedidos listos
  - Reutilización de espacio (circular)

### 5. Pseudocódigo Representativo

```pseudocode
// Estructura de Datos Principal
CLASE SistemaCafeteria:
    colaPendientes: ColaPrioridad<Pedido>
    hashPedidos: HashMap<Integer, Pedido>
    listaPreparacion: ListaEnlazada<Pedido>
    bufferListos: BufferCircular<Pedido>
    catalogoProductos: Array<Producto>
    pilaDeshacer: Pila<Accion>
    pilaRehacer: Pila<Accion>
    contadorID: Integer = 1

// Operación: Registrar nuevo pedido
FUNCIÓN registrarPedido(cliente, listaProductos, esUrgente):
    pedido = NUEVO Pedido(contadorID, cliente, listaProductos, PENDIENTE)
    
    // Insertar en cola con prioridad
    SI esUrgente:
        colaPendientes.insertar(pedido, PRIORIDAD_ALTA)
    SINO:
        colaPendientes.insertar(pedido, PRIORIDAD_NORMAL)
    FIN SI
    
    // Indexar para consultas rápidas
    hashPedidos[contadorID] = pedido  // O(1)
    
    // Registrar acción para deshacer
    accion = NUEVA Accion(CREAR_PEDIDO, pedido, null)
    pilaDeshacer.push(accion)  // O(1)
    pilaRehacer.limpiar()  // Limpiar rehacer al hacer nueva acción
    
    contadorID++
    RETORNAR pedido.id
FIN FUNCIÓN

// Operación: Consultar estado de pedido
FUNCIÓN consultarEstado(idPedido):
    SI hashPedidos.contiene(idPedido):
        pedido = hashPedidos[idPedido]  // O(1)
        RETORNAR pedido.estado
    SINO:
        RETORNAR "Pedido no encontrado"
    FIN SI
FIN FUNCIÓN

// Operación: Procesar siguiente pedido pendiente
FUNCIÓN procesarSiguientePedido():
    SI colaPendientes.estaVacia():
        RETORNAR "No hay pedidos pendientes"
    FIN SI
    
    pedido = colaPendientes.extraer()  // O(log n)
    pedido.estado = EN_PREPARACION
    pedido.timestamp_inicio = HORA_ACTUAL()
    
    listaPreparacion.agregarAlFinal(pedido)  // O(1)
    
    // Registrar acción
    accion = NUEVA Accion(INICIAR_PREPARACION, pedido, PENDIENTE)
    pilaDeshacer.push(accion)
    
    RETORNAR "Preparando pedido #" + pedido.id
FIN FUNCIÓN

// Operación: Marcar pedido como listo
FUNCIÓN marcarPedidoListo(idPedido):
    pedido = hashPedidos[idPedido]  // O(1)
    
    SI pedido.estado != EN_PREPARACION:
        RETORNAR "Error: Pedido no está en preparación"
    FIN SI
    
    listaPreparacion.eliminar(pedido)  // O(n) pero n pequeño
    pedido.estado = LISTO
    pedido.timestamp_listo = HORA_ACTUAL()
    
    SI NO bufferListos.estaLleno():
        bufferListos.insertar(pedido)  // O(1)
        
        accion = NUEVA Accion(MARCAR_LISTO, pedido, EN_PREPARACION)
        pilaDeshacer.push(accion)
        
        RETORNAR "Pedido #" + pedido.id + " está listo"
    SINO:
        RETORNAR "Error: Mostrador lleno, esperar"
    FIN SI
FIN FUNCIÓN

// Operación: Entregar pedido
FUNCIÓN entregarPedido():
    SI bufferListos.estaVacio():
        RETORNAR "No hay pedidos listos"
    FIN SI
    
    pedido = bufferListos.extraer()  // O(1)
    pedido.estado = ENTREGADO
    pedido.timestamp_entrega = HORA_ACTUAL()
    
    accion = NUEVA Accion(ENTREGAR_PEDIDO, pedido, LISTO)
    pilaDeshacer.push(accion)
    
    RETORNAR "Pedido #" + pedido.id + " entregado"
FIN FUNCIÓN

// Operación: Cancelar pedido
FUNCIÓN cancelarPedido(idPedido):
    pedido = hashPedidos[idPedido]  // O(1)
    estadoAnterior = pedido.estado
    
    SEGÚN pedido.estado:
        CASO PENDIENTE:
            colaPendientes.eliminar(pedido)
        CASO EN_PREPARACION:
            listaPreparacion.eliminar(pedido)
        CASO LISTO:
            bufferListos.eliminar(pedido)
        CASO ENTREGADO:
            RETORNAR "No se puede cancelar pedido entregado"
    FIN SEGÚN
    
    pedido.estado = CANCELADO
    
    accion = NUEVA Accion(CANCELAR_PEDIDO, pedido, estadoAnterior)
    pilaDeshacer.push(accion)
    
    RETORNAR "Pedido #" + pedido.id + " cancelado"
FIN FUNCIÓN

// Operación: Deshacer última acción
FUNCIÓN deshacer():
    SI pilaDeshacer.estaVacia():
        RETORNAR "No hay acciones para deshacer"
    FIN SI
    
    accion = pilaDeshacer.pop()  // O(1)
    
    SEGÚN accion.tipo:
        CASO CREAR_PEDIDO:
            cancelarPedido(accion.pedido.id)
        CASO CANCELAR_PEDIDO:
            restaurarPedido(accion.pedido, accion.estadoAnterior)
        CASO INICIAR_PREPARACION:
            devolverAPendientes(accion.pedido)
        CASO MARCAR_LISTO:
            devolverAPreparacion(accion.pedido)
        CASO ENTREGAR_PEDIDO:
            devolverAListos(accion.pedido)
    FIN SEGÚN
    
    pilaRehacer.push(accion)  // O(1)
    RETORNAR "Acción deshecha"
FIN FUNCIÓN

// Operación: Rehacer última acción deshecha
FUNCIÓN rehacer():
    SI pilaRehacer.estaVacia():
        RETORNAR "No hay acciones para rehacer"
    FIN SI
    
    accion = pilaRehacer.pop()  // O(1)
    
    // Volver a ejecutar la acción
    reaplicarAccion(accion)
    
    pilaDeshacer.push(accion)  // O(1)
    RETORNAR "Acción rehecha"
FIN FUNCIÓN

// Operación: Listar productos disponibles
FUNCIÓN listarProductos():
    productos_disponibles = []
    
    PARA i DESDE 0 HASTA catalogoProductos.longitud - 1:
        SI catalogoProductos[i].disponible:
            productos_disponibles.agregar(catalogoProductos[i])
        FIN SI
    FIN PARA
    
    RETORNAR productos_disponibles
FIN FUNCIÓN

// Operación: Obtener estadísticas
FUNCIÓN obtenerEstadisticas():
    stats = NUEVO Estadisticas()
    stats.pendientes = colaPendientes.tamaño()
    stats.enPreparacion = listaPreparacion.tamaño()
    stats.listos = bufferListos.tamaño()
    stats.totalProcesados = contadorID - 1
    
    RETORNAR stats
FIN FUNCIÓN
```

### 6. Análisis de Complejidad

| Operación | Complejidad Temporal | Justificación |
|-----------|---------------------|---------------|
| Registrar pedido | O(log n) | Inserción en cola prioridad + O(1) hash |
| Consultar estado | O(1) | Búsqueda directa en tabla hash |
| Procesar siguiente | O(log n) | Extracción de cola prioridad |
| Marcar listo | O(p) | Eliminar de lista prep (p pequeño) |
| Entregar pedido | O(1) | Extracción de buffer circular |
| Cancelar pedido | O(log n) o O(p) | Depende del estado del pedido |
| Deshacer/Rehacer | O(1) a O(log n) | Pop de pila + restaurar estado |
| Listar productos | O(m) | Recorrer catálogo de m productos |
| Estadísticas | O(1) | Acceso directo a contadores |

**Donde:**
- n = número de pedidos pendientes
- p = número de pedidos en preparación (típicamente pequeño)
- m = número de productos en el catálogo

### 7. Ventajas de esta Integración

1. **Eficiencia operativa**: Cada estructura optimiza su función específica.
2. **Gestión de prioridades**: Cola de prioridad permite atender pedidos urgentes.
3. **Consultas rápidas**: Tabla hash permite responder instantáneamente al cliente.
4. **Recuperación de errores**: Sistema deshacer/rehacer reduce errores humanos.
5. **Escalabilidad controlada**: Buffer circular evita sobrecarga del mostrador.
6. **Flexibilidad**: Lista enlazada permite ajustes dinámicos en preparación.
7. **Trazabilidad completa**: Todas las acciones quedan registradas.
8. **Experiencia de usuario**: Respuestas rápidas y capacidad de corrección.

### 8. Casos de Uso Prácticos

#### Caso 1: Cliente pregunta por su pedido
```
Cliente: "¿Está listo mi pedido #127?"
Sistema: consultarEstado(127) → O(1)
Respuesta: "Su pedido está EN_PREPARACION"
```

#### Caso 2: Error en pedido
```
Empleado registra pedido incorrectamente
Empleado: "Deshacer"
Sistema: deshacer() → Cancela último pedido → O(1)
Empleado crea pedido correcto
```

#### Caso 3: Hora pico
```
10 pedidos pendientes (cola normal)
Llega pedido para llevar (urgente)
Sistema: colaPendientes.insertar(pedido, ALTA_PRIORIDAD)
Resultado: Pedido urgente se procesa antes que los 10 normales
```

### 9. Consideraciones de Implementación

- **Tamaño del buffer**: Ajustar según espacio físico del mostrador (típicamente 15-20 pedidos).
- **Prioridades**: Definir niveles claros (urgente, normal, para llevar).
- **Límite de deshacer**: Limitar pila de deshacer a últimas 50 acciones para optimizar memoria.
- **Persistencia**: Guardar pedidos entregados en base de datos para historial.
- **Notificaciones**: Integrar sistema de notificaciones cuando pedido cambia a "LISTO".
- **Validaciones**: Verificar disponibilidad de productos antes de registrar pedido.
- **Métricas**: Calcular tiempo promedio de preparación para optimización.
- **Interfaz**: Pantallas separadas para: registro, cocina, entrega, consulta.

### 10. Diagrama de Estados de un Pedido

```
┌───────────┐
│ PENDIENTE │
└─────┬─────┘
      │ procesarSiguiente()
      ▼
┌─────────────────┐
│ EN_PREPARACION  │
└─────┬───────────┘
      │ marcarListo()
      ▼
┌───────────┐         ┌────────────┐
│   LISTO   │────────►│ ENTREGADO  │
└─────┬─────┘         └────────────┘
      │ entregarPedido()
      │
      │ cancelarPedido() (desde cualquier estado)
      ▼
┌───────────┐
│ CANCELADO │
└───────────┘

* Las flechas pueden revertirse con deshacer()
```

### 11. Extensiones Posibles

1. **Cola de prioridad múltiple**: Separar bebidas calientes, frías y alimentos.
2. **Índice por cliente**: Tabla hash adicional para buscar todos los pedidos de un cliente.
3. **Árbol de decisión**: Para sugerencias de productos basadas en historial.
4. **Grafo de dependencias**: Para productos que requieren preparación en orden específico.
5. **Sistema de notificaciones push**: Cola de mensajes para alertas a clientes.

---