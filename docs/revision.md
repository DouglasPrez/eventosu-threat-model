# Revisión del modelo de amenazas de EventosU

Contrastamos el modelo, los diagramas, el informe y la tabla de amenazas para comprobar que la entrega fuera consistente. Como EventosU está en fase de diseño, las pruebas que definimos son criterios de aceptación para su implementación.

## Modelo y priorización

El modelo contiene dos diagramas, 22 flujos y seis límites de confianza. Identificamos 24 amenazas, cuatro por cada categoría STRIDE. Comprobamos que sus identificadores, elementos, escenarios, mitigaciones y puntuaciones coincidan en el JSON, el CSV y el informe.

Calculamos el riesgo multiplicando impacto por probabilidad. El resultado es de cinco riesgos críticos de 9, trece altos de 6 y seis medios: cinco de 4 y uno de 3.

Durante la revisión ajustamos dos puntos:

- Asociamos D04 a F12 porque describe la saturación del endpoint de validación de entradas. F11 corresponde a la captura local del QR.
- Cambiamos la probabilidad de I04 de 2 a 3: cualquier visitante puede automatizar solicitudes al catálogo público. Con impacto 1, su riesgo es 3. Aceptamos este riesgo en el modelo porque copiar información destinada a difusión no justifica restringir su consulta. La aceptación no cubre datos privados ni interrupciones del servicio; asignamos su responsabilidad al dueño funcional y su revisión antes de cada evento o en 90 días.

## Trazabilidad y pruebas propuestas

Usamos OWASP ASVS 4.0.3 para relacionar los cinco riesgos críticos con tareas y condiciones verificables.

| Amenaza | Requisito y tarea | Condición de aceptación |
| --- | --- | --- |
| S01, webhook falso | 11.1.5 · SEC-01 | Rechazar firmas ausentes, inválidas o vencidas. Repetir el mismo pago secuencialmente y con 20 solicitudes concurrentes debe producir una sola transición y una sola entrada. |
| T01, reutilización de QR | 11.1.6 · SEC-02 | De 20 validaciones simultáneas del mismo QR, exactamente una debe ser válida. Una entrada impagada o de otro evento no debe aceptarse. |
| T02, manipulación del importe | 11.1.1 y 11.1.5 · SEC-03 | Calcular el total en servidor y rechazar confirmaciones con orden, importe o moneda distintos, aunque estén firmadas. |
| I01, exposición de CSV | 4.2.1 y 4.1.3 · SEC-04 | Impedir la generación y descarga de listas de otro organizador, el acceso anónimo y el uso de enlaces vencidos. Detectar permisos públicos mediante IaC. |
| E01, elevación a administrador | 4.1.1 y 4.3.1 · SEC-05 | Rechazar roles insuficientes y el campo `role=admin` del cliente. Denegar también a un administrador con contraseña válida y MFA pendiente. |

Para T03 añadimos casos con `=`, `+`, `-`, `@` y prefijos de control en los campos del CSV. El visor aprobado debe mostrarlos como texto, sin evaluar fórmulas.

## Árbol de ataque

Representamos tres caminos alternativos mediante una raíz O y condiciones conjuntas mediante nodos Y. En la rama del webhook falso incluimos la emisión de un QR aceptado en puerta. Consideramos que obtener un QR ajeno y llegar primero es el camino de menor esfuerzo cuando no se contrasta identidad; esta comparación depende de conseguir una entrada vigente.

## Comprobación final

Importamos y abrimos la versión corregida en Threat Dragon 2.6.2. Verificamos que las 24 amenazas y sus productos de impacto por probabilidad coincidan entre los archivos. Los aportes y la revisión de ambos integrantes están registrados en el historial del repositorio.

Referencias: [ASVS: control de acceso](https://github.com/OWASP/ASVS/blob/v4.0.3/4.0/en/0x12-V4-Access-Control.md) y [ASVS: lógica de negocio](https://github.com/OWASP/ASVS/blob/v4.0.3/4.0/en/0x19-V11-BusLogic.md).
