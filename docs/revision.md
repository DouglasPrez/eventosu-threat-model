# Revisión del modelo de amenazas de EventosU

Aporte de Carlos Daniel Martínez García. La revisión se hizo sobre el modelo del commit `18e92f1`, el informe de 18 páginas y el CSV. Se revisó el diseño y las pruebas propuestas.

## Modelo, diagramas y puntuaciones

- El JSON contiene dos diagramas. El de componentes contiene 22 flujos y seis límites de confianza. Los diagramas del PDF representan el contexto y los componentes del modelo.
- Las 24 amenazas coinciden en ID, elemento asociado, categoría STRIDE, escenario, justificación, mitigación, estado y puntuación. En T01, T03 y R03 el título del JSON es más corto y la descripción contiene el escenario completo.
- Los 24 productos de impacto por probabilidad son correctos. Hay cinco riesgos críticos de 9, trece altos de 6, cinco medios de 4 y uno bajo de 2. Cada fila justifica ambos factores. I04 cubre la copia de información pública y su aceptación sigue sujeta a ratificación.
- **D04** está vinculado a F11 en los tres archivos. F11 cubre la captura local. El escenario y sus controles describen saturación del endpoint de la API en F12. Propongo acordar con Douglas la reasignación a F12 y actualizar JSON, CSV e informe juntos. La valoración 3 × 2 sigue siendo razonable por el efecto en puerta y el acceso necesario para intentar saturar el servicio.
- **T03** cubre fórmulas en CSV. I01 controla quién puede obtener el archivo. Conviene añadir a T03 una prueba que exporte campos que empiecen con `=`, `+`, `-` y `@` y confirme que el visor los abre como texto.

## Trazabilidad de los riesgos críticos

Los requisitos se contrastaron con OWASP ASVS 4.0.3. Estas condiciones corresponden a pruebas propuestas.

| Amenaza | ASVS y tarea | Revisión de la aceptación |
| --- | --- | --- |
| S01, webhook falso | 11.1.5 · SEC-01 | Rechazar firmas ausentes, inválidas o vencidas sin mutar el pago. Repetir el mismo ID debe producir una sola entrada. Añadir la repetición concurrente para verificar la deduplicación. |
| T01, reutilización de QR | 11.1.6 · SEC-02 | La prueba de 20 solicitudes en paralelo exige exactamente un éxito. Los casos impagado y de otro evento deben producir cero éxitos. |
| T02, manipulación del importe | 11.1.1 y 11.1.5 · SEC-03 | El total se calcula en servidor. Un webhook firmado con orden, importe o moneda distintos debe rechazarse sin confirmar el pago ni emitir QR. |
| I01, exposición de CSV | 4.2.1 y 4.1.3 · SEC-04 | Probar generación y descarga cruzadas entre organizadores, acceso anónimo y vencimiento del enlace. La prueba de IaC debe detectar lectura o listado público. |
| E01, elevación a administrador | 4.1.1 y 4.3.1 · SEC-05 | Probar que el servidor rechace roles insuficientes y el atributo `role=admin` enviado por el cliente. Añadir un caso de administrador autenticado con MFA pendiente para aislar ese control. |

Fuentes: [ASVS V4, control de acceso](https://github.com/OWASP/ASVS/blob/v4.0.3/4.0/en/0x12-V4-Access-Control.md) y [ASVS V11, lógica de negocio](https://github.com/OWASP/ASVS/blob/v4.0.3/4.0/en/0x19-V11-BusLogic.md).

## Árbol de ataque

La raíz O representa tres caminos alternativos. Cada rama Y exige sus condiciones. La rama C es la de menor esfuerzo si se consigue un QR vigente, se llega primero y la puerta lo acepta como credencial al portador. La comparación es cualitativa y depende del acceso al QR. Para la rama A conviene explicitar como condición final que el pago falsificado provoque la emisión de un QR que la puerta acepte.

El aporte incorpora el informe, el CSV y esta revisión. La reasignación de D04 y las ampliaciones de pruebas quedan propuestas para revisión conjunta. La comprobación del modelo se hizo sobre su JSON. Quedan por comprobar su apertura en Threat Dragon y las pruebas de seguridad cuando exista una implementación.
