# Videojuego-John-Wick-UML
"Proyecto de diagramas UML en PlantUML para videojuego basado en John Wick". :)


## Diagrama de Casos de Uso (UML)

```plantuml
@startuml
left to right direction
skinparam packageStyle rectangle

actor Jugador
actor "Hotel Continental" as Continental
actor "Alta Mesa" as AltaMesa
actor "Servidor de Guardado" as Servidor

rectangle "Sistema: Videojuego John Wick" {
    usecase "Iniciar Sesion" as UC1
    usecase "Seleccionar Mision" as UC2
    usecase "Cargar Escenario" as UC3
    usecase "Comprar Equipo" as UC4
    usecase "Validar Monedas" as UC5
    usecase "Ejecutar Combate" as UC6
    usecase "Realizar Takedown" as UC7
    usecase "Despojar Municion" as UC8
    usecase "Pagar Favor" as UC9
    usecase "Guardar Progreso" as UC10
}

Jugador --> UC1
Jugador --> UC2
Jugador --> UC4
Jugador --> UC6

UC2 .> UC3 : <<include>>
UC4 .> UC5 : <<include>>
UC7 .> UC6 : <<extends>>
UC7 .> UC8 : <<include>>
UC9 .> UC6 : <<extends>>

UC1 --> Servidor
UC10 <-- Servidor
UC4 --> Continental
UC2 --> AltaMesa
UC9 --> AltaMesa
@enduml

-------------------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------------------
## 2. Diagrama de Clases
@startuml
skinparam classAttributeIconSize 0

class Personaje {
  - id: int
  - nombre: string
  - saludMax: int
  - saludActual: int
  - nivel: int
  - monedasOro: int
  - esExcomunicado: boolean
  + mover(): void
  + atacar(): void
  + usarHabilidad(): void
  + equiparArma(arma: Arma): void
  + activarBerserker(): void
  + violarReglaContinental(): void
}

class Arma {
  - nombre: string
  - danoBase: int
  - capacidadCargador: int
  - rango: float
  + disparar(): void
  + recargar(): void
}

class TiendaArmas {
  - idTienda: int
  - nombre: string
  - inventario: List<Arma>
  + mostrarArmas(): List<Arma>
  + venderArma(arma: Arma, personaje: Personaje): bool
}

abstract class BaseOperaciones {
  - idBase: int
  - nombre: string
  - ciudad: string
  - esZonaNeutral: boolean
  + otorgarRefugio(p: Personaje): bool
  + ofrecerServicios(): void
}

class HotelContinental {
  - gerente: string
  - tieneSommelier: boolean
  - tieneSastre: boolean
  + sanarPersonaje(p: Personaje): void
}

class RedBowery {
  - ubicacionSubterranea: string
  - redMensajeria: boolean
  + venderArmasMercadoNegro(): void
}

class RuskaRoma {
  - sindicatoFamilia: string
  - salvoconductoActivo: boolean
  + honrarMarcadorSangre(): void
}

abstract class Habilidad {
  - nombre: string
  - cooldown: float
  - costoEnergia: int
  + activar(): void
  + estaDisponible(): bool
}

class ModoBerserker {
  - multiplicadorDano: float
  - duracionSegundos: float
  - lifestealPorcentaje: float
  + aumentarVelocidad(): void
  + regenerarSalud(): void
}

class AltaMesa {
  - nivelExcomunion: int
  - recompensaCabeza: int
  + asignarContrato(): void
  + declararExcomunicado(p: Personaje): void
  + desplegarCazarrecompensas(): void
}

' Relaciones y Herencia
Personaje "1" o-- "*" Arma : equipa
Personaje "1" *-- "*" Habilidad : posee
Habilidad <|-- ModoBerserker : hereda
TiendaArmas "1" o-- "*" Arma : vende

BaseOperaciones <|-- HotelContinental : hereda
BaseOperaciones <|-- RedBowery : hereda
BaseOperaciones <|-- RuskaRoma : hereda

HotelContinental "1" *-- "1" TiendaArmas : alberga
Personaje "*" -- "1" BaseOperaciones : interactua
AltaMesa ..> Personaje : persigue
@enduml
-----------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------
## 3. Diagrama de Secuencia (Compra en Tienda)
@startuml
autonumber
actor Jugador
participant "TiendaArmas" as Tienda
participant "Inventario" as Inv

Jugador -> Tienda: seleccionarArma(armaId)
Tienda -> Jugador: consultarMonedas()
alt tiene monedas suficientes
    Jugador --> Tienda: descuentoExitoso()
    Tienda -> Inv: agregarArma(armaId)
    Tienda --> Jugador: confirmarCompra()
else no tiene suficientes monedas
    Tienda --> Jugador: rechazarCompra("Fondos insuficientes")
end
@enduml
--------------------------------------------------------------------------------------------

--------------------------------------------------------------------------------------------
## 4. Diagrama de Estados (Ciclo de Vida del Jugador)

@startuml
[*] --> Inactivo

Inactivo --> EnCombate : Detectado por enemigos
EnCombate --> ModoBerserker : Activar habilidad (Energia llena)
ModoBerserker --> EnCombate : Tiempo agotado (cooldown)

EnCombate --> Aturdido : Recibe impacto pesado
Aturdido --> EnCombate : Recuperacion / Takedown

EnCombate --> Derrotado : Salud == 0
Derrotado --> EnCombate : Pagar Favor (Marcador)
Derrotado --> [*] : Game Over

EnCombate --> Excomunicado : Rompe regla de Alta Mesa
Excomunicado --> EnCombate : Sobrevivir a oleada
@enduml
------------------------------------------------------------------------------------------------

------------------------------------------------------------------------------------------------
## 5. Diagrama de Actividades (Algoritmo de Combate y Takedown)

@startuml
start
:Jugador presiona boton de ataque;
if (¿Enemigo esta a corta distancia?) then (si)
    if (¿Enemigo desarmado o aturdido?) then (si)
        :Ejecutar Takedown (Invulnerabilidad temporal);
        :Despojar municion / arma;
        :Regenerar proporcion de salud;
    else (no)
        :Realizar golpe melee basico;
    endif
else (no)
    :Disparar arma equipada;
    :Reducir contador de cargador;
endif

if (¿Salud del enemigo == 0?) then (si)
    :Eliminar enemigo;
    :Sumar monedas de oro;
endif
stop
@enduml
-------------------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------------------
## Descripciones

### CU-01: Iniciar Sesión
| Campo | Descripción |
| :--- | :--- |
| **Actor Principal** | Jugador |
| **Actores Secundarios** | Servidor de Guardado |
| **Precondición** | El jugador debe tener conexión a la red y credenciales registradas. |
| **Flujo Principal** | 1. El jugador ingresa usuario y contraseña.<br>2. El sistema valida las credenciales con el **Servidor de Guardado**.<br>3. Se habilita el menú principal del juego. |
| **Postcondición** | Sesión autenticada para el almacenamiento de datos. |

---

### CU-02: Seleccionar Misión
| Campo | Descripción |
| :--- | :--- |
| **Actor Principal** | Jugador |
| **Actores Secundarios** | Alta Mesa |
| **Precondición** | Haber iniciado sesión exitosamente (`CU-01`). |
| **Flujo Principal** | 1. El jugador accede al mapa de contratos.<br>2. El sistema consulta las misiones activas enviadas por la **Alta Mesa**.<br>3. El jugador selecciona un contrato.<br>4. El sistema invoca `CU-03 Cargar Escenario` (`<<include>>`)[cite: 1]. |
| **Postcondición** | El mapa y la dificultad quedan definidos para la partida. |

---

### CU-03: Cargar Escenario
| Campo | Descripción |
| :--- | :--- |
| **Actor Secundario** | Servidor / Sistema |
| **Precondición** | Se ha seleccionado una misión (`CU-02`). |
| **Flujo Principal** | 1. Carga los assets gráficos, mapa y puntos de regeneración.<br>2. Genera los patrones de patrulla de los enemigos. |
| **Postcondición** | El mapa queda renderizado y listo para iniciar el combate. |

---

### CU-04: Comprar Equipo
| Campo | Descripción |
| :--- | :--- |
| **Actor Principal** | Jugador |
| **Actores Secundarios** | Hotel Continental (TiendaArmas) |
| **Precondición** | El jugador se encuentra en una zona neutral (Hotel Continental). |
| **Flujo Principal** | 1. El jugador selecciona un arma en el armero/sommelier.<br>2. El sistema ejecuta `CU-05 Validar Monedas` (`<<include>>`)[cite: 1].<br>3. Se descuenta el precio y el arma pasa al inventario. |
| **Flujo Alternativo** | 2a. Si no hay Monedas de Oro suficientes, el sistema cancela la compra y emite un mensaje de error. |
| **Postcondición** | El inventario del jugador es actualizado. |

---

### CU-05: Validar Monedas
| Campo | Descripción |
| :--- | :--- |
| **Actor Secundario** | Hotel Continental / Sistema |
| **Precondición** | Intento de compra en `CU-04`. |
| **Flujo Principal** | Compara el saldo de Monedas de Oro del `Personaje` con el costo del ítem seleccionado. |
| **Postcondición** | Retorna un valor booleano (`true`/`false`) que autoriza o deniega la transacción. |

---

### CU-06: Ejecutar Combate
| Campo | Descripción |
| :--- | :--- |
| **Actor Principal** | Jugador |
| **Precondición** | El personaje se encuentra en un escenario de misión activo. |
| **Flujo Principal** | 1. El jugador se desplaza y utiliza armas/habilidades contra enemigos.<br>2. El sistema calcula daño, precisión y munición consumida. |
| **Postcondición** | Modifica la vida de los enemigos y la salud del jugador. |

---

### CU-07: Realizar Takedown
| Campo | Descripción |
| :--- | :--- |
| **Actor Principal** | Jugador |
| **Precondición** | Estar en ejecución de combate (`CU-06`) con un enemigo aturdido o desarmado a corta distancia. |
| **Flujo Principal** | 1. El jugador presiona el botón de remate melee.<br>2. El sistema otorga invulnerabilidad temporal (I-Frames).<br>3. Ejecuta `CU-08 Despojar Munición` (`<<include>>`)[cite: 1].<br>4. Elimina al enemigo con una animación especial Gun-Fu. |
| **Relación** | Extiende el flujo de combate normal (`<<extends>>` `CU-06`)[cite: 1]. |
| **Postcondición** | Recupera recursos y remata al objetivo sin sufrir daño durante la animación. |

---

### CU-08: Despojar Munición
| Campo | Descripción |
| :--- | :--- |
| **Actor Secundario** | Sistema |
| **Precondición** | Ejecución en curso de `CU-07 Realizar Takedown`. |
| **Flujo Principal** | Extrae automáticamente el cargador del enemigo ejecutado y lo añade al contador del jugador. |
| **Postcondición** | Aumenta el parque de munición del arma equipada. |

---

### CU-09: Pagar Favor
| Campo | Descripción |
| :--- | :--- |
| **Actor Principal** | Jugador |
| **Actores Secundarios** | Alta Mesa / Ruska Roma |
| **Precondición** | La salud del personaje llega a 0 dentro de `CU-06`. |
| **Flujo Principal** | 1. El jugador entrega un Marcador de Sangre o sello a la **Alta Mesa**.<br>2. Se evita el *Game Over* y se reaparece en un punto seguro con salud parcial. |
| **Relación** | Extiende de forma condicional el evento de muerte en combate (`<<extends>>` `CU-06`)[cite: 1]. |
| **Postcondición** | Se consume un Marcador del inventario del personaje. |

---

### CU-10: Guardar Progreso
| Campo | Descripción |
| :--- | :--- |
| **Actores Secundarios** | Servidor de Guardado |
| **Precondición** | Finalización de misión, compra de equipo o salida del juego. |
| **Flujo Principal** | Guarda de manera asíncrona la salud, inventario, monedas de oro y misiones completadas en la nube. |
| **Postcondición** | La base de datos queda sincronizada con el estado actual del juego. |
