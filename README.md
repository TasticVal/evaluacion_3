1. Monitoreo y Observabilidad (IE1)
Implementación: Se utiliza AWS CloudWatch Logs para la centralización de logs y Metrics Server en el clúster.

Visualización: El comando kubectl rollout status garantiza que el microservicio esté disponible. Si este comando llega a falla (debido a errores o indisponibilidad), el pipeline se detiene automáticamente, notificando el error en tiempo real.

2. Entorno Orquestado (IE2)
Implementación: La aplicación está desplegada en Amazon EKS.

Gestión: Se utilizan manifiestos YAML declarativos que definen Deployments y Services, permitiendo que Kubernetes gestione automáticamente el ciclo de vida, la escalabilidad y la recuperación de los pods.

3. Dashboard de Métricas (IE3)
Implementación: Se integran métricas clave mediante  AWS:

Uso de CPU/Memoria: Visualizado a través de kubectl top pods y kubectl top nodes (gracias a Metrics Server).

Tiempo de Despliegue y Errores: Registrados automáticamente por GitHub Actions en cada ejecución del pipeline y validados mediante los estados de rollout.

4. Políticas de Cumplimiento y Auditoría (IE5)
Implementación:

Auditoría de Código/IaC: Uso de Snyk en el pipeline para analizar los manifiestos de Kubernetes y dependencias antes de cada despliegue.

Branch Protection: Configuración en GitHub para impedir el merge a main si el pipeline de CI/CD no finaliza correctamente, garantizando que solo código validado llegue a producción.

5. Integración CI/CD y Toma de Decisiones (IE4)
Flujo: El pipeline automatiza desde el build hasta el despliegue (kubectl apply).

Toma de decisiones: La automatización permite decidir instantáneamente si un cambio es viable. Si el pipeline falla, el despliegue no ocurre, manteniendo la integridad del entorno EKS sin intervención manual.

6. Falla Crítica y Detención del Pipeline (IE6)
Seguridad: El paso de Snyk está configurado con continue-on-error: false. Si se detecta una vulnerabilidad con severidad high, el proceso se detiene inmediatamente.

Calidad: El pipeline utiliza rollout status con un tiempo de espera (timeout=60s). Si los nuevos pods no logran estar en estado Running en ese tiempo, el pipeline devuelve un código de error (exit code 1) y detiene el despliegue, protegiendo al sistema de una actualización fallida.

Reflexión de Valeria:
"Personalmente, fue un proceso de mucho aprendizaje. Lo que más me llevo tiempo fue a no frustrarme con la configuración del cluster, sobre todo cuando el pipeline daba error, Fue un desafío constante entender el proceso completo y como se despliega en AWS"

Reflexión de Kevin:
"Hubo momentos donde los errores de despliegue nos tuvieron bloqueados, pero el hecho de investigar e intentar nuevamente me dio una seguridad técnica que no tenía antes. Al final, nos quedamos con una infraestructura funcional y, sobre todo, con la capacidad de diagnosticar y arreglar fallos en un entorno real."
