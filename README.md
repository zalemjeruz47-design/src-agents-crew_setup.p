# ComisariaBot Marketplace API — Starter

API inicial con FastAPI + PostgreSQL para marketplace multivendedor.

## Arranque local con Docker
1. Copia `.env.example` a `.env`.
2. Cambia ambos secretos por valores largos y aleatorios.
3. Ejecuta `docker compose up --build`.
4. Abre `http://localhost:8000/docs` para explorar la API.
5. Comprueba `http://localhost:8000/health`.

## Endpoints iniciales
- `POST /auth/register` — alta de comprador.
- `POST /auth/login` — obtiene token JWT.
- `GET /products` — catálogo público.
- `GET /stores` — tiendas activas.
- `POST /stores` — crear tienda (rol seller).
- `POST /products` — publicar producto (seller con tienda activa).
- `POST /orders` — crea orden principal y subórdenes por tienda, bloquea filas de inventario y calcula comisión dentro de una transacción.
- `GET /seller/orders` — pedidos del vendedor autenticado.
- `GET /admin/overview` — resumen para administrador.

## Importante: estado del starter
Es una base de desarrollo, no un sistema listo para producción. La creación automática de tablas está habilitada para facilitar pruebas; debe sustituirse por migraciones Alembic antes de producción. No hay aún alta/aprobación segura de vendedores, panel para asignar roles, recuperación de contraseña, verificación de correo, pasarela de pagos, webhooks, envíos, reembolsos, rate limiting, auditoría completa ni pruebas automatizadas.

El registro público solo crea usuarios con rol `buyer`. La asignación de roles `seller` y `admin` debe hacerse mediante un flujo administrativo seguro; nunca se debe aceptar el rol enviado libremente por el cliente.

## Decisiones de arquitectura
- PostgreSQL como fuente de verdad.
- FastAPI como API HTTP.
- JWT de corta duración para autenticación inicial.
- Hash de contraseñas con Argon2.
- Una orden general por compra y una suborden por vendedor.
- Inventario reservado mediante `SELECT ... FOR UPDATE`.
- Precios, importes y comisión recalculados en servidor; nunca confiar en totales enviados por frontend.
- Comisión guardada como snapshot en cada línea de pedido.

## Próximas tareas
1. Migraciones Alembic y modelo de roles/permisos robusto.
2. Endpoints administrativos para aprobar vendedores y tiendas.
3. Direcciones, impuestos, tarifas de envío y políticas de cancelación.
4. Integración de pasarela de pago y webhooks idempotentes.
5. Liquidaciones, conciliación y bitácora de movimientos.
6. Pruebas de concurrencia, seguridad y despliegue.
