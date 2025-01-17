# Informe de Laboratorio 10: Procesamiento de una señal EEG

## Integrantes
- Adrian Alberto Gutierrez Gonzales (adrian.gutierrez@upch.pe)
- Micaela Ivy Horny Insua (micaela.horny@upch.pe)
- Gian Pierre Santivañez Condor (gian.santivanez@upch.pe)
- Rodrigo Alfonso Gonzales Cabrera (rodrigo.gonzales@upch.pe)

## Tabla de contenidos
1. [Introducción](#id1)
2. [Objetivos](#id2)
3. [Preprocesamiento de señal EEG](#id3)
4. [Filtrado de señal EEG](#id4)
5. [Extracción de características de señal EEG](#id5)
6. [Discusión](#id6)
7. [Archivos de códigos](#id7)
8. [Referencias](#id8)

## **Introducción** <a name="id1"></a>
La actividad del EEG refleja la suma temporal de la actividad sincrónica de millones de neuronas corticales que están alineadas espacialmente. El EEG normal presenta una gran diversidad y una amplia variabilidad fisiológica. Las formas de onda del EEG se pueden caracterizar según su ubicación, amplitud, frecuencia, morfología, continuidad (rítmica, intermitente o continua), sincronía, simetría y reactividad. No obstante, el método más común para clasificar las formas de onda del EEG es por su frecuencia, de manera que las ondas del EEG se nombran de acuerdo a su rango de frecuencia utilizando numerales griegos. Las formas de onda más estudiadas incluyen delta (0.5 a 4Hz); theta (4 a 7Hz); alfa (8 a 12Hz); sigma (12 a 16Hz) y beta (13 a 30Hz). Además, existen otras formas de onda como las oscilaciones infralentas (ISO) (menos de 0.5Hz) y las oscilaciones de alta frecuencia (HFOs) (más de 30Hz), que están fuera del rango convencional del EEG clínico pero que han ganado importancia clínica con el desarrollo del procesamiento digital de señales [1].
<p align="center">
<img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/bf1db342-f359-443d-99f9-ee0e08b3b558" width="400"><br> 
<p align="center"><b>Figura 1: Muestras de ondas cerebrales con frecuencias dominantes pertenecientes a las bandas beta, alfa, theta y delta, así como ondas gamma [2].</b> <br> 

Las señales EEG obtenidas de los electrodos colocados en el cuero cabelludo a menudo están contaminadas por varios tipos de ruido y artefactos, como movimientos oculares, actividad muscular e interferencias eléctricas externas [3]. Por lo tanto, la preprocesamiento y filtrado son pasos cruciales en el análisis de datos de EEG para mejorar la calidad de la señal y extraer información significativa. El preprocesamiento de señales EEG incluye pasos como la re-referencia, eliminación de artefactos e interpolación de canales defectuosos. Luego, se aplica filtrado para eliminar frecuencias no deseadas, utilizando técnicas como filtrado de paso de banda, de muesca y de paso alto o bajo. Posteriormente, se extraen características de los datos EEG, analizando propiedades espectrales, temporales y de conectividad funcional mediante métodos como el análisis de Fourier y de wavelets [3]. Estas técnicas permiten capturar patrones y dinámicas subyacentes, útiles en aplicaciones como interfaces cerebro-computadora, monitoreo cognitivo y diagnóstico de trastornos neurológicos.

<p align="center">
<img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/14c111cb-5f71-459a-8f61-266c9bc21ee7" width="400"><br> 
<p align="center"><b>Figura 2: El proceso de cuatro pasos para el análisis de señales EEG. El análisis de señales EEG implica cuatro etapas: adquisición, eliminación de ruido, ingeniería de características y clasificación. [3]</b> <br> 

## **Objetivos** <a name="id2"></a>
<ul>
  <li>Comprender el proceso de preprocesamiento de señales EEG, ya que es fundamental para garantizar la calidad de los datos</li>
  <li>Analizar la importancia y aplicaciones del procesamiento de señales EEG, lo que proporciona contexto sobre su relevancia en la investigación y la práctica clínica</li>
</ul>

## **Preprocesamiento de señal EEG** <a name="id3"></a>
Se procederá a presentar el análisis de las señales preprocesadas del OpenBCI, utilizando el ultracortex. Es importante destacar que el OpenBCI guarda los datos en microvoltios (uV), por lo cual se convirtieron a milivoltios (mV). Además, la frecuencia de muestreo utilizada fue de 125 Hz. Esta frecuencia se empleó para calcular el tiempo y se trabajó con 8 canales.

### Señales obtenida del ultracortex

| Estado          | Señal                                                                                      |
|--------------------|---------------------------------------------------------------------------------------------|
| Ojos Cerrados      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/5ff1d445-799f-4b8b-b015-3a5ea5aaa8d5" width="800" height="300"></p> |
| Ojos Abiertos      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/7ef22b8f-bc86-423e-a0eb-1319090423c6" width="800" height="300"></p> |
| Ojos Cerrados      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/1052106b-2cca-4082-ac5c-fb3b9d1b5803" width="800" height="300"></p> |
| Ejercicio Matemático | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/bfb3f410-ea9b-45a2-aaa9-b4c844b44a9b" width="800" height="300"></p> |

## **Filtro de señal EEG** <a name="id4"></a>
Para esta sección, se aplicó un filtro Butterworth pasa banda con el objetivo de eliminar ruidos innecesarios. El filtro se configuró para operar en el rango de frecuencias de 8 a 30 Hz. La decisión del filtro y las frecuencias de corte se basó en el análisis de un paper seleccionado previamente. Además, se utilizó un filtro Butterworth de orden 10 para proporcionar mayores tasas de caída entre la banda de paso y la banda de parada, lo cual puede ser necesario para alcanzar los niveles requeridos de atenuación en la banda de parada o para mejorar la nitidez del corte.[4]

| Estado          | Filtrado de Señal                                                                                      |
|--------------------|---------------------------------------------------------------------------------------------|
| Ojos Cerrados      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/c6f74026-a792-40e0-9159-444f066f05b9" width="800" height="300"></p> |
| Ojos Abiertos      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/2b3a6184-c38f-4c1e-9276-9cdba0b79577" width="800" height="300"></p> |
| Ojos Cerrados      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/a8053500-4b09-4991-98dd-620c01b4ce4b" width="800" height="300"></p> |
| Ejercicio Matemático | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/9948094d-9e5f-410e-8c49-c71a750ee063" width="800" height="300"></p> |


## **Extracción de características de señal EEG** <a name="id5"></a>
Para decidir las características del EEG a extraer, nos hemos guiado por el análisis detallado de un paper seleccionado. Este estudio estableció las bandas de frecuencia de interés y la generación de espectrogramas para el promedio de los canales como componentes clave de nuestro enfoque. Estas decisiones se basan en la necesidad de comprender y representar adecuadamente las diferentes bandas de frecuencia presentes en los datos del EEG, lo cual es crucial para nuestro análisis y posterior interpretación de los resultados.[4]

En primer lugar, se presentarán las bandas del EEG del promedio de los 8 canales.

* Canal General
  
| Estado          | Bandas                                                                                      |
|--------------------|---------------------------------------------------------------------------------------------|
| Ojos Cerrados      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/b432ab51-3c97-4546-9e81-699a02c28673" width="800" height="400"></p> |
| Ojos Abiertos      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/9cfbad07-9a10-4318-8444-f318dd2381af" width="800" height="400"></p> |
| Ojos Cerrados      | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/28b05cad-95a3-4a4c-9569-d7c2fd9c754d" width="800" height="400"></p> |
| Ejercicio Matemático | <p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/992e77f4-7753-4202-9409-c5eb66214404" width="800" height="400"></p> |

Ahora se presentarán el espectograma del EEG del promedio de los 8 canales.

* Ojos cerrados
<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/7b8b91c6-125a-4bbb-b377-e1bf707c022b" width="800" height="400"></p>
* Ojos abiertos-cerrados
<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/e5835c3a-923b-4f66-b7a8-a0429fa76d9a" width="800" height="400"></p>
* Ojos cerrados (post-ejercicio)
<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/167a157b-845e-46b9-a9ef-2c77e4b2daf0" width="800" height="400"></p>
* Ejercicios matemáticos
<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/452099d7-1b2a-4a85-b3ce-9742ff7f99cd" width="800" height="400"></p>


## **Discusión** <a name="id6"></a>

* Ojos cerrados: Como se puede ver en la imagen respectiva a este ejercicio, las ondas dominantes corresponden a las ondas Alpha (8 – 13 Hz), ya que estas se relacionan con la relajación con los ojos cerrados, actuando como un puente entre la mente consciente y subconsciente. Además, hay cierta actividad en las ondas Theta (4 – 8 Hz), ya que facilitan la transición entre la vigilia y el sueño, promoviendo un estado mental tranquilo que es importante para la meditación. Por otro lado, se puede observar que hay cierta actividad en las ondas Beta (13 – 30 Hz) y Gamma (30 – 45 Hz) ya que los sentidos del tacto, oído y el olfato, así como la memoria están involucrados mientras se permanece con los ojos cerrados, además que las ondas Gamma integra la información que se reciba por medio de dichas sensaciones con la memoria. [5] En cuanto a su espectrograma se puede observar que a lo largo del tiempo hay una intensidad más constante en el rango de frecuencias de la onda Alpha, debido a que la persona se encuentra con los ojos cerrados y en un estado de relajación. También se observa actividad en el rango Beta y Gamma, debido a que la persona podría estar involucrada en algún tipo de actividad mental o procesamiento cognitivo.

* Ojos abiertos-cerrados: En las imágenes se observa una mayor actividad en las ondas Alpha, Beta y Gamma, ya que a medida que la persona pasa de estar con los ojos abiertos a tenerlos cerrados, se ven involucrados distintos procesos cerebrales. Mientras se encuentra con los ojos abiertos predominan las ondas Beta y Gamma, ya que hay una mayor interacción con los sentidos, la memoria y la percepción de la persona con su entorno. Luego, cuando pasa a estar los ojos cerrados, las ondas Alpha predominan debido a que estas se observan cuando la persona se encuentra despierta, pero con los ojos cerrados. [5] Por otro lado, en su espectrograma se puede ver una mayor uniformidad en la intensidad de las frecuencias en el transcurso del tiempo, debido a la transición de la recepción sensorial de la persona cuando pasa de tener los ojos abiertos a cerrados, ocasionando que los distintos tipos de ondas actúen en cada etapa como se mencionó anteriormente.

* Ojos cerrados (post-ejercicio): Después del ejercicio físico con los ojos cerrados, se observan patrones distintivos en las ondas EEG. Las ondas Alpha (8 – 13 Hz) pueden experimentar una disminución en su amplitud en comparación con el estado de reposo con los ojos cerrados, reflejando una persistente activación cortical y una mayor demanda de atención y procesamiento cognitivo posterior al ejercicio físico. Las ondas Theta (4 – 8 Hz) tienden a incrementarse, indicando fatiga física y mental, así como estados de relajación profunda y meditación durante la fase de recuperación post-ejercicio. Por otro lado, las ondas Beta (13 – 30 Hz) y Gamma (30 – 45 Hz) muestran un aumento de actividad, reflejando la continuación de la activación cognitiva y sensorial derivada del ejercicio físico. En el espectrograma correspondiente a este estado post-ejercicio, se observa una menor y más variable intensidad en las ondas Alpha, junto con un aumento en la intensidad de las ondas Theta, indicativo de un estado de recuperación y relajación profunda. Las ondas Beta y Gamma muestran una actividad más marcada, subrayando una mayor actividad cognitiva y sensorial durante este período de recuperación post-ejercicio. [5].
  
* Ejercicios matemáticos: Durante la realización de ejercicios matemáticos, se observa una modulación específica en las ondas EEG. Las ondas Alpha (8 – 13 Hz) tienden a disminuir, especialmente en las áreas frontales y parietales del cerebro, indicando una mayor activación y atención durante la resolución de problemas. Por otro lado, las ondas Theta (4 – 8 Hz) pueden aumentar en las regiones frontales, lo cual está asociado con la concentración y el enfoque mental requeridos para resolver problemas. Las ondas Beta (13 – 30 Hz) son predominantes durante la ejecución de tareas cognitivas como los ejercicios matemáticos, reflejando una atención sostenida y un procesamiento activo de información. Además, las ondas Gamma (30 – 45 Hz) pueden incrementarse durante la resolución de problemas complejos, indicando la integración intensa de información y una actividad cognitiva elevada. En el espectrograma correspondiente a estos ejercicios, se observa una reducción en la intensidad de las ondas Alpha, acompañada de un aumento en la actividad Theta en las áreas frontales. Las ondas Beta y Gamma se vuelven más prominentes en el espectrograma, lo cual sugiere un estado de alta actividad cognitiva y concentración durante la tarea matemática [5].

## **Archivos de códigos** <a name="id7"></a>

[Código_de_señal_EEG](https://github.com/adri201022/ISB-Grupo-11/blob/f2a7943b77a79f54b5fcee86f1809a4c94e958cb/Documentaci%C3%B3n/Laboratorios/L10_Procesamiento_EEG/Senal_EEG.ipynb)

## **Referencias** <a name="id8"></a>
  
    [1]C. S. Nayak y A. C. Anilkumar, «EEG Normal Waveforms», en StatPearls, Treasure Island (FL): StatPearls Publishing, 2024. Accedido: 18 de junio de 2024. [En línea]. Disponible en: http://www.ncbi.nlm.nih.gov/books/NBK539805/
  
    [2]P. A. Abhang, B. W. Gawali, y S. C. Mehrotra, «Chapter 2 - Technological Basics of EEG Recording and Operation of Apparatus», en Introduction to EEG- and Speech-Based Emotion Recognition, P. A. Abhang, B. W. Gawali, y S. C. Mehrotra, Eds., Academic Press, 2016, pp. 19-50. doi: 10.1016/B978-0-12-804490-2.00002-6.

    [3]A. Chaddad, Y. Wu, R. Kateb, y A. Bouridane, «Electroencephalography Signal Processing: A Comprehensive Review and Analysis of Methods and Techniques», Sensors (Basel), vol. 23, n.o 14, p. 6434, jul. 2023, doi: 10.3390/s23146434.
    
    [4]Goyal and A. Mehta, "Acquisition, pre-processing, and feature extraction of EEG," International Research Journal of Engineering and Technology (IRJET), vol. 8, no. 2, pp. 237-241, [Online]. Available: https://www.irjet.net/archives/V8/i2/IRJET-V8I237.pdf.
    
    [5]J. S. Kumar y P. Bhuvaneswari, «Analysis of Electroencephalography (EEG) Signals and Its Categorization–A Study», Procedia Engineering, vol. 38, pp. 2525-2536, ene. 2012, doi: 10.1016/j.proeng.2012.06.298. Disponible en: https://www.sciencedirect.com/science/article/pii/S1877705812022114?ref=pdf_download&fr=RR-2&rr=897035332a28287d
