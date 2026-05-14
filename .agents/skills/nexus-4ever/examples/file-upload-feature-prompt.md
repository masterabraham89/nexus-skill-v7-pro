# Prompt de carga de archivos segura para Antigravity

Actúa usando el skill `nexus-4ever`.

Necesito implementar o revisar una carga de archivos:

```text
[DESCRIBIR ARCHIVO, TIPO, USUARIO Y FINALIDAD]
```

Requisitos:

- Validar tamaño.
- Validar extensión allowlist.
- Validar MIME real.
- Renombrar archivo.
- Guardar en storage privado si contiene datos sensibles.
- Usar temporary URL para descarga.
- Eliminar metadata sensible en imágenes si aplica.
- Evitar path traversal.
- Auditar si el archivo es sensible.
- No exponer archivo públicamente sin justificación.

Entrega:

1. Diseño seguro.
2. FormRequest.
3. Service.
4. Repository si aplica.
5. Policy.
6. Tests.
7. Checklist de upload security.
