# Conda Environments: `condas`

Este directorio contiene múltiples ambientes Conda organizados por dominio funcional:
- bioinfomática: NGS, GWAS, CNV, RNA-seq, anotación, workflows, etc.
- ...

Separar ambientes mejora:

* Reproducibilidad
* Estabilidad de dependencias
* Mantenimiento
* Velocidad de resolución

---

# Requisitos

* Conda (Miniconda / Anaconda)
  o
* Mamba
  o
* Micromamba

Se recomienda usar **mamba o micromamba** por velocidad y menor conflicto de dependencias.

---

# Estructura

```
condas/
  bioinfornatician/
    genomics-core.yml
    alignment.yml
    calling-snv-indels.yml
    calling-cnv-sv.yml
    rna-seq.yml
    gwas-prs.yml
    qc-visualization.yml
    annotation.yml
    workflow.yml
    notebooks.yml
```

---

# Crear ambientes

## Con conda

```bash
conda env create -f genomics-core.yml
```

## Con mamba (recomendado)

```bash
mamba env create -f genomics-core.yml
```

## Con micromamba

```bash
micromamba create -f genomics-core.yml
```

---

# Activar ambiente

```bash
conda activate genomics-core
```

o si usas micromamba:

```bash
micromamba activate genomics-core
```

---

# Actualizar ambiente

```bash
mamba env update -f genomics-core.yml --prune
```

El flag `--prune` elimina paquetes que ya no estén en el archivo YAML.

---

# Eliminar ambiente

```bash
conda remove -n genomics-core --all
```

---

# Exportar ambiente reproducible

Exportar solo paquetes explícitos:

```bash
conda env export --from-history -n genomics-core > genomics-core.lock.yml
```

Exportar con versiones exactas:

```bash
conda env export -n genomics-core > genomics-core.full.yml
```

---

# Crear desde archivo lock

```bash
mamba env create -f genomics-core.lock.yml
```

---

# Buenas prácticas

* No mezclar herramientas de dominios distintos en un solo ambiente.
* Fijar versiones en producción:

  ```yaml
  - bcftools=1.19
  - samtools=1.19
  ```
* Preferir `conda-forge` y `bioconda`.
* Evitar canal `defaults` si no es necesario.
* Usar `mamba` para entornos grandes.

---

# Uso en HPC

Para clústeres:

```bash
module load micromamba
micromamba create -p /storage/envs/genomics-core -f genomics-core.yml
micromamba activate /storage/envs/genomics-core
```

O crear en un path específico:

```bash
mamba create -p /opt/conda-envs/genomics-core -f genomics-core.yml
```

---

# Sugerencia de flujo

Ejemplo para análisis de variantes:

```bash
mamba activate alignment
# alineamiento

mamba activate variant-calling
# llamado de variantes

mamba activate annotation
# anotación
```

---

# Notas

* Estos ambientes están pensados para pipelines reproducibles.
* Para máxima estabilidad en producción, considerar:

  * lockfiles
  * contenedores (Apptainer / Docker)
  * CI de validación de ambientes
