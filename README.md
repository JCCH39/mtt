# MANTENIMIENTO — paquete para Cloudflare Workers

Este paquete conserva el dashboard original en `public/index.html` y añade un Worker para reemplazar las rutas Netlify que el HTML ya utiliza:

- `/.netlify/functions/admin-auth` (GET, POST y DELETE)
- `/.netlify/functions/analizar` (POST)

## Estructura

```text
MT/
├── wrangler.jsonc
├── public/
│   └── index.html
├── src/
│   └── index.js
└── README.md
```

## Subir a GitHub

1. En el repositorio `MT`, coloca el contenido de esta carpeta en la raíz del repositorio (no subas la carpeta `MT_CLOUDFLARE` como una carpeta anidada).
2. Deben quedar `wrangler.jsonc`, `public/` y `src/` al mismo nivel.
3. Conserva cualquier otro archivo que tu repositorio necesite; no borres `netlify/` ni `netlify.toml` hasta validar el despliegue.

## Cloudflare Workers Builds

- Comando de construcción: vacío / ninguno.
- Comando de despliegue: `npx wrangler deploy`.
- Directorio raíz: `/`.
- El archivo `wrangler.jsonc` configura `public/` como carpeta de archivos estáticos.

## Secretos obligatorios

En Cloudflare → Workers & Pages → MT → Settings → Variables and Secrets, agrega como **secrets**:

- `ADMIN_PASSWORD`: contraseña elegida por el administrador.
- `SESSION_SECRET`: cadena aleatoria larga (mínimo 32 caracteres), distinta de la contraseña.
- `OPENAI_API_KEY`: clave de API de OpenAI para habilitar el análisis con IA.

Opcional:

- `OPENAI_MODEL`: modelo disponible en tu cuenta; si no se define, se usa `gpt-4o-mini`.

Después de agregar o modificar secretos, realiza un nuevo despliegue.

## Notas importantes

- El HTML se conserva sin rediseñarlo.
- El Worker implementa las rutas que el HTML llama con el prefijo `/.netlify/functions/` para mantener compatibilidad sin reescribir el dashboard.
- Sin `ADMIN_PASSWORD` no funcionará el acceso administrativo; sin `OPENAI_API_KEY` el chatbot podrá seguir mostrando sus respuestas locales, pero el análisis con IA responderá que falta configurar la clave.
- No publiques claves API ni contraseñas dentro de GitHub, `index.html` o `wrangler.jsonc`.
- Este paquete incluye el HTML que se compartió en esta conversación. Si el dashboard dependía de funciones Netlify adicionales no presentes en ese HTML, deberán migrarse por separado.
