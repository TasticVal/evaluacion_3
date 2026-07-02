# Documentación del Pipeline CI/CD - Proyecto DevOps

Este repositorio cuenta con un pipeline de CI/CD robusto implementado en GitHub Actions, diseñado para garantizar la calidad, seguridad y trazabilidad en el despliegue de nuestros microservicios.

## Garantía de Calidad y Seguridad


* **Construcción Automatizada (IE1):** Cada *push* a la rama `main` dispara automáticamente la construcción de imágenes Docker para todos los componentes (microservicios y frontend), las cuales son etiquetadas y almacenadas en Amazon ECR.
* **Pruebas Unitarias (IE2):** El pipeline integra la ejecución automática de pruebas unitarias (JUnit). Si el código no cumple con los criterios de calidad mínimos, el pipeline se bloquea, impidiendo cualquier despliegue de código defectuoso.
* **Escaneo de Vulnerabilidades (IE3):** Integramos **Snyk** en el pipeline. Realizamos un análisis estático de dependencias (`--all-projects`) con un umbral de severidad alta. Si se detecta alguna vulnerabilidad crítica, el despliegue es bloqueado automáticamente (`continue-on-error: false`), notificando de inmediato al equipo.
* **Despliegue y Orquestación (IE4/IE5):** Utilizamos **AWS SSM** para gestionar el despliegue remoto en nuestra instancia EC2. La orquestación se realiza mediante **Docker Compose**, asegurando un entorno de producción consistente.

## Trazabilidad
Para asegurar la transparencia en todo el proceso:
1.  **Historial de Cambios:** Cada despliegue está vinculado a un *commit* específico de GitHub, lo que permite identificar exactamente qué cambios están corriendo en producción.
2.  **Monitoreo de Dependencias:** Implementamos **Dependabot** para monitorear semanalmente nuestras librerías y componentes, permitiendo gestionar actualizaciones de seguridad antes de que se conviertan en riesgos.
3.  **Registro de Eventos:** El historial de ejecuciones de GitHub Actions proporciona una auditoría completa de todas las construcciones, pruebas y despliegues realizados.
