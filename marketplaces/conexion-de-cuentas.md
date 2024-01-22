# Conexión de cuentas

Antes de poder conectar un seller con un Marketplace se requiere la creación de una aplicación Marketplace en Zippin, la cual es creada durante el [setup inicial del marketplace](zippin-para-marketplaces.md#alta-de-marketplace).

Una vez que se dispone de la aplicación, el Marketplace podrá permitir a los sellers vincular sus cuentas Zippin con la cuenta Zippin del Marketplace, a través de un flujo OAuth2 que, en forma combinada, autoriza a la aplicación a acceder a la cuenta del seller y genera un vínculo entre Seller y Marketplace.

## Autorización via OAuth2

### Requerimientos

* Haber obtenido el alta del Marketplace en Zippin
* Disponer de las credenciales de la aplicación creada por Zippin para el Marketplace.

### Procedimiento

Sigue los pasos indicados en nuestra [guía sobre la autorización y autenticación basada en oAuth2](../api-docs/autorizacion-con-oauth.md).
