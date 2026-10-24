# Modelo de Dominio: Tienda en Línea

## Entidades
- **Cliente:** persona que compra.
- **Producto:** artículo que se vende (nombre, precio, stock).
- **Pedido:** compra realizada por un cliente.
- **LineaPedido:** un producto dentro de un pedido, con su cantidad.
- **Pago:** cobro asociado a un pedido.

## Relaciones
- Un Cliente realiza muchos Pedidos.
- Un Pedido contiene una o más LineasPedido.
- Cada LineaPedido referencia un Producto.
- Un Pedido se paga con un Pago.

## Reglas de negocio
- Un pedido solo se paga si hay stock suficiente.
- La cantidad de cada línea debe ser mayor que 0.
- El total del pedido es la suma de (precio unitario × cantidad) de sus líneas.
- Al aprobarse el pago, se descuenta el stock.

## Diagrama de clases

```mermaid
classDiagram
    class Cliente {
        -int id
        -string nombre
        -string email
        +hacerPedido()
    }
    class Pedido {
        -int id
        -date fecha
        -double total
        +calcularTotal()
    }
    class LineaPedido {
        -int cantidad
        -double precioUnitario
        +calcularSubtotal()
    }
    class Producto {
        -int id
        -string nombre
        -double precio
        -int stock
        +hayStock(cantidad)
    }
    class Pago {
        -int id
        -double monto
        -string estado
        +procesar()
    }

    Cliente "1" --> "0..*" Pedido : realiza
    Pedido "1" *-- "1..*" LineaPedido : contiene
    LineaPedido "*" --> "1" Producto : referencia
    Pedido "1" --> "0..1" Pago : se paga con
```

## Diagrama de actividades (proceso de compra)

```mermaid
flowchart TD
    A([Inicio]) --> B[Cliente agrega productos al carrito]
    B --> C[Cliente confirma el pedido]
    C --> D{"¿Hay stock suficiente?"}
    D -- No --> E[Notificar falta de stock]
    E --> Z([Fin])
    D -- Sí --> F[Registrar pedido]
    F --> G[Cliente realiza el pago]
    G --> H{"¿Pago aprobado?"}
    H -- No --> I[Cancelar pedido]
    I --> Z
    H -- Sí --> J[Descontar stock y confirmar pedido]
    J --> Z
```
