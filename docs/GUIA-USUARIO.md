# Guía del usuario — Invitaciones y lista de invitados

Sistema de invitaciones digitales con **RSVP por WhatsApp**: cada invitado tiene un enlace personalizado que muestra su nombre y pre-carga el mensaje de confirmación.

---

## 1. Acceso al panel del organizador

1. Abrí la página de administración: **`https://<tu-sitio>/admin`** (ej. `https://misxv.vercel.app/admin`).
2. Ingresá la **clave de administrador** (te la entrega el encargado; se guarda solo en ese navegador).
3. Entrás y ves la lista de invitados, las herramientas para crearlos y los estados de confirmación.

> En el celular también funciona: la lista se muestra como tarjetas con botones grandes.

---

## 2. Crear invitados

En el panel **"Crear invitados"** pegá la lista. **Una línea por invitado**, separando con `;`:

```
Ana María Pérez; 2; a
Juan Carlos; 1; d
Flia Jaramillo; 3; fam
Carlos y Ana; 2; grupo
```

| Columna | Qué es | Opciones |
|---|---|---|
| Nombre | Nombre como aparece en la invitación | obligatorio |
| Cupos | Cuántas personas ocupa | opcional, por defecto 1 |
| Tratamiento | Cómo se saluda al invitado | opcional: `a` (Querida), `d` (Querido), `fam` (Querida familia), `grupo` (Queridos) |

También se aceptan `|` como separador (`Nombre | 2 | d`) y tab (pegar desde Excel).

Al apretar **"Crear invitados"**, se generan los enlaces de cada uno. Si alguna línea tiene un error (ej. cupos inválido), se indica cuál y por qué.

---

## 3. La lista de invitados

- **Buscar**: escribí en el campo de búsqueda para filtrar por nombre, cupos o estado.
- **Paginación**: la lista se muestra de a 20; usá *Anterior / Siguiente*.
- **Contador**: indica cuántos hay (ej. "12–20 de 84").

Cada invitado tiene estos botones:

| Botón | Acción |
|---|---|
| **Copiar link** | Copia el enlace personalizado de ese invitado |
| **Editar** | Cambia nombre, cupos o tratamiento (el enlace NO cambia) |
| **Confirmar** | Marca que confirmó asistencia (verde) |
| **No asistirá** | Marca que no puede asistir (rojo) |
| **Pendiente** | Vuelve a pendiente (ámbar) |

**Copiar todos**: copia los enlaces de todos los invitados **que coinciden con la búsqueda actual** (si no hay búsqueda, todos), uno por línea — ideal para mandarlos por WhatsApp de una vez.

---

## 4. Cómo lo ve el invitado

Al abrir su enlace personalizado, la invitación muestra:

- **Portada**: una dedicatoria con su nombre (`para Flia Jaramillo`).
- **Carta**: abre con el saludo según el tratamiento (`Querida Ana María Pérez:`).
- **Botón WhatsApp**: el mensaje ya viene con su nombre completo, listo para enviar — el invitado **no tiene que escribir nada**.

El invitado responde por WhatsApp al organizador, y el organizador marca **Confirmar / No asistirá** en el panel.

---

## 5. Preguntas frecuentes

**¿El enlace sigue funcionando si edito el nombre?** Sí — el enlace (token) no cambia al editar. Solo cambia cómo se ve el nombre.

**¿Puedo invitar a más gente después?** Sí: pegá las líneas nuevas en "Crear invitados" y se suman.

**¿Qué pasa si el invitado pierde el enlace?** Buscalo en la lista y tocá **Copiar link** de nuevo.

**¿Puedo cambiar la clave de administrador?** Sí — es una configuración del servidor; avisá al encargado técnico.

**¿Esto reemplaza el WhatsApp?** No: el WhatsApp es el canal de confirmación. El panel solo registra quién confirmó.

---

## 6. Datos técnicos rápidos (para el encargado)

- Enlace público: `/?invitacion=<token>` (token de 24 caracteres, único por invitado).
- Backend: tabla `guests` en Supabase + RPC `get_guest_rsvp` (lectura pública solo por token, sin exponer la lista).
- Panel: `src/pages/admin.astro` — consulta la Edge Function `guest-admin` (list / mint / set_status / update_guest) autenticada con `x-admin-secret`.
- Estado: `pending | confirmed | declined` · Tratamiento: `a | d | fam | grupo`.