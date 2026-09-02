# worker-productivity-clustering

Segmentación no supervisada de operarios de campo de una empresa agroexportadora (fundos en Piura), a partir de variables de productividad, comportamiento laboral y perfil sociodemográfico, con el objetivo de identificar perfiles de desempeño y apoyar decisiones de retención de personal.

Autor: Stefano Giacomo Landa Morante — Universidad Nacional de Ingeniería (UNI)

Datos y variables
Unidad de análisis: operarios (trabajadores de campo)

Variables utilizadas: número de campañas trabajadas, edad, número de hijos, nivel de pobreza, productividad per cápita (log), días trabajados en la empresa (log), ingreso total (log) y sexo

Imputación: valores faltantes en distancia al fundo, edad y número de hijos de asistentes imputados con KNN Imputer (k=5)
Estandarización: variables continuas escaladas con StandardScaler antes del clustering


Metodología:

Preprocesamiento: imputación KNN de valores faltantes, transformación logarítmica de variables con alta dispersión (productividad, días trabajados, ingreso total) y estandarización Z.

Selección del número de clusters: método del codo (evolución de la inercia intra-cluster) evaluando de 1 a 15 clusters.

Modelo final: K-Means con k=4, n_init=20, random_state=123.

Validación: comparación de K-Means contra Gaussian Mixture Model (GMM) usando Silhouette Score, Davies-Bouldin Score y Calinski-Harabasz Score.

Visualización: proyección de los clusters en 3 componentes principales (PCA) para inspección visual de la separación entre grupos.


Resultados del modelo:

Métrica	K-Means	GMM
Silhouette Score	0.1482	0.0383
Davies-Bouldin	1.8740	4.2177
Calinski-Harabasz	4019.2	1920.1

Las tres métricas coinciden en que K-Means produce una estructura de clusters más clara y mejor separada que GMM, lo que sugiere que los grupos tienen una forma compacta cercana al supuesto esférico de K-Means.

Perfiles identificados:
Cluster	Perfil	% operarios	Productividad (jabas/hora)	% de producción generada
S1	Jóvenes altamente productivos	32.4%	100.34	51.4%
S2	Mujeres adultas productivas	20.2%	83.19	35.8%
S3	Operarios adultos, experiencia media	21.2%	47.87	11.3%
S4	Jóvenes de baja productividad	26.2%	22.72	1.5%

Hallazgos clave:

S1 (Jóvenes Alt. Productivos): el grupo más numeroso y productivo; jóvenes (26 años en promedio), versátiles en variedades y fundos, con poca antigüedad pero muy alto rendimiento.
S2 (Mujeres Adultas Productivas): el grupo más leal y rentable a largo plazo — mayor ingreso, mayor constancia (582 días trabajados) y casi 4 campañas de experiencia en promedio; prioritario para retención.
S3 (Operarios Adultos, Experiencia Media): mayor edad promedio (42.7 años) pero baja antigüedad relativa (1.46 campañas), con el nivel de bancarización más alto de todos los grupos.
S4 (Jóvenes de Baja Productividad): el segmento más problemático — un cuarto de los operarios genera apenas 1.5% de la producción; se caracteriza por vivir más lejos del fundo (132 km), poca experiencia y alta rotación, representando un alto costo de selección y capacitación con bajo retorno.
Conclusión

La segmentación identifica dos grupos que concentran más del 87% de la producción (S1 + S2) pese a ser aproximadamente la mitad de la fuerza laboral, mientras que el grupo S4 representa un costo de rotación elevado con retorno productivo mínimo. Esto sugiere que la empresa debería priorizar la retención de S2 y explorar estrategias de acompañamiento o capacitación temprana para reducir la rotación en S4.

Estructura del repositorio
├── Examen_Parcial_copy.ipynb   # Notebook con preprocesamiento, clustering y perfiles
├── data/                        # Dataset utilizado (Pedregal, Piura)
└── README.md
Cómo ejecutarlo
bash
git clone https://github.com/Stefanolm/[nombre-del-repo].git
cd [nombre-del-repo]
pip install -r requirements.txt   # pandas, numpy, scikit-learn, seaborn, matplotlib
jupyter notebook Examen_Parcial_copy.ipynb
Herramientas utilizadas

Python — pandas, scikit-learn (KMeans, GaussianMixture, KNNImputer, StandardScaler, PCA), seaborn, matplotlib.
