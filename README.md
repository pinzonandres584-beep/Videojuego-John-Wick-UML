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


## Diagrama de Clases (UML)

```plantuml
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


