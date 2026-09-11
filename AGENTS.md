# Useful Tech & Home Gear — Web, SEO & Amazon — instrucciones permanentes

Aplican a todos los chats, agentes, Tasks y ejecuciones del proyecto.

- Repositorio canónico único: `usefultechhomegear/usefultechhomegear.github.io`
- Repository ID: `1340720092`; rama: `main`.
- Abrir directamente por nombre completo. Cero resultados en búsqueda genérica no significa que falte.
- Leer primero `AGENTS.md`, `policies/github-only-lock.json`, `policies/source-precedence.json`, `policies/pinterest-publication-bridge.json`, `docs/pinterest-publication-runbook.md` y `state/project-state.json`.
- GitHub es la única memoria persistente y el único plano de gestión.
- No leer, escribir, buscar, auditar, recuperar ni usar Google Drive como fallback.
- Blogger/sitio publicado, Search Console, Amazon Associates y Pinterest sólo aportan hechos vivos. Persistir en GitHub cualquier cambio material.
- Excepción operativa autorizada: para producción, publicación y monitoreo de Pinterest, este proyecto puede operar sobre `usefultechhomegear/pinterest-amazon-car-safety-tech` exclusivamente como repositorio ejecutor. No es repositorio canónico de este proyecto ni fallback de memoria.
- Para operaciones Pinterest, seguir literalmente `policies/pinterest-publication-bridge.json` y `docs/pinterest-publication-runbook.md`; el control se realiza mediante `.github/pinterest/control/command.json` del repositorio ejecutor y el estado durable vuelve a GitHub.
- Contrato manual: `Genere los productos pendientes` → `generate-pending-products`; `Aprobados` → `approve-publish-monitor`; salida esperada tras publicar: `Publicación Finalizada` y monitoreo persistido.
- Las Tasks autónomas no deben quedar bloqueadas esperando `Aprobados`: cuando la política de la Task autorice publicación autónoma, deben completar generación → gates → creativos → publicación → monitoreo sin interacción humana.
- Si GitHub falla por conexión, autenticación o permisos, detenerse e informar; no usar otra fuente.
