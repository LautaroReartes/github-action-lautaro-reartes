# PasswordGenerator

GitHub Action personalizada para generar contraseñas aleatorias y seguras, permitiendo definir la cantidad de caracteres deseada.

## Características

- Generación de contraseñas aleatorias.
- Longitud configurable mediante un parámetro de entrada.
- Utiliza `System.Security.Cryptography.RandomNumberGenerator` para la generación de números aleatorios criptográficamente seguros.
- Combina letras minúsculas, mayúsculas, números y caracteres especiales.
- Devuelve la contraseña generada mediante un output reutilizable en otros pasos del workflow.

## Estructura del repositorio

```text
github-action-lautaro-reartes/
│
├── PasswordGenerator/
│   └── action.yml
│
└── .github/
    └── workflows/
        └── test.yml
```

## Parámetros de entrada

| Parámetro | Descripción | Obligatorio | Valor por defecto |
|---|---|---|---|
| `Longitud` | Cantidad de caracteres de la contraseña. | Sí | `10` |

## Outputs

| Output | Descripción |
|---|---|
| `resultado` | Contraseña generada. |

## Caracteres utilizados

La contraseña se genera utilizando una combinación de los siguientes caracteres:

- Letras minúsculas: `a-z`
- Letras mayúsculas: `A-Z`
- Números: `0-9`
- Caracteres especiales: `!@#$%&*_-+=`

## Instalación y uso

Para utilizar esta Action desde un workflow, se debe invocar mediante la propiedad `uses`.

### Ejemplo de uso

```yaml
name: Test Password Generator

on:
  workflow_dispatch:
    inputs:
      longitud:
        description: 'Cantidad de caracteres de la contraseña'
        required: true
        type: number
        default: 16

jobs:
  generar:
    runs-on: windows-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Generar contraseña
        id: password
        uses: ./PasswordGenerator
        with:
          Longitud: ${{ inputs.longitud }}

      - name: Mostrar resultado
        run: |
          Write-Host "Contraseña generada: ${{ steps.password.outputs.resultado }}"
        shell: pwsh
```

En este ejemplo, el workflow permite ingresar manualmente la cantidad de caracteres desde la interfaz de GitHub Actions.

La contraseña generada queda disponible mediante:

```yaml
${{ steps.password.outputs.resultado }}
```

## Utilización desde otros repositorios

La Action también puede utilizarse desde cualquier otro repositorio público, indicando el propietario, repositorio, directorio y rama correspondiente.

```yaml
- name: Generar contraseña
  id: password
  uses: LautaroReartes/github-action-lautaro-reartes/PasswordGenerator@main
  with:
    Longitud: 20
```

Para recuperar el resultado:

```yaml
${{ steps.password.outputs.resultado }}
```

## Consideraciones

- La longitud debe ser un número entero mayor a cero.
- La generación utiliza un generador criptográficamente seguro.
- Se recomienda evitar imprimir contraseñas sensibles directamente en los logs del workflow.
- La Action está implementada utilizando el formato `composite` y ejecuta sus instrucciones mediante PowerShell (`pwsh`).

## Autor

**Lautaro Reartes**

[GitHub - LautaroReartes](https://github.com/LautaroReartes)
