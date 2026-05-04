# cro-informes

Repositorio de informes CRO comerciales de Industrial Shields.

## Arquitectura

- Cada comercial tiene su carpeta en `docs/{slug_comercial}/`
- HTML del informe cifrado con AES-256-GCM + PBKDF2 (SALT `cro-informes-v1`, 100k iter)
- Passphrase semanal de 32 caracteres enviada por email cada lunes 7am via Cowork
- Loader `index.html` por comercial: pide passphrase, descifra blob, renderiza HTML inline
- GitHub Pages sirve `/docs/`

## Carpetas

- `docs/` — root publicado por GitHub Pages
  - `index.html` — landing con lista de comerciales
  - `jordi/` — informe Jordi Hernandez (JHS)
    - `index.html` — loader que pide passphrase y descifra
    - `data/latest.html.enc` — blob cifrado del HTML actual
    - `data/index.json` — historial de versiones
- `scripts/` — scripts auxiliares (cifrado/recifrado)

## Tarea Cowork asociada

`cro-informes-weekly` — cron `0 7 * * 1` (lunes 7am). Por cada comercial activo:
1. Genera passphrase aleatoria 32 chars
2. Recifra el HTML actual del informe con esa passphrase
3. Commitea `data/latest.html.enc` actualizado al repo
4. Manda email al comercial + CC (Albert, Ramon, Fra) con link y passphrase

## Política de seguridad

- Repo PRIVADO en GitHub
- HTML siempre cifrado en reposo
- Passphrase rota cada 7 días → semanas anteriores quedan inaccesibles automáticamente
- Loader 100% client-side: el navegador descifra; el servidor nunca ve la passphrase
