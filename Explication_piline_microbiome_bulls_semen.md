# Análisis de Microbioma en Semen Bovino mediante QIIME 2

> *This project was done on Purdue University's computing clusters. Therefore, some parts of code may not pertain to your local machine.*

---
### Fuente: Raw sequence and Articule
Data is available in the National Center for Biotechnology Information (NCBI) Sequence Read Archive (SRA), Bioproject: PRJNA747921, Biosamples SAMN20300345- SAMN20300393. For purposes of reproducibility, all commands used in QIIME2 are available at 
[Link articule]("https://doi.org/10.1016/j.theriogenology.2022.01.029")
[Link scribs]("https://github.com/sheets27/16SrRNABullSemen")   
[Video]("https://youtu.be/M2iXewkYHE0?si=jhXIO4VN-7luI1nb")

### Introducción al Software
El software utilizado en este estudio es **QIIME 2** (*Quantitative Insights Into Microbial Ecology*), el cual no es una aplicación convencional con botones, sino un **ecosistema bioinformático de código abierto** diseñado para analizar datos de secuenciación masiva. Está desarrollado principalmente en el lenguaje de programación **Python** y se basa en una **arquitectura de plugins**, lo que le permite integrar herramientas externas muy potentes bajo una misma lógica de trabajo. 

En entornos de alto rendimiento como el cluster de la **Universidad de Purdue**, los científicos acceden a él mediante la **terminal de comandos** cargando módulos específicos que preparan todas las dependencias necesarias para procesar grandes volúmenes de datos genéticos.

---

### Metodología: Importación y Demultiplexación
La metodología inicia con la **Importación y Demultiplexación**, donde los archivos de texto crudos del secuenciador (**Fastq**) se transforman al formato de QIIME 2. Este paso es fundamental para organizar las millones de lecturas de ADN y asignar cada una al **toro correspondiente**. Tras la importación, se genera un **resumen de calidad** que permite observar el estado de las bases nitrogenadas en cada posición de la secuencia, lo que sirve de guía para los parámetros de limpieza que se aplicarán posteriormente.

---

### Proceso de Limpieza: DADA2 vs. OTUs
El proceso central de limpieza se realiza mediante la herramienta **DADA2**, la cual marca un cambio tecnológico importante respecto a los métodos tradicionales:

*   **Antiguamente (OTUs):** Los investigadores agrupaban las secuencias por similitud en "bolsas" llamadas *Operational Taxonomic Units*, generalmente con un umbral del 97%. Sin embargo, este método era menos preciso porque podía agrupar bacterias distintas en una misma categoría si se parecían mucho.
*   **Actualidad (ASVs):** DADA2 utiliza un modelo estadístico de **"desruido"** para generar *Amplicon Sequence Variants*. Los ASVs tienen una **resolución mucho mayor**, ya que son capaces de distinguir si una diferencia de un solo nucleótido en el ADN es un error de la máquina o si representa una bacteria realmente diferente, lo que permite una identificación mucho más fina de los microorganismos en el semen.

---

### Inferencia Filogenética
Una vez obtenidas las secuencias limpias (ASVs), se procede a la **Inferencia Filogenética**. En esta etapa se utiliza:
1.  **MAFFT:** Para alinear las secuencias.
2.  **FastTree:** Para construir un árbol evolutivo utilizando el método de **Máxima Verosimilitud Aproximada**. 

Este árbol no es una simple lista, sino una **representación jerárquica** que muestra qué tan emparentadas están las bacterias entre sí. La creación de un **árbol enraizado** es un requisito técnico indispensable para calcular métricas de distancia que consideren la historia evolutiva de las comunidades bacterianas, permitiendo entender mejor la estructura del microbioma.

---

### Validación mediante Rarefacción
Para asegurar que los resultados sean confiables, se aplica una validación mediante **Rarefacción**. Este proceso consiste en realizar **submuestreos aleatorios** de las lecturas para verificar si la profundidad de la secuenciación fue suficiente para capturar toda la biodiversidad presente. 

Si al aumentar el número de lecturas analizadas ya no se encuentran nuevas especies, se dice que la curva ha llegado a una **meseta**, lo que confirma que el experimento es robusto y que las comparaciones entre los toros sanos y aquellos con patologías como **vesiculitis o epididimitis** serán estadísticamente válidas.

---

### Clasificación Taxonómica y Análisis de Diversidad
Finalmente, se realiza la clasificación y el análisis final:
*   **Clasificación:** Utilizando un clasificador entrenado con la base de datos **Silva**, el sistema compara las secuencias obtenidas con millones de registros conocidos para asignarles un **nombre científico**. 
*   **Diversidad Alfa:** Para medir la riqueza dentro de cada muestra.
*   **Diversidad Beta:** Para comparar la composición global entre individuos. 

**Resultado final:** Permite a los investigadores identificar si existen bacterias específicas asociadas a problemas reproductivos y visualizar estas diferencias mediante **gráficos de barras taxonómicos**.
