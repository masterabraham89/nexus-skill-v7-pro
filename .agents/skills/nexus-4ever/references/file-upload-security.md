# File Upload Security Reference

## Propósito

Este documento define reglas para cargas de archivos seguras. Los uploads pueden introducir malware, path traversal, consumo excesivo de almacenamiento, exposición de datos y ejecución de contenido no deseado.

## Reglas base

- Validar tamaño máximo.
- Validar MIME real.
- Validar extensión permitida.
- Renombrar archivo.
- Guardar fuera de `/public` si es privado.
- Usar URLs temporales.
- Eliminar metadata sensible cuando aplique.
- Escanear con antivirus si el riesgo lo exige.
- No ejecutar archivos subidos.
- No confiar en nombre original.

## Validación Laravel

```php
'file' => [
    'required',
    'file',
    'max:5120',
    'mimes:jpg,jpeg,png,pdf',
]
```

Complementar con validación de MIME real y procesamiento seguro.

## Nombres de archivo

Prohibido guardar con nombre original como ruta final.

Correcto:

```text
company/{companyId}/uploads/{uuid}.{extension}
```

## Storage

### Público

Solo para archivos que realmente deben ser públicos.

### Privado

Para:

- documentos;
- evidencias;
- datos personales;
- reportes;
- exports;
- contratos;
- archivos de clientes.

Usar signed/temporary URLs.

## Imágenes

- Validar dimensiones máximas.
- Recomprimir.
- Eliminar EXIF si contiene ubicación o información sensible.
- Convertir formato si aplica.
- Evitar image bombs.

## PDFs

- Validar tamaño.
- No renderizar inline sin controles si el contenido es no confiable.
- Considerar antivirus.
- Evitar extracción automática insegura.

## Path traversal

Nunca usar rutas enviadas por el cliente para decidir destino.

Prohibido:

```php
Storage::put($request->input('path'), $file);
```

## Checklist

- [ ] Tamaño máximo.
- [ ] MIME real validado.
- [ ] Extensión allowlist.
- [ ] Nombre renombrado.
- [ ] Storage privado si aplica.
- [ ] Temporary URL.
- [ ] Metadata sensible removida.
- [ ] Antivirus considerado.
- [ ] Path traversal prevenido.
- [ ] Auditoría si el archivo es sensible.
