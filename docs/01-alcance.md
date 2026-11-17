# Documento de alcance — Portfolio personal de Uriel Barrios

| Campo | Valor |
|---|---|
| Proyecto | Portfolio personal y marca profesional |
| Dominio | `urielivanbarrios.dev` (registrado el 16/11/2026 en Cloudflare) |
| Responsable | Uriel Ivan Barrios |
| Versión del documento | 0.3 |
| Fecha | 16/11/2026 |
| Estado | En discusión |

### Historial de cambios

| Versión | Fecha | Cambio |
|---|---|---|
| 0.1 | 15/11/2026 | Borrador inicial |
| 0.2 | 16/11/2026 | Backend propio con login y panel de administración; stack; subdominios; seguridad; un solo dominio con sección de servicios; objetivo de aprendizaje |
| 0.3 | 16/11/2026 | El proyecto de aprendizaje pasa a ser el **tracker de postulaciones** (proyecto aparte). El portfolio vuelve a ser estático, con el contenido en archivos. El panel de administración propio se posterga y solo se retoma si el volumen de contenido lo justifica. La demo del club pasa a la v1. Se suma el tracker al portfolio y al mapa de subdominios |

---

## 1. Propósito del documento

Este documento define qué es el proyecto, para quién es, qué incluye y qué no, y en qué etapas se construye. Es el punto de partida para los documentos siguientes: requerimientos, casos de uso, historias de usuario, arquitectura y backlog.

## 2. Contexto

Soy Analista de Sistemas (ORT, 90% aprobado) y tengo experiencia en análisis funcional, implementación, testing funcional y soporte. Además curso la Licenciatura en Ciberdefensa (FADENA). Hoy busco trabajo y no tengo un lugar propio donde mostrar lo que hago y cómo lo pienso. El CV y LinkedIn cuentan *qué* hice, pero no muestran *cómo* trabajo.

El portfolio resuelve eso. Es una **vidriera**: muestra los casos y da acceso a las demos de mis proyectos. La lógica de negocio vive en cada proyecto, no en el portfolio. Lo planifico, lo documento y lo gestiono como un producto, y esa forma de trabajar es parte de lo que muestra.

El proyecto donde escribo el código a mano, como práctica de desarrollo, es el **tracker de postulaciones**. Tiene su propio documento de relevamiento.

## 3. Objetivos

| # | Objetivo | Tipo |
|---|---|---|
| O1 | Que un reclutador entienda en menos de 30 segundos qué perfil tengo y qué puedo aportar | Laboral |
| O2 | Que un entrevistador técnico pueda verificar mi trabajo: demos funcionando, repos y documentación | Laboral |
| O3 | Mostrar mi forma de trabajar: relevamiento, documentación, gestión del backlog y testing | Laboral / marca |
| O4 | Tener un lugar donde publicar demos de mis aplicaciones en subdominios | Técnico |
| O5 | Registrar mi avance en ciberseguridad (TryHackMe, CCNA, labs) de forma visible y sostenida | Marca / formación |
| O6 | Habilitar un canal para recibir propuestas de trabajo freelance | Comercial (secundario) |
| O7 | Construir una marca profesional propia y sostenerla en el tiempo | Marca |

**Posicionamiento.** El perfil principal es *Analista de Sistemas · análisis funcional, implementación y soporte de aplicaciones*. La ciberseguridad aparece como formación, en una sección aparte. El desarrollo aparece como capacidad demostrada en los proyectos, no en el titular.

## 4. Usuarios (stakeholders)

| Usuario | Quién es | Qué necesita | Tiempo que dedica | Prioridad |
|---|---|---|---|---|
| **Reclutador no técnico** | RR. HH., consultora, búsqueda por LinkedIn | Perfil claro, experiencia, CV descargable, contacto | 30 s – 2 min | Alta |
| **Reclutador o entrevistador técnico** | Líder funcional, líder técnico, PM | Casos concretos, demos funcionando, repos, documentación, criterio | 5 – 20 min | Alta |
| **Cliente freelance** | Pyme, club, comercio, conocido de un conocido | Saber qué problemas resuelvo, ver ejemplos, contactarme fácilmente | 2 – 5 min | Media |
| **Comunidad de ciberseguridad** | Pares, gente de Ekoparty, docentes | Writeups, registro de práctica, aprendizajes | Variable | Baja (crece con el tiempo) |
| **Administrador (yo)** | Mantengo el sitio | Cargar contenido sin tocar el HTML, con un costo mínimo | — | Alta |

> Nota: en esta etapa, el *Administrador* carga el contenido editando archivos de datos (`.json` / `.md`) separados del diseño. El panel de administración se evalúa más adelante (ver 5.4).

## 5. Arquitectura

### 5.1 Decisión: ¿hace falta un servidor?

**Conclusión: el portfolio no lleva servidor propio. Cada proyecto con demo tiene su propio backend.**

| Componente | ¿Necesita servidor? | Por qué | Solución prevista |
|---|---|---|---|
| Sitio público (perfil, casos, experiencia, labs) | **No** | Todos los visitantes ven el mismo contenido | Sitio estático, con el contenido en archivos de datos |
| Selección de idioma | **No** | El navegador ya informa el idioma preferido | Lógica en el navegador más selector manual |
| Formulario de contacto | **Mínimo** | Hay que recibir y reenviar un mensaje | Función serverless o servicio de formularios, sin servidor propio |
| **Demo del club** | **Sí** | API, base de datos, login y reglas de negocio | Backend Spring Boot + PostgreSQL alojados aparte |
| **Demo del tracker** | **Sí** | Igual que el club | Su propio backend y base, en su subdominio |

### 5.2 Mapa de subdominios

| Subdominio | Qué es | Acceso |
|---|---|---|
| `dominio` (raíz) | Portfolio | Público |
| `club.` | Demo del sistema de gestión del club | Público, con usuario de prueba |
| `tracker.` | Demo del tracker de postulaciones | Público, con usuario de prueba |
| `labs.` o `sec.` | Laboratorios y writeups de ciberseguridad | Público (futuro) |

> La **instancia privada del tracker** (mis datos reales) no se publica en ningún subdominio público. Corre en mi máquina o detrás de Cloudflare Access, además de su propio login. Se define en el relevamiento del tracker.

### 5.3 Stack del portfolio

| Capa | Tecnología | Motivo |
|---|---|---|
| Sitio | HTML/CSS (v1) → Astro + TypeScript (v2) | Estático, rápido, sin superficie de ataque |
| Contenido | Archivos `.json` / `.md` en el repo | Se cargan sin tocar el diseño |
| Hosting | Cloudflare Pages o GitHub Pages | Gratis, con HTTPS y headers configurables |
| DNS | Cloudflare | Subdominios simples, gratis |
| Gestión | GitHub Projects | Backlog público, muestra cómo organizo el trabajo |

### 5.4 Panel de administración: postergado

En la versión 0.2 se había decidido un backend propio con login y un panel para gestionar el contenido. **Se posterga** por dos motivos:

1. El objetivo de aprendizaje (escribir yo el backend y la autenticación) lo cumple mejor el tracker, que tiene datos reales y más reglas de negocio.
2. Hoy el volumen de contenido es chico: cuatro casos y unos pocos labs. Un panel no se justifica todavía.

**Cuándo se retoma:** cuando cargar contenido en archivos se vuelva lento o frecuente, por ejemplo si hay más de un lab por semana o secciones nuevas como videos. En ese momento se evalúa un CMS basado en Git o una plataforma de gestión propia, que podría reutilizar lo aprendido con el tracker.

## 6. Seguridad del portfolio

| ID | Requisito | Etapa |
|---|---|---|
| SEG-01 | HTTPS en todo el dominio y subdominios | v1 |
| SEG-02 | Headers de seguridad: CSP, HSTS, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy` | v1 |
| SEG-03 | Sin scripts ni trackers de terceros | v1 |
| SEG-04 | Las demos nunca contienen datos reales de personas | v1 |
| SEG-05 | Revisión de seguridad de cada demo antes de publicarla (usuario de prueba con permisos limitados, sin endpoints abiertos, límite de intentos de login) | v1 (club) / tracker |
| SEG-06 | Formulario de contacto con protección antispam y límite de envíos | v3 |

> Los requisitos de seguridad propios del tracker (login escrito a mano, 2FA, auditoría con OWASP ZAP) se documentan en el relevamiento del tracker.

## 7. Lógica de negocio identificada

**Del portfolio:**
- RN-01: El idioma por defecto se toma del navegador. Si no es español ni inglés, se muestra en español. La elección manual del usuario tiene prioridad sobre la detección.
- RN-02: Cada proyecto tiene un estado: *demo en vivo*, *en desarrollo* o *caso* (sin demo). La etiqueta en el sitio refleja ese estado.
- RN-03: Un proyecto se muestra como *demo en vivo* solo si la demo responde. Si no, se muestra como *próximamente*.
- RN-04: Un lab tiene como mínimo título, fecha, plataforma (TryHackMe, CCNA, lab propio) y link al writeup.
- RN-05: Las métricas de la búsqueda laboral que salgan del tracker se publican solo agregadas, sin empresas ni personas identificables.

**De las demos (club y tracker):**
- RN-06: La base de cada demo se restablece automáticamente una vez por día con datos ficticios predefinidos.
- RN-07: Cada demo tiene un único usuario de acceso público con permisos limitados.

## 8. Decisión: un solo dominio

**Se usa un único dominio con mi nombre** para los dos públicos: búsqueda laboral y clientes freelance.

- El sitio principal queda orientado al trabajo en relación de dependencia.
- Los clientes tienen una sección propia, `/servicios`, con tono comercial. Desde la home se accede de forma discreta.
- El formulario de contacto distingue *propuesta laboral* de *proyecto freelance*.

**Cuándo se revisa:** cuando el freelance tenga tres o más proyectos reales, o cuando quiera una marca comercial con otro nombre.

## 9. Alcance

### 9.1 Incluido

- Sitio en español con perfil, casos, experiencia, formación, ciberseguridad, herramientas y contacto.
- Cuatro proyectos:
  1. **Sistema de gestión del club:** demo en vivo, repo y nota sobre el uso de IA.
  2. **Tracker de postulaciones:** primero como *en desarrollo*, con el problema, el enfoque y el modelo de datos; después con demo en vivo.
  3. **Mejora de procesos con Excel en el club:** caso funcional.
  4. **Implementación de ERP en Mantis:** caso sin código ni capturas.
- Subsección de ciberseguridad con labs y writeups (incluido el lab de Active Directory).
- Contenido en archivos de datos, separado del diseño.
- Sección `/servicios`, versión en inglés y formulario de contacto (etapas posteriores).
- Documentación: alcance, arquitectura, changelog.
- Backlog público en GitHub Projects.

### 9.2 Excluido (por ahora)

- Servidor propio y panel de administración para el portfolio (ver 5.4).
- Instancia privada del tracker en un subdominio público.
- Blog con comentarios o usuarios registrados.
- Videos explicativos: quedan en el backlog.
- Analíticas con seguimiento de usuarios.
- Un segundo dominio para clientes.
- Proyectos que todavía no existen (billetera virtual) o que no prosperaron (PRECO).
- Código o capturas de software de terceros (Mantis).

### 9.3 Por decidir

- Hosting del backend del club (y después, del tracker).
- Si se incluye la web de los prestamistas como caso freelance.

## 10. Etapas

Cada etapa termina con algo publicado y funcionando.

| Etapa | Nombre | Contenido | Criterio de cierre |
|---|---|---|---|
| **v0** | Fundaciones | Este documento, registro del dominio, repo, backlog inicial en GitHub Projects | Dominio registrado, repo creado, backlog cargado |
| **v1** | Vidriera + demo del club | Sitio estático con los cuatro proyectos (el tracker como *en desarrollo*), CV, contacto por email, headers de seguridad, demo del club con datos ficticios y reset diario | Sitio online y link en el CV y LinkedIn. **Plan B:** si el hosting del club se demora, se publica igual con la demo como *próximamente* |
| **v1.1** | Labs | Lab de Active Directory y labs de TryHackMe y CCNA en la sección de ciberseguridad | Lab de AD publicado y enlazado |
| **v2** | Demo del tracker | Cuando el MVP del tracker esté listo: demo en `tracker.`, caso completo con aprendizajes agregados del dashboard | Demo en vivo con reset diario |
| **v3** | Alcance ampliado | Migración a Astro, versión en inglés, formulario de contacto, `/servicios` | Las dos versiones completas y el formulario probado |
| **v4+** | Gestión y contenido | Panel de administración (si se cumple el criterio de 5.4), videos, nuevos proyectos | Se define en su momento |

**Orden de prioridades:**
1. Postulaciones diarias.
2. Esta semana (después de la entrega del TP del 18/11): portfolio v1 con la demo del club y lab de Active Directory.
3. Bloques fijos de CCNA.
4. Tracker: **no arranca hasta que la v1 esté publicada**. Antes de programar, escribo yo el relevamiento del MVP.

## 11. Forma de trabajo con IA

| Proyecto | Rol de la IA |
|---|---|
| Sistema del club | Escribió la autenticación, el frontend y la suite E2E; yo dirigí y validé |
| Tracker | Explica, guía y revisa. **El código lo escribo yo** |
| Portfolio (sitio estático y docs) | Asistencia amplia: no es el objetivo de aprendizaje |

En el README de cada repo y en cada caso del sitio se declara cómo se usó la IA.

## 12. Documentación prevista

| Doc | Contenido | Etapa |
|---|---|---|
| `01-alcance.md` | Este documento | v0 |
| `02-arquitectura.md` | Hosting, DNS, subdominios, headers, despliegue de las demos | v1 |
| `CHANGELOG.md` | Qué cambió en cada versión publicada | Desde v1 |
| *Tracker:* relevamiento del MVP | Alcance, historias de usuario y modelo de datos (lo escribo yo) | Antes de programar el tracker |

## 13. Supuestos y restricciones

- **Costo:** mínimo. Se paga el dominio y, si hace falta, el hosting de los backends de las demos.
- **Tiempo:** limitado, se comparte con la facultad, la beca y las postulaciones.
- **Honestidad:** todo lo que se afirme en el sitio tiene que ser verificable. El uso de IA se declara.
- **Privacidad:** ningún dato real de terceros (club, reclutadores, empresas) se publica.
- **Propiedad:** no se publica nada de software o datos de terceros.

## 14. Riesgos

| Riesgo | Impacto | Mitigación |
|---|---|---|
| El portfolio o el tracker frenan las postulaciones | Alto | Orden de prioridades fijo, la v1 se publica antes del tracker |
| Una vulnerabilidad en una demo deja mal parado un perfil con formación en ciberseguridad | Alto | SEG-04 y SEG-05, usuario de prueba limitado, reset diario |
| Se filtran datos reales de la instancia privada del tracker | Alto | Sin exposición pública, Cloudflare Access o uso local, métricas solo agregadas (RN-05) |
| La demo del club demora la publicación de la v1 | Medio | Plan B: publicar con la demo como *próximamente* |
| Un entrevistador pregunta por partes del club hechas con IA | Medio | Declararlo, estudiar las piezas clave y mostrar el tracker como contraejemplo |
| La demo se cae o tarda en arrancar en un plan gratuito | Medio | Estado visible (RN-03) y elegir hosting con eso en cuenta |
| El contenido deja de actualizarse | Medio | Contenido en archivos simples y criterio de publicación mensual |

## 15. Criterios de éxito

- El link del portfolio está en el CV, en LinkedIn y en cada postulación.
- Al menos una entrevista en la que el portfolio sirvió para mostrar un caso concreto.
- Las demos funcionan y se restablecen solas.
- Puedo explicar sin ayuda cómo funciona el código del tracker, incluida su autenticación.
- El backlog y la documentación están al día con lo publicado.
- Publico contenido nuevo al menos una vez por mes.

---

*Próximo paso: elegir y registrar el dominio, crear el repo del portfolio y cargar el backlog de la v1 en GitHub Projects.*
