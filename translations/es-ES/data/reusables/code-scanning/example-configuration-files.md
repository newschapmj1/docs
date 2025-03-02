Este archivo de configuración agrega el conjunto de consultas `security-and-quality` a la lista de consultas que se ejecutan con {% data variables.product.prodname_codeql %} cuando se escanea tu código. Para obtener más información acerca de los conjuntos de consultas que están disponibles para utilizarse, consulta la sección "[Ejecutar consultas adicionales](#running-additional-queries)".

``` yaml
name: "My {% data variables.product.prodname_codeql %} config"

queries:
  - uses: security-and-quality
```

El siguiente archivo de configuración inhabilita las consultas predeterminadas y especifica un conjunto de consultas personalizadas para ejecutarse en vez de éstas. También configura a {% data variables.product.prodname_codeql %} para escanear archivos en el directorio _src_ (relativo a la raíz), con excepción del directorio _src/node_modules_ y de los archivos cuyo nombre termine en _.test.js_. Los archivos en _src/node_modules_ y aquellos cuyos nombres terminan en _.test.js_ se excluyen por lo tanto del análisis.

``` yaml
name: "My {% data variables.product.prodname_codeql %} config"

disable-default-queries: true

queries:
  - name: Use an in-repository {% data variables.product.prodname_ql %} pack (run queries in the my-queries directory)
    uses: ./my-queries
  - name: Use an external JavaScript {% data variables.product.prodname_ql %} pack (run queries from an external repo)
    uses: octo-org/javascript-qlpack@main
  - name: Use an external query (run a single query from an external {% data variables.product.prodname_ql %} pack)
    uses: octo-org/python-qlpack/show_ifs.ql@main
  - name: Use a query suite file (run queries from a query suite in this repo)
    uses: ./codeql-qlpacks/complex-python-qlpack/rootAndBar.qls

paths:
  - src 
paths-ignore: 
  - src/node_modules
  - '**/*.test.js'
```

{% ifversion code-scanning-exclude-queries-from-analysis %}

El siguiente archivo de configuración solo ejecuta consultas que generan alertas de errores de gravedad. La configuración selecciona primero todas las consultas predeterminadas, todas las consultas en `./my-queries` y la suite predeterminada en `codeql/java-queries` para luego excluir todas las consultas que generan advertencias o recomendaciones.

``` yaml
queries:
  - name: Use an in-repository QL pack (run queries in the my-queries directory)
    uses: ./my-queries
packs:
  - codeql/java-queries
query-filters:
- exclude:
    problem.severity:
      - warning
      - recommendation
```

{% endif %}