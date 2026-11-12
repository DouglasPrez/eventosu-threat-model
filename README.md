# EventosU - Modelado de amenazas

## Integrantes
- Douglas Daniel Pérez Hernández
- Carlos Daniel Martínez García

## Alcance
Analizamos el diseño de EventosU: React servido desde un CDN, API Node.js con JWT, PostgreSQL, bucket, app de puerta, panel /admin e integraciones con Microsoft Entra ID, pagos y correo. Incluimos registro, recuperación, inscripciones, pagos, entradas QR, afiches, exportaciones CSV y auditoría.

Quedan fuera los sistemas internos de proveedores, datos de tarjetas y controles físicos ajenos a la aplicación. Es un ejercicio de diseño: los controles y pruebas están propuestos, no implementados. Los supuestos completos están en el informe.

## Contenido del trabajo

En el [informe](docs/informe.pdf) desarrollamos el alcance, los diagramas, las amenazas, el árbol de ataque y la priorización de riesgos. La [tabla de amenazas](docs/amenazas.csv) contiene el detalle de cada escenario y su mitigación. En la [revisión](docs/revision.md) documentamos los ajustes y las comprobaciones que realizamos, y en la [lista de revisión](docs/PENDIENTES.md) registramos el estado de la entrega.

## Estructura
```text
eventosu-threat-model/
├── README.md
├── model/eventosu.json
└── docs/
    ├── dfd-nivel-0.png
    ├── dfd-nivel-1.png
    ├── captura-threat-dragon.png
    ├── informe.pdf
    ├── amenazas.csv
    ├── validacion-modelo-final.png
    ├── revision.md
    └── PENDIENTES.md
```

## Cómo abrir el modelo
1. Abrir [OWASP Threat Dragon](https://www.threatdragon.com/) y elegir sesión local.
2. Elegir abrir/importar un modelo existente.
3. Pegar el contenido completo de `model/eventosu.json` y pulsar Importar. En escritorio también puede abrirse el archivo directamente.
4. Abrir los diagramas `Nivel 0 - Contexto` y `Nivel 1 - Componentes y límites`.
5. Seleccionar un elemento o flujo para consultar sus amenazas. Ajustar el zoom cuando sea necesario.

Abrimos el modelo en Threat Dragon 2.6.2 el 12 de noviembre de 2026. Incluye 24 amenazas (cuatro por categoría STRIDE), 22 flujos y seis zonas de confianza. `Open` indica un control pendiente; `Accepted` corresponde a la aceptación de I04 propuesta para el ejercicio. Las pruebas de seguridad quedan definidas para cuando implementemos el sistema.

## Diagramas
### Nivel 0
![DFD de contexto](docs/dfd-nivel-0.png)

### Nivel 1
![DFD de componentes y límites](docs/dfd-nivel-1.png)

Abrir las imágenes originales y ampliarlas para leer los flujos.

## Referencias
- [OWASP Threat Dragon](https://www.threatdragon.com/)
- [OWASP ASVS 4.0.3: control de acceso](https://github.com/OWASP/ASVS/blob/v4.0.3/4.0/en/0x12-V4-Access-Control.md)
- [OWASP ASVS 4.0.3: lógica de negocio](https://github.com/OWASP/ASVS/blob/v4.0.3/4.0/en/0x19-V11-BusLogic.md)

Usamos ASVS 4.0.3 como referencia para mantener los mismos identificadores en todo el trabajo.


## Revisión final

Comprobamos la consistencia de las 24 amenazas entre el modelo, el CSV y el informe. Obtuvimos cinco riesgos críticos, trece altos y seis medios. Corregimos la asociación de D04 a F12 y justificamos la aceptación de I04 con su responsable y plazo de revisión.

También abrimos el modelo final en Threat Dragon 2.6.2 y guardamos una [captura de la validación](docs/validacion-modelo-final.png). El historial conserva nuestros aportes y el [pull request de revisión](https://github.com/DouglasPrez/eventosu-threat-model/pull/1).
