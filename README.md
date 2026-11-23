# Sant Cugat Avui

Web local de Sant Cugat del Vallès, en catalán, castellano e inglés, hermana de [Cerdanyola Avui](https://cerdanyola.horaverda.es) (mismo código, adaptado a las fuentes de Sant Cugat). Reúne:
- la previsión del tiempo;
- las horas más baratas de la luz;
- los avisos de tren: FGC (S1 y S2) y Rodalies R8;
- la agenda (Ajuntament y Agenda cultural de Catalunya), con las fiestas y conciertos marcados con el icono de fiesta y los actos infantiles con el suyo;
- avisos del Ajuntament en vigor (tráfico, obras, cortes, tratamientos) y los plazos de ayudas, oposiciones y licitaciones que cierran en 14 días;
- redes («Xarxes»): los vídeos más vistos del último año de canales locales de YouTube y un directorio de perfiles (`curated/social.json`; añadir un perfil = una línea, y con `youtube_id` sus vídeos entran solos);
- recomendaciones de la semana: lugares y experiencias elegidos a mano (`curated/picks.json`), que rotan cada semana. De momento solo hay experiencias (`"kind": "experience"`); al añadir seis o más fichas `"kind": "food"` aparece «Dónde comer»;
- «Participa»: guía neutral de participación política del municipio al planeta (Ajuntament, AMB, Consell Comarcal, Diputació, Generalitat, Estado, UE, ONU): quién decide qué, cómo se elige y cómo influir, con enlaces oficiales (`curated/politics.json`, escrito a mano; `--check` avisa si `checked` tiene más de un año);
- una página «Ahorra»: horas baratas de la luz, gasolinera más barata, actos gratuitos y ayudas para particulares;
- una guía de ocio: plan para hoy según el tiempo, próximas fiestas, rutas, fuentes, miradores, patrimonio, restaurantes, bares y pubs;
- las ayudas, becas y subvenciones que se pueden pedir ahora (Ajuntament, comarca, AMB, Generalitat y Estado), con su plazo;
- empleo: ofertas de Feina Activa (SOC) en Sant Cugat y municipios vecinos, y oposiciones y bolsas públicas del CIDO;
- los precios de hoy de las gasolineras de Sant Cugat y alrededores;
- inversiones y empresas: grandes contratos públicos (1 M€ o más), licitaciones abiertas, contratos anunciados y altas, traslados y cambios de capital de empresas de Sant Cugat según el BORME, con un canal Atom por idioma. Es información, no consejo de inversión;
- la calidad del aire (Sant Cugat tiene estación propia);
- un directorio de equipamientos públicos y puntos de recarga;
- una página de emergencias con los teléfonos útiles;
- las farmacias de Sant Cugat (directorio) y el enlace al buscador oficial de farmacias de guardia;
- buscador y filtros en las listas largas (empleo, ayudas y agenda), sin servidor: `static/filter.js`;
- `agenda.ics`, la agenda como calendario para suscribirse desde el móvil.

Se actualiza sola cuatro veces al día con GitHub Actions y se publica en GitHub Pages. Funciona sin dependencias y sin cookies.

## Lo que cambia respecto a Cerdanyola

| | Cerdanyola | Sant Cugat |
|---|---|---|
| Agenda municipal | HTML de `cerdanyola.cat/agenda` | RSS `santcugat.cat/download/agenda.rss` (campos `tipus`, `lloc`, `hora`, `datainici`, `datafi`) |
| Avisos municipales | RSS de noticias filtrado por palabras clave | RSS propio de avisos (`/download/avisos.rss`), con fecha de inicio y de fin: salen mientras están en vigor |
| Trenes | Renfe R4, R7, R8 | FGC S1 y S2 (GTFS-RT en protobuf, `dadesobertes.fgc.cat`) + Renfe R8 |
| Farmacias de guardia | Calendario mensual en PDF del Ajuntament | No hay calendario abierto: directorio de OpenStreetMap + enlace a Farmaguia (Col·legi de Farmacèutics de Barcelona) |
| Agenda UAB y entidades | Sí | No (`TRIBE_FEEDS` vacío; añadir una entidad = una línea) |
| Tablón vecinal | En marcha | Apagado hasta que tenga su worker: la sección solo sale si `board_api_url` está en `config.json` |

## Fuentes

| Dato | Fuente | Licencia |
|---|---|---|
| Tiempo | MET Norway `locationforecast` | CC BY 4.0 |
| Agenda | Ajuntament (RSS `santcugat.cat/download/agenda.rss`) y Generalitat (`rhpv-yr4f`) | Llicència oberta Catalunya; del Ajuntament solo se usan título, fechas, hora, lugar, categoría y enlace (ni descripción ni imágenes) |
| Avisos del Ajuntament | RSS `santcugat.cat/download/avisos.rss` | Solo titular, fechas y enlace; no se guarda ni se publica el cuerpo del aviso |
| Equipamientos, recarga, aire | Generalitat (`8gmd-gz7i`, `tb2m-m33b`, `tasf-thgu`) | Llicència oberta Catalunya |
| Avisos de trenes | FGC `alerts-gtfs_realtime` (protobuf) y Renfe GTFS-RT `alerts.json` | CC BY 4.0 |
| Farmacias | OpenStreetMap vía Overpass (misma consulta que la guía de ocio) | ODbL. Sin guardias: se enlaza `farmaguia.net` |
| Luz | web hermana Hora Verda (`light_api_url` en `config.json`) | Red Eléctrica, uso informativo |
| Empleo | CIDO de la Diputació (`api.diba.cat`) y XML de Feina Activa (SOC) | CC BY 4.0 y Llicència oberta Catalunya |
| Carburantes | API del Ministerio para la Transición Ecológica (`FiltroMunicipio`) | Reutilización libre citando fuente y fecha |
| Redes | Feed Atom público de cada canal de YouTube + `curated/social.json` (cuentas de la lista oficial `santcugat.cat/web/xarxes-socials`) | Solo título, fecha, visitas y enlace; miniaturas servidas por YouTube. Instagram y TikTok: solo se enlazan perfiles |
| Recomendaciones | `curated/picks.json`, escrito a mano | Propio; `--check` avisa si una ficha lleva más de un año sin revisar (`checked`) |
| Participa | `curated/politics.json`, escrito a mano a partir de las leyes y webs oficiales enlazadas | Propio; sin datos de personas ni de partidos concretos; revisión anual (`checked`) |
| Guía de ocio | OpenStreetMap vía Overpass API (una consulta al día) | ODbL: `data/leisure.json`, `data/pharmacy.json`, `tests/fixtures/osm*.json` y `geo/santcugat.json` (término municipal) son © colaboradores de OpenStreetMap, bajo ODbL |
| Más locales nocturnos | Overture Maps, PMTiles de lugares de la última versión, leído por rangos HTTP | CDLA Permissive 2.0; se descartan los registros con otras licencias. `_NIGHT_SKIP` en `sources.py` excluye por nombre los clubs de alterne |
| Ayudas | Base de Datos Nacional de Subvenciones (`infosubvenciones.es/bdnstrans/api`) | Reutilización libre (Ministerio de Hacienda) |
| Inversiones y licitaciones | Plataforma de serveis de contractació pública (`ybgg-dgi6`): Ajuntament, Promusa, EMD de Valldoreix, CAR... | Llicència oberta Catalunya. Los nombres de adjudicatarios particulares no se publican. No incluye la plataforma del Estado |
| Empresas | BORME sección A de Barcelona (API de datos abiertos del BOE + página HTML del día) | Reutilización libre citando la fuente. Solo nombre, acto, capital y actividad. La primera vez lee un año entero en varias ejecuciones (40 días por pasada) |

Las alertas de FGC casi siempre traen solo notas de trenes concretos («OBSERVATIONS_…»: coches reservados a escolares, enlaces con autobús), que se descartan. Una incidencia se muestra si está etiquetada con S1 o S2 o si su texto nombra esas líneas o la línea Barcelona-Vallès. Ese filtro está probado con alertas sintéticas (`tests/test_site.py`), no con una incidencia real: conviene revisarlo la primera vez que FGC publique una.

## Comandos locales (Python 3.12 o superior)

```bash
python -m unittest discover tests
```

```bash
python src/build.py
```

```bash
python src/build.py --offline
```

```bash
python -m http.server 8003 -d _site
```

`data/*.json` guarda la última versión buena de cada fuente. Si una fuente cae, la web sigue mostrando lo último que tenía, y el workflow solo falla (y te avisa por email) si lleva 3 días sin responder.

## Publicación

1. Repositorio público en GitHub (`santcugat-local`), Settings → Pages → Source: «GitHub Actions».
2. DNS: `CNAME santcugat → <usuario>.github.io`; en Settings → Pages, dominio personalizado `santcugat.horaverda.es` y «Enforce HTTPS».
3. Contador de visitas: lo sirve el worker del tablón de Cerdanyola (`visits_url` en `config.json`, `site=sc`); hay que volver a desplegarlo para que acepte `sc`.
4. Tablón vecinal (opcional): seguir `worker/README.md` con un worker propio (`santcugat-board`) y poner su URL en `board_api_url` y la clave pública de Turnstile en `turnstile_site_key`.

## Diseño
- El marco común (`static/base.css`, `static/ui.js`, `static/fonts/`, `src/icons.py`, `src/tz.py`) se sincroniza desde `llum-hores` con `python ..\tools\sync_base.py`; no se edita aquí. Las reglas propias están en `static/site.css`; el generador sirve un solo `static/style.css`.
- Iconos meteorológicos: `static/wx/*.svg`, de [MET Norway weathericons](https://github.com/metno/weathericons) (MIT). Resto de iconos: Lucide (ISC) vía `src/icons.py`.
- PNG (iconos PWA, `apple-touch-icon`, `og.png`): `py tools/make_assets.py` (Pillow, solo en local).
- App instalable (PWA): `static/app.js`, `static/sw.js` (servido en `/sw.js`) y un manifest por idioma, igual que en Cerdanyola.
