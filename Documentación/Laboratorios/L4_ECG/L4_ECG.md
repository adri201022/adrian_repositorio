# Informe de Laboratorio 4: Uso de BiTalino para ECG

## Integrantes
- Adrian Alberto Gutierrez Gonzales (adrian.gutierrez@upch.pe)
- Micaela Ivy Horny Insua (micaela.horny@upch.pe)
- Gian Pierre Santivañez Condor (gian.santivanez@upch.pe)
- Rodrigo Alfonso Gonzales Cabrera (rodrigo.gonzales@upch.pe)

## Tabla de contenidos
1. [Introducción](#id1)
2. [Objetivos](#id2)
3. [Materiales y equipos](#id3)
4. [Fotos de conexión usada](#id4)
5. [Videos de la señal](#id5)
6. [Ploteo de la señal en OpenSignals](#id6)
7. [Ploteo de la señal en python](#id7)
8. [Resumen y explicación de la señal ploteada](#id8)
9. [Archivo de los datos de la señal ploteada](#id9)
10. [Códido del ploteo de la señal en Python](#id10)
11. [Referencias](#id11)

## **Introducción** <a name="id1"></a>

### Una visión fisiológica del corazón
<p style="text-align: justify;">
  
El corazón realiza dos funciones principales: bombea sangre rica en oxígeno desde la aurícula izquierda hasta la aorta y el resto del cuerpo, y bombea sangre pobre en oxígeno desde la aurícula derecha hacia los pulmones. Estas acciones se deben a las contracciones regulares y repetitivas del músculo cardíaco, que son generadas por señales eléctricas conocidas como potenciales de acción. La actividad eléctrica del corazón comienza en el nódulo sinoauricular (SA), ubicado en la aurícula derecha, también llamado marcapasos del corazón. Desde aquí, las señales se propagan al nódulo atrioventricular (AV) y luego a través de las ramas del haz de His y las fibras de Purkinje, lo que permite la activación coordinada de las células ventriculares para bombear sangre eficientemente [1]. 

</p>

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/00483b35-750f-4cef-a4b2-3d545925a5cf" width="400" height="300"> </p>
<p align="center"> <small>Figura 1: Ciclo cardíaco: Zonas del corazón [2].</small> </p>

<p style="text-align: justify;">
  
El ciclo cardíaco incluye dos fases principales: la diástole, en la que las cámaras cardíacas se llenan de sangre, y la sístole, durante la cual se contraen las cámaras cardíacas para bombear la sangre fuera del corazón. Durante este proceso, se generan cambios eléctricos que pueden ser detectados en la superficie de la piel mediante un electrocardiograma (ECG). Cada parte del corazón tiene tiempos específicos para la excitación y propagación de las señales eléctricas, lo que influye en los patrones observados en el ECG [3].
  
</p>

### Adquisición de un ECG
Cuando una onda de despolarización se desplaza hacia un electrodo, se registra como una elevación positiva, mientras que cuando se aleja, se registra como una deflexión negativa. La amplitud de una onda se refiere a la magnitud de esta deflexión, medida en milímetros. Un electrocardiograma (ECG) básico incluye seis derivaciones primarias, obtenidas con electrodos en los brazos y las piernas. Las derivaciones cardíacas registran la diferencia de potencial eléctrico entre dos puntos, ya sea entre dos electrodos (derivaciones bipolares) o entre un punto virtual y un electrodo (derivaciones monopolares). Las derivaciones bipolares incluyen I(brazo derecho–brazo izquierdo), II(brazo derecho–pierna izquierda) y III(pierna izquierda–brazo izquierdo), que forman el triángulo de Einthoven. Según la Ley de Einthoven, la suma de las derivaciones II y III es igual a la derivación I. Las derivaciones unipolares incluyen aVR(brazo derecho), aVL(brazo izquierdo) y aVF(pierna izquierda), que parten de un electrodo positivo central hacia los extremos. Estas derivaciones amplifican el voltaje de las extremidades. Finalmente, las derivaciones precordiales incluyen V1 a V6, ubicadas en puntos específicos del tórax para registrar la actividad eléctrica del corazón desde diferentes ángulos [4].

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/f09c100a-3ac1-47a6-ab35-cef6be7e55ce" width="400" height="300"> </p>
<p align="center"> <small>Figura 2: Los sitios de colocación estándar para las derivaciones precordiales [4].</small> </p>

La **onda P** es una pequeña deflexión que representa la despolarización auricular. El **intervalo PR** es el tiempo entre la primera deflexión de la onda P y la primera deflexión del complejo QRS. El **complejo QRS** representa la despolarización ventricular. Para los no experimentados, uno de los aspectos más confusos de la lectura del ECG es la etiqueta de estas ondas. La regla es: si la onda inmediatamente después de la onda P es una deflexión ascendente, es una onda R; si es una deflexión descendente, es una onda Q. Las **ondas Q** pequeñas corresponden a la despolarización del septum interventricular. Las ondas Q también pueden relacionarse con la respiración y generalmente son pequeñas y delgadas. También pueden indicar un infarto de miocardio antiguo (en cuyo caso son grandes y anchas). La **onda R** refleja la despolarización de la masa principal de los ventrículos, por lo tanto, es la onda más grande. La **onda S** significa la despolarización final de los ventrículos, en la base del corazón. El **segmento ST**, también conocido como intervalo ST, es el tiempo entre el final del complejo QRS y el inicio de la onda T. Refleja el período de potencial cero entre la despolarización y la repolarización ventricular. Las **ondas T** representan la repolarización ventricular (la repolarización auricular está oscurecida por el complejo QRS) [4].

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/61681b8b-8030-4764-ad36-36711f3bc43b" width="400" height="300"> </p>
<p align="center"> <small>Figura 3: El patrón básico de actividad eléctrica en todo el corazón. [4].</small> </p>

### Aplicaciones
<p style="text-align: justify;">
  
En el contexto de las aplicaciones médicas, la señal del electrocardiograma (ECG) juega un papel crucial en la identificación de anomalías, como la elevación del segmento ST, donde la señal no regresa a su línea base después del complejo QRS. Esta observación nos proporciona indicios sobre posibles obstrucciones en el suministro de oxígeno a los tejidos del corazón, lo que podría desencadenar un ataque cardíaco. Otro factor relevante es la variabilidad de la frecuencia cardíaca (VFC), ya que puede ofrecer información sobre la salud general del corazón y puede evaluarse mediante el análisis de los intervalos entre los picos R [2].

</p>

## **Objetivos** <a name="id2"></a>
- Adquirir señales biomédicas de ECG.
- Hacer una correcta configuración de BiTalino
- Extraer la información de las señales ECG del software OpenSignals (r)evolution

## **Materiales y equipos** <a name="id3"></a>
<div align="center">

|  **Modelo**  | **Descripción** | **Cantidad** |
|:------------:|:---------------:|:------------:|
| (R)EVOLUTION |   Kit BITalino  |       1      |
|       -      |      Laptop     |       1      |
| Fluke Biomedical ProSim 4 | Vital Signs Simulator | 1 |

</div>

## **Fotos de conexión usada** <a name="id4"></a>
<p style="text-align: justify;">
  
La colocación de los electrodos en el usuario de prueba se realiza conforme a la Guía de Inicio BITalino (r)evolution en casa para Electrocardiografía (ECG), diseñada para familiarizarse con las bioseñales específicas de ECG [2]. En las siguientes imágenes se pueden observar las colocaciones electrodos-cuerpo para la derivación I: IN+(rojo) & IN-(negro) en las muñecas y REF(blanco) en la cresta ilíaca. Adicionalmente, se observan las colocaciones BITalino-cables, al igual que electrodos-simulador.

</p>

<div align="center">

|  **Figura 4: Prueba con usuario**  | **Figura 5: Prueba con simulador de paro cardíaco** 
|:------------:|:---------------:|
|<p><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/925752c6-bd24-4f0f-a469-47593b03ce60" width="60%" height="60%"></p>|<p><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/9c875430-87de-4aeb-b7bb-d25da83601b4" width="60%" height="60%"></p>|

</div>

## **Videos de la señal** <a name="id5"></a>

### **Prueba con usuario**
<div align="center">

| **Antes de actividad (reposo)** | **Durante respiraciones rápidas** | **Durante actividad física** | **Después de actividad física** |
|:--------------------------------:|:---------------------------------:|:----------------------------:|:--------------------------------:|
| <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/5e95f0c3-f1d3-41bc-9e2d-58d7d24cf8b4"></video> | <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/5d88b39e-38da-43da-a8d6-4046aef8e5ed"></video> | <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/633e99b8-0350-4416-a4f2-8daf8bea16ef"></video> | <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/03ed0916-50cb-4b50-b88d-55b8196b068e"></video> |

</div>

### **Prueba con simulador de paro cardíaco**
<div align="center">

|  **CVP (VI)**  | **Taq. vent. 160 lpm** | **Fib. vent. severa** | **Asistolia** |
|:------------:|:---------------:|:------------:|:------------:|
| <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/6635b400-6426-44e2-ae86-d348da1f3887"></video> | <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/a48faa9a-92a5-4887-b891-336287c95599"></video> | <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/d2614676-a40e-4211-8260-6377a6172635"></video> | <video src="https://github.com/adri201022/ISB-Grupo-11/assets/164538327/9eec286f-0f4e-4b33-a9e2-5221e1e80f8c"></video> |

</div>

## **Ploteo de la señal en OpenSignals** <a name="id6"></a>
En esta sección presentaremos los gráficos de los datos recopilados mediante el uso de BITalino y OpenSignals.

* Antes de actividad (reposo):

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/0640e803-0752-4684-b347-e05737b1e874" width="700" height="350"> </p>
<p align="center"> <small>Figura 6: Ploteo de los datos en reposo.</small> </p>

* Durante respiraciones rápidas:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/53855182-b069-4fb8-abd2-af3e96019c27" width="700" height="350"> </p>
<p align="center"> <small>Figura 7: Ploteo de los datos durante respiraciones rápidas.</small> </p>

* Durante actividad física:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/8940821e-8eb5-408f-89b8-9582e3474791" width="700" height="350"> </p>
<p align="center"> <small>Figura 8: Ploteo de los datos durante actividad física.</small> </p>

* Despues de actividad física:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/579accd9-b6ea-4540-99f9-23cd08f1ebe9" width="700" height="350"> </p>
<p align="center"> <small>Figura 9: Ploteo de los datos despues de actividad física.</small> </p>

### Simulación de paro cardíaco
* CVP:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/c483d4c0-6a30-41af-b52c-e86847b17718" width="700" height="350"> </p>
<p align="center"> <small>Figura 10: Ploteo de los datos durante CVP.</small> </p>

* Taquicardia ventricular:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/b710cdcd-befa-4c41-9abc-b3fbb7cf95ee" width="700" height="350"> </p>
<p align="center"> <small>Figura 11: Ploteo de los datos durante taquicardia ventricular.</small> </p>

* Fibrilacion auricular:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/e844e987-2361-4d3e-8e15-76cbf404625a" width="700" height="350"> </p>
<p align="center"> <small>Figura 12: Ploteo de los datos durante fibrilación auricular.</small> </p>

* Asistolia

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/100977549/558dfba9-42a2-4728-8add-3f933c024d2c" width="700" height="350"> </p>
<p align="center"> <small>Figura 13: Ploteo de los datos durante asistolia.</small> </p>

## **Ploteo de la señal en Python** <a name="id7"></a>
Se presentará las imágenes que fueron ploteadas en python, además algunas de las imágenes se ha agarrado un intervalo de timempo para que se aprecie mejor la señal.

* Antes de actividad (reposo):

Durante este periodo, es común observar un ritmo cardiaco regular y estable, con ondas P, complejos QRS e intervales QT normales. Se observa el complejo QRS el cual indica un tiempo corto de despolarización de los ventrículos, lo que indicaría un corto  periodo de movimiento dentro de sas cavidades. Vemos ruido en la región ST y también una rápida frecuencia entre cada señal, considerando que esta medición es antes de la actividad y estamos en un estado de reposo [6]

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/e16b9097-ce55-4653-aa6b-9a8f3ab9b6ea" width="700" height="350"> </p>

<p align="center"> <small>Figura 6: Ploteo de los datos en reposo.</small> </p>

* Durante respiraciones rápidas:

Durante las respiraciones rápidas es posible observar cambios respecto al estado en reposo, esto puede deberse a la influencia del sistema nervioso autónomo en la regulación del ritmo cardíaco durante la respiración. La inspiración profunda puede generar cambios tanto en la posición como en la orientación del corazón a través de un desplazamiento hacia abajo del diafragma que afectan la señal de ECG. Estos se pueden observar en el gráfica en un intervalo de tiempo de 16 y 18 segundos [8]

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/49984312-6bdd-4135-96aa-13a36cf118a8" width="700" height="350"> </p>

<p align="center"> <small>Figura 7: Ploteo de los datos durante respiraciones rápidas.</small> </p>

* Durante actividad física:

Vemos un gran incremento en la frecuencia y ruido en general de la señal, varias ondas que antes tenían la forma más plana con el intervalos ST ahora presentan una distorsión en su señal que sospechamos que se debe que al ser señales más chicas, fueron muy afectadas directamente por el movimiento de la actividad física ya que antes de los trotes realizados también se probaron unos saltos pequeños.

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/02eff7fe-7e42-409d-ae1c-6de8a0dcf353" width="700" height="350"> </p>

<p align="center"> <small>Figura 8: Ploteo de los datos durante actividad física.</small> </p>

* Despues de actividad física:

El aumento de frecuencia cardiaca debido al ejercicio físico, en este caso trotes ligeros al rededor y durante el sitio desencadena cambios en la señal ECG, al principio fueron un poco erráticos los valores por la sensibilidad de los electrodos que parece que perdierorn algo de contacto pero ya al ajustarlos y hacer la meición se vió como una mayor amplitud de la onda Q, disminución del intervalo RR, ondas T altas y puntiagudas, superposición de ondas P y ondas T [9]

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/23161984-9c01-48ad-92fd-e5bab3cad153" width="700" height="350"> </p>

<p align="center"> <small>Figura 9: Ploteo de los datos despues de actividad física.</small> </p>

### Simulación de paro cardíaco

Para la simulación de este evento en clase se empleó un simulador de ECG donde con una autosecuencia de 5 etapas se simuló el paro cardiaco, la primera etapa consiste en un ECG de 80lpm durante 45 seg esto corresponde a una pequeña bradicardia, posteriormente se pasó a un CVP (contracciones ventriculares prematuras) esto ocurre cuando el latido nace de un foco ectópico del ventrículo [10]. Esto durante 30 segundos

Las CVP constituye n las arritmias más frecuentes, pudiendo observarse en el ECG en un 5% y en el Holter de 24 horas en un 50% de los pacientes sanos. Si existe cardiopatía previa estas cifras aumentan a un 10% en el ECG y a un 60-90% en el Holter. Las características morfológicas de la CVP, su asociación entre sí y con el ritmo base, permiten realizar una clasificación de las mismas y estimar el grado de peligrosidad que presentan [11]. El paciente puede notar estos latidos como un vuelco de corazón, una sensación de vacío o un golpeteo en el pecho, aunque muchas veces su hallazgo es casual y no tiene importancia [12]. 

Luego, se continua con una taquicardia ventricular a 160lpm por durante 30 segundos. En las 
últimas etapas de este paro cardiaco pasamos a una fibrilación ventricular severa a 30 segundos y finalizamos con una asistolia durante 15 seg lo cuál indica que el paciente ha sufrido un paro cardiaco.

* CVP:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/a3553e02-6599-41cd-a032-842778f5ee47" width="700" height="350"> </p>

<p align="center"> <small>Figura 10: Ploteo de los datos durante CVP.</small> </p>

* Taquicardia ventricular:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/3b841fff-ec1b-4843-80db-eb0a91695968" width="700" height="350"> </p>

<p align="center"> <small>Figura 11: Ploteo de los datos durante taquicardia ventricular.</small> </p>

* Fibrilacion auricular:

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/f002c740-a0bb-49f5-9fc9-3a3938e00ac2" width="700" height="350"> </p>

<p align="center"> <small>Figura 12: Ploteo de los datos durante fibrilación auricular.</small> </p>

* Asistolia

<p align="center"><img src="https://github.com/adri201022/ISB-Grupo-11/assets/164541653/d4499fc8-bc25-41fb-b05b-b1340cb2596b" width="700" height="350"> </p>

<p align="center"> <small>Figura 13: Ploteo de los datos durante asistolia.</small> </p>


## **Resumen y explicación de la señal ploteada** <a name="id8"></a>

El complejo QRS representa la despolarización (activación) de los ventrículos. Siempre se le conoce como "complejo QRS", aunque es posible que no siempre muestre 3 ondas. Dando que el vector eléctrico generado por el ventríoculo izquierdo es muchas veces mayor que el vector generado por el ventrículo derecho, el complejo QRS es en realidad un reflejo de la despolarización del ventrículo izquierdo [5]. Para la explicación de las señales ploteadas debemos recordar la forma que tiene un electrocardiograma normal el cuál se puede interpretar siguiendo una secuencia de pasos: Ritmo (sinusal o no sinusal), frecuencia cardiaca (en latidos por minuto, lpm), eje cardiacao (en grados), análisis de ondas, segmentos e intervalos, búsqueda de anormalidades. Podemos recordar esto con la memotecnia de FRESA [7]. 

Estado de reposo: Se puede observar que la grafica obtenida de la simulacion y la parte experimental son parecidas ,por lo que se estima que el rango del valor del la frecuencia cardiaca de la persona estudiada esta en el rango normal de 60 a 100 lpm,el cual es el valor de una persona sana.Asi mismo el desempeño de la tarea el ritmo cardiaco se regularizaba significativamente de acuerdo a los valores obtenidos en condiciones de reposo [6].

Estado durante actividad física: Durante el ejercicio se ve alterada la frecuencia en la respiración, esto afecta a la morfología del latido la cuál está influenciada por 2 mecanismos, los cambios en la impedancia toráxica y la orientación del eje electrónico del corazón con respecto a los electrodos ECG, por la frecuencia cardiaca está en el rango de 60-100 lpm y la frecuencia respiratoria está en el rango de 15-60 respiraciones por minuto. Se ve un descenso en los picos que corresponden cuando la persona toma aire .

Esado posterior a la actividad física: En este caso las frecuencia cardiacas de interés son la frecuencia cardiaca máxima y la frecuencia cardiaca mínima,ya que se puede corroborar con la primera el reflejo del trabajo anaeróbico producido por el ejercicio realizado.De igual forma ocurre con la frecuencia mínima, ya que está debe estar en los valores del intervalo de recuperación del estado basal [9].

## **Archivo de los datos de la señal ploteada** <a name="id9"></a>

* [Datos de las pruebas de ECG (.txt)](https://github.com/adri201022/ISB-Grupo-11/tree/main/Documentación/Laboratorios/L4_ECG/ECG_data)

## **Códido del ploteo de la señal en Python** <a name="id10"></a>
- [Señal ploteada en python](https://github.com/adri201022/ISB-Grupo-11/blob/ed91474541181bd8891bdb22316e75ba2e4bdda4/Documentaci%C3%B3n/Laboratorios/L4_ECG/Ploteo_senales.py)
- [Señal ploteada en python de la secuencia de paro cardiaco](https://github.com/adri201022/ISB-Grupo-11/blob/30f67bbcfc21a6c5a8ed1e39f236889d8ee5f35c/Documentaci%C3%B3n/Laboratorios/L4_ECG/Plot_secuencia_paro.py)

## **Referencias** <a name="id11"></a>
[1] R. Rhoades and D. R. Bell, Eds., Medical Physiology: Principles for Clinical Medicine, 6th ed, R. A. Rhoades and D. R. Bell, Eds. Lippincott Williams & Wilkins, 2022.

[2] M. Proença and K. Mrotzeck, vol. 2021. BITalino (r)Evolution Lab Guide, Home Guide 2—ECG. Available at: https://support.pluxbiosignals.com/wp-content/uploads/2022/04/HomeGuide2_ECG.pdf

[3] J. G. Betts et al., 2013. Anatomy and Physiology. Houston, Texas: OpenStax. Available at: https://openstax.org/books/anatomy-and-physiology/pages/1-introduction

[4] E. A. Ashley and J. Niebauer. Chapter 3, Conquering the ECG, Cardiology Explained. London: Remedica, 2004. Available at: https://www.ncbi.nlm.nih.gov/books/NBK2214/

[5] Cascino, T., & Shea, M. J. (2021, 6 de julio). Electrocardiografía - Trastornos del corazón y los vasos sanguíneos - Manual MSD versión para público general. Manual MSD versión para público general. https://www.msdmanuals.com/es-es/hogar/trastornos-del-corazón-y-los-vasos-sanguíneos/diagnóstico-de-las-enfermedades-cardiovasculares/electrocardiografía#:~:text=En%20un%20ECG%20pueden%20observarse,las%20pareces%20del%20músculo%20cardíaco.

[6] Guardado R. Vallín D., (2009, 2 de junio). Aplicación del Análisis Tiempo-Frecuencia en Electrocardiografía.https://laccei.org/LACCEI2009-Venezuela/Papers/IT117_GuardadoMedina.pdf

[7] “Taller de interpretación del electrocardiograma. | FISIOLOGÍA”. FISIOLOGÍA | DEPARTAMENTO DE FISIOLOGÍA. Accedido el 21 de abril de 2024. [En línea]. Available at: https://fisiologia.facmed.unam.mx/index.php/taller-de-interpretacion-del-electrocardiograma/

[8] S. Kurisu, K. Nitta, Y. Sumimoto, H. Ikenaga, K. Ishibashi, Y. Fukuda, and Y. Kihara, “Effects of deep inspiration on QRS axis, T-wave axis and frontal QRS-T angle in the routine electrocardiogram,” Heart and Vessels, vol. 34, no. 9, pp. 1519–1523, 2019.

[9] G. P. Whyte and S. Sharma, Practical ECG for Exercise Science and Sports Medicine. Champaign, IL: Human Kinetics, 2010.

[10] “Pagina nueva 1”. SEEUE - Sociedad Española de Enfermería de Urgencias y Emergencias. Accedido el 21 de abril de 2024. [En línea]. Disponible: https://www.enfermeriadeurgencias.com/ciber/PRIMERA_EPOCA/2006/octubre/contraccionesventriculares.htm

[11] Docherty B, Douglas M. Cardiac care: 3. ECG interpretation - ventricular arrhythmias. Prof Nurse. 2003 Apr;18(8):459-61.

[12] Moreno Gómez R, García Fernández A: Electrocardiografía. Madrid: McGraw-Hill, 1999:55.
