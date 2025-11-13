# Laboratorio-5 Variabilidad de la Frecuencia Cardiaca usando la Transformada Wavelet
# Introducción 

La frecuencia cardíaca es un parámetro fisiológico fundamental que refleja la actividad del sistema nervioso autónomo (SNA), el cual regula el equilibrio entre la acción simpática —asociada con la respuesta de alerta y el aumento de la frecuencia cardíaca y la acción parasimpática relacionada con el reposo y la disminución de la misma. Las variaciones naturales que se presentan entre los intervalos consecutivos R–R del electrocardiograma (ECG) son conocidas como variabilidad de la frecuencia cardíaca (HRV, Heart Rate Variability), constituyen un indicador no invasivo de la función autonómica del corazón.

El análisis de la HRV permite identificar alteraciones en la modulación cardíaca y se ha convertido en una herramienta diagnóstica y de investigación en áreas como la fisiología, la cardiología y la ingeniería biomédica. Tradicionalmente, este análisis se realiza en el dominio del tiempo o de la frecuencia mediante la Transformada de Fourier; sin embargo, dichas técnicas no capturan adecuadamente la evolución temporal de los componentes de frecuencia. Por ello, se empleara métodos tiempo–frecuencia como la Transformada Wavelet Continua (CWT), que nos ayudara a estudiar la distribución espectral de la señal cardíaca con alta resolución temporal y frecuencial.

En la presente práctica de laboratorio se implementara un procesamiento digital completo de una señal ECG utilizando Python, con el fin de analizar la HRV a partir de los intervalos R–R y evaluar las variaciones en las bandas de baja (0.04–0.15 Hz) y alta frecuencia (0.15–0.4 Hz). Para ello, se aplicaran tecnicas de filtrado digital, detección de picos R y análisis tiempo–frecuencia mediante la transformada Wavelet. Los resultados obtenidos permiten observar cómo la actividad simpática y parasimpática se manifiestan en diferentes regiones espectrales, evidenciando la utilidad de las herramientas de procesamiento digital de señales para el estudio del control autonómico del corazón.

# Fundamento teorico 
1. Sistema Nervioso Autónomo (SNA) y control de la frecuencia cardíaca

El sistema nervioso autónomo (SNA) es el encargado de regular de manera involuntaria las funciones fisiológicas esenciales del organismo, entre ellas la actividad cardíaca. Este sistema se divide en dos ramas principales: el sistema simpático y el sistema parasimpático.

El sistema simpático se activa en situaciones de estrés o alerta, liberando catecolaminas (como la adrenalina), lo que incrementa la frecuencia cardíaca, la contractilidad del miocardio y la presión arterial. Por el contrario, el sistema parasimpático, mediado por el nervio vago y la liberación de acetilcolina, promueve estados de reposo y recuperación, reduciendo la frecuencia cardíaca y favoreciendo la estabilidad del ritmo sinusal.

El balance dinámico entre ambas ramas determina el comportamiento del ritmo cardíaco en reposo y durante estímulos externos. Un predominio simpático se asocia con menor variabilidad entre los intervalos cardíacos, mientras que un predominio parasimpático genera mayor variabilidad. Por ello, la evaluación de la variabilidad de la frecuencia cardíaca (HRV) se ha convertido en un indicador indirecto del estado funcional del SNA.

<img width="1080" height="1350" alt="image" src="https://github.com/user-attachments/assets/dd9a31d8-5494-4d88-aebb-b05050810eea" />

2. Variabilidad de la Frecuencia Cardíaca (HRV)

La variabilidad de la frecuencia cardíaca (HRV) se define como la fluctuación temporal entre intervalos consecutivos R–R del electrocardiograma (ECG). Dichas variaciones reflejan la capacidad del sistema cardiovascular para adaptarse a cambios fisiológicos y ambientales.

El análisis de la HRV puede realizarse en distintos dominios:

Dominio del tiempo: Se calculan parámetros estadísticos a partir de los intervalos R–R, como la media (mean RR) y la desviación estándar (SDRR o SDNN). Un valor alto de desviación estándar indica una mayor modulación vagal y, por tanto, un buen equilibrio autonómico.

Dominio de la frecuencia: Se estudia la distribución de energía en bandas espectrales específicas:

Baja frecuencia (LF, 0.04–0.15 Hz): relacionada con la actividad simpática y parasimpática combinada.

Alta frecuencia (HF, 0.15–0.4 Hz): asociada principalmente a la modulación parasimpática (vagal).

La razón LF/HF es usada como indicador del balance autonómico.

El análisis de HRV proporciona información valiosa sobre la regulación cardiovascular, pudiendo emplearse en el diagnóstico de estrés, fatiga, patologías cardíacas o alteraciones del sistema nervioso autónomo.

<img width="750" height="337" alt="image" src="https://github.com/user-attachments/assets/35e33912-f2bf-426f-9942-3341f9d4d58d" />

3. Transformada Wavelet

La transformada Wavelet es una herramienta matemática utilizada para analizar señales no estacionarias, es decir, aquellas cuyos componentes de frecuencia cambian con el tiempo, como las señales biológicas.
A diferencia de la Transformada de Fourier, la Wavelet permite obtener información simultáneamente en el dominio del tiempo y de la frecuencia, mediante el uso de funciones base denominadas wavelets madre.

En el análisis de la HRV, la transformada Wavelet permite:

Detectar variaciones transitorias en las bandas de baja frecuencia (LF) y alta frecuencia (HF).

Evaluar la evolución temporal de la actividad simpática y parasimpática.

Visualizar la potencia espectral mediante un escalograma o espectrograma Wavelet, que representa cómo cambia la energía de la señal en diferentes frecuencias a lo largo del tiempo.

El uso de la Transformada Wavelet mejora la interpretación fisiológica de la HRV y complementa los análisis tradicionales en los dominios del tiempo y la frecuencia.

# Adquisición de la señal ECG

Para el desarrollo del laboratorio se realizó la adquisición de la señal electrocardiográfica (ECG) de un compañero en condiciones de reposo, con el fin de analizar la variabilidad de la frecuencia cardíaca (HRV).

1. Instrumentación utilizada

Electrodos de superficie: 3 electrodos colocados según la configuración estándar para derivación DI (brazo derecho, brazo izquierdo y pierna derecha como referencia).

Sistema de adquisición: módulo de adquisición de datos compatible (BiTalino).

Software de captura: aplicación de registro o script en Python.

Frecuencia de muestreo: entre 250 Hz y 500 Hz, suficiente para capturar las componentes relevantes del ECG.

Duración del registro: aproximadamente 5 minutos, en estado de reposo.

2. Procedimiento experimental

# Preparación del sujeto:

Se limpió la piel con alcohol isopropílico para reducir la impedancia.

Se colocaron los electrodos en las posiciones anatómicas correspondientes.

Se solicitó al sujeto permanecer sentado, relajado y sin hablar durante el registro.

# Configuración del sistema de adquisición:

Se verificó la conexión de los cables y la polaridad de los electrodos.

Se seleccionó la frecuencia de muestreo y el canal de entrada.

Se inició la captura continua de la señal ECG por un periodo de 5 minutos.

 # Registro de la señal:

Se almacenaron los valores del voltaje en función del tiempo en un archivo para su posterior procesamiento digital.

3. Consideraciones de calidad de señal

Para garantizar una señal ECG confiable y de buena calidad se tuvieron en cuenta las siguientes precauciones:

Utilizar electrodos bien adheridos y cables de baja impedancia.

Mantener un entorno sin movimiento y con mínimas interferencias eléctricas.

Asegurar una correcta referencia a tierra del sistema de adquisición.

Verificar que no existan pérdidas de conexión o ruido por mala colocación de los electrodos.

4. Resultado esperado de la adquisición

El resultado fue una señal ECG con complejos P-QRS-T claramente visibles, lo que permitió aplicar posteriormente el proceso de filtrado digital y la detección de picos R para calcular los intervalos R-R y analizar la HRV.

Se evitó la proximidad de fuentes de ruido electromagnético (celulares, monitores, cables de corriente).

<img width="358" height="356" alt="image" src="https://github.com/user-attachments/assets/6c3ababc-1268-4077-b8ed-0a32175046b7" />

<img width="365" height="645" alt="image" src="https://github.com/user-attachments/assets/7484a5ef-f936-40e2-979d-6ba0a00c5816" />

<img width="243" height="177" alt="image" src="https://github.com/user-attachments/assets/9731ac26-1883-4908-804c-07ae5483e291" />

## Diagrama de flujo

![image](https://github.com/user-attachments/assets/4580bdea-f575-46fa-8ea0-8af3424bc487)













