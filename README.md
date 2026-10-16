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
