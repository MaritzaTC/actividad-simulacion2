# Actividad de seguimiento - Simulación 2

|Integrante|correo|usuario github|
|---|---|---|
|Maritza Tabarez Cárdenas|maritza.tabarezc@udea.edu.co|MaritzaTC |
|Juan David Vásquez Ospina|juan.vasquez21@udea.edu.co|JuanVasquezO|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).


## Homework (Simulation)

This program, [mlfq.py](mlfq.py), allows you to see how the MLFQ scheduler presented in this chapter behaves. See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-sched-mlfq/README.md) for details.


### Questions

1. Run a few randomly-generated problems with just two jobs and two queues; compute the MLFQ execution trace for each. Make your life easier by limiting the length of each job and turning off I/Os.
    <details>
   <summary>Answer</summary>
     El comando a ejecutar es de dos colas de prioridad, con un quantum de 5 y el siguiente de 10. Los trabajos pueden permanecer 1 y 2 quantums, respectivamente, antes de degradarse(A 1,2 ). El tiempo máximo de ejecución es de 30 unidades.
       
       python mlfq.py -n 2 -Q 5,10 -A 1,2 -j 2 -m 30 -M 0 -s 42 -c
   ## Resultados
   ![question1](https://github.com/user-attachments/assets/68a99c92-5d0c-46bb-8867-a848aade80c4)
   ![question1 1](https://github.com/user-attachments/assets/3b325819-25eb-4f8b-a920-ed80cbb9e94f)
   
   ### Análisis del resultado
      - Job 0
      
         Comienza en el tiempo 0.
         
         Responde inmediatamente (response = 0), es decir, comienza su ejecución de inmediato.
         
         Turnaround = 27 (tiempo total desde que llegó hasta que terminó).
      
      - Job 1
      
         También comienza en 0 (mismo tiempo de llegada).
         
         Response = 5, lo cual indica que tuvo que esperar 5 unidades antes de ejecutarse.
         
         Turnaround = 23.
   
   </details>
   <br>

2. How would you run the scheduler to reproduce each of the examples in the chapter?
   
   <details>
   <summary>Answer</summary>
       
   ## Example 1: A Single Long-Running Job
       
    Let’s look at some examples. First, we’ll look at what happens when there
    has been a long running job in the system, with a time slice of 10 ms (and
    with the allotment set equal to the time slice). Figure 8.2 shows what
    happens to this job over time in a three-queue scheduler.
    As you can see in the example, the job enters at the highest priority
    (Q2). After a single time slice of 10 ms, the scheduler reduces the job’s
    priority by one, and thus the job is on Q1. After running at Q1 for a time
    slice, the job is finally lowered to the lowest priority in the system (Q0),
    where it remains. Pretty simple, no?
   
    ![figure 8 2](https://github.com/user-attachments/assets/3eb82f0d-2f91-4e64-b8a7-76c6a5f92d6b)

    Para reproducir el Example 1: A Single Long-Running Job del capítulo usando el simulador mlfq.py, se necesita simular
   - Un solo trabajo de larga duración.
   - Un planificador con 3 colas (-n 3).
   - Un quantum de 10 ms para cada cola (-Q 10,10,10).
   - Un allotment igual al quantum para cada cola (-A 1,1,1), es decir, el trabajo baja de prioridad después de solo 1 ejecución en cada cola.
   - Sin operaciones de (I/O) → I/O frequency = 0.

     ### Comando

         python mlfq.py -n 3 -Q 10,10,10 -A 1,1,1 -l 0,100,0 -j 1 -c

     
     ### ¿Qué se espera?
    1.  Job 0 comienza en Q2.
    2.  Baja a Q1 después de 10 ms.
    3.  Luego baja a Q0 después de otros 10 ms.
    4. Permanece en Q0 el resto del tiempo.
     
    ### Resultados:
    ![example 1](https://github.com/user-attachments/assets/1f176b19-2e90-4234-9efc-aab9ca44f308)
    ![exaple 1 1](https://github.com/user-attachments/assets/33039f4f-a5c8-4469-844a-493f30ccbd3e)

    ## Example 2: Along Came A Short Job
    Now let’s look at a more complicated example, and hopefully see how
    MLFQ tries to approximate SJF. In this example, there are two jobs: A,
    which is a long-running CPU-intensive job, and B, which is a short-running
    interactive job. Assume A has been running for some time, and then B arrives. What will happen? Will MLFQ approximate SJF for B?
    Figure 8.3 on page 5 (left) plots the results of this scenario. Job A
    (shown in black) is running along in the lowest-priority queue (as would
    any long-running CPU-intensive jobs); B (shown in gray) arrives at time
    T = 100, and thus is inserted into the highest queue; as its run-time is
    short (only 20 ms), B completes before reaching the bottom queue, in two
    time slices; then A resumes running (at low priority).
    From this example, you can hopefully understand one of the major
    goals of the algorithm: because it doesn’t know whether a job will be a
    short job or a long-running job, it first assumes it might be a short job, thus
    giving the job high priority. If it actually is a short job, it will run quickly
    and complete; if it is not a short job, it will slowly move down the queues,
    and thus soon prove itself to be a long-running more batch-like process.
    In this manner, MLFQ approximates SJF.
   
     ![8 3 1](https://github.com/user-attachments/assets/8fa3d0e4-fcdc-4ea4-be0d-655b14cac503)
 

      Para reproducir el Example 2:  Along Came A Short Job del capítulo usando el simulador mlfq.py, se necesita simular  
        - Crear dos trabajos, uno de larga duración (A) y uno de corta duración (B).
        - El trabajo A será un proceso de CPU intensivo y largo, mientras que B será interactivo y corto.
        - Utilizar 3 colas, con quantum de 10 ms, -Q 10,10,10. 
       - Un allotment igual al quantum para cada cola (-A 1,1,1), es decir, el trabajo baja de prioridad después de solo 1 ejecución en cada cola.
       - Sin operaciones de (I/O) → I/O frequency = 0.

   ### Comando

        python mlfq.py -n 3 -Q 10,10,10 -A 1,1,1 -l 0,180,0:100,20,0 -j 2 -c

   ### Resultados

   ![2 1](https://github.com/user-attachments/assets/ec348cc5-a526-45bd-abe1-6a26d48dacea)

   ![2 2](https://github.com/user-attachments/assets/4b70104f-027f-4dbd-b030-3d5ab80ca408)

    
    ## Example 3: What About I/O?
    Let’s now look at an example with some I/O. As Rule 4b states above, if a
    process gives up the processor before using up its allotment, we keep it at
    the same priority level. The intent of this rule is simple: if an interactive
    job, for example, is doing a lot of I/O (say by waiting for user input from
    the keyboard or mouse), it will relinquish the CPU before its allotment is
    complete; in such case, we don’t wish to penalize the job and thus simply
    keep it at the same level.
    Figure 8.3 (right) shows an example of how this works, with an interactive job B (shown in gray) that needs the CPU only for 1 ms before
    performing an I/O competing for the CPU with a long-running batch job
    A (shown in black). The MLFQ approach keeps B at the highest priority because B keeps releasing the CPU; if B is an interactive job, MLFQ
    further achieves its goal of running interactive jobs quickly.

    Para reproducir el Example 3:   What About I/O? del capítulo usando el simulador mlfq.py, se necesita simular  
        - Crear dos trabajos, uno de larga duración (A) y uno de corta duración (B).
        - El trabajo A será un proceso de CPU intensivo y largo, mientras que B será interactivo y corto que realiza operaciones de I/O. Este trabajo debe hacer I/O frecuentemente, lo que lo hará liberar la CPU y mantener su                 prioridad alta..
        - Utilizar 3 colas, con quantum de 10 ms, -Q 10,10,10. 
       - Un allotment igual al quantum para cada cola (-A 1,1,1), es decir, el trabajo baja de prioridad después de solo 1 ejecución en cada cola.
       - El trabajo A sin operaciones de (I/O) → I/O frequency = 0.
        - El trabajo B con operaciones de (I/O) → I/O frequency = 1.

    ![8 3 RIGHT](https://github.com/user-attachments/assets/5ed85563-f2fe-4c7d-b3cb-2ecd234f56e4)

    ### Comando

             python mlfq.py -n 3 -Q 10,10,10 -A 1,1,1 -l 0,180,0:50,20,1 -j 2 -c

   
    ### Resultados

    ![3 1](https://github.com/user-attachments/assets/2067f595-6b5a-40da-ac3d-6996c027263a)
    ![3 2](https://github.com/user-attachments/assets/5b9435a9-0e46-4246-b2e7-29a8b9bffc65)

   </details>
   <br>

4. How would you configure the scheduler parameters to behave just like a round-robin scheduler?

   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

5. Craft a workload with two jobs and scheduler parameters so that one job takes advantage of the older Rules 4a and 4b (turned on
with the -S flag) to game the scheduler and obtain 99% of the CPU over a particular time interval.

   <details>
   <summary>Answer</summary>
   
     ## Objetivo
   Crear una carga de trabajo con dos trabajos donde uno aprovecha las reglas antiguas 4a y 4b (activadas con -S) para quedarse con el 99% de la CPU.

   ### Regla 4a: 
   Si el trabajo usa completamente el quantum de tiempo, se reduce su prioridad.
   ### Regla 4b: 
   Si el trabajo entrega la CPU antes de finalizar su quantum de tiempo, mantiene el mismo nivel de prioridad.
   ### Procedimiento 
   Vamos a:
   1. Crear dos trabajos 
   -  Con la regla 4b: "bueno" que hace I/O frecuente justo antes de agotar su quantum.
   -  Con la regla 4a Uno "largo" que simplemente quiere CPU pero nunca hace I/O.
   2. Usar el flag -S para activar las Reglas antiguas.
   3. Usar -Q y -A para configurar colas con distinto quantum y allotment.
   <br>
   El caso a ejecutarse es: 
   `python mlfq.py -n 2 -Q 10,20 -A 1,2 -j 2 -l 0,1000,0:10,20,5 -S -s 1 -c`

   ## Análisis del comando
   1. -n 2 Dos niveles de cola.
   2. -Q 10,20 Quantum: 10ms para la cola 0, 20ms para la cola 1.
   3. -A 1,2  Allotment: solo un turno en cola 0 antes de degradar, y 2 turnos en cola 1.
   4. -j 2  Dos trabajos.
   5. -l 0,1000,0:10,20,5
   - Job 0 empieza en t=0, corre 1000ms sin I/O.
   - Job 1 empieza en t=10, corre 20ms y hace I/O cada 5ms.
   6. -S  Activadas las reglas antiguas 4a/4b. El trabajo no se degrada si cede CPU antes del final del quantum.
   7. -s 1 	Semilla para aleatoriedad.
   8. -c Estadísticas 

   ## ¿Qué está pasando?
   -  Job 0 empieza desde el principio y ocupa la CPU sin interrupciones.
   -  Job 1 aparece en t = 10ms, corre 20ms, pero hace I/O cada 5ms.
   -  Gracias a la regla -S, Job 1 nunca es degradado mientras siga emitiendo I/O antes de consumir completamente su quantum (10ms en cola 0).
   -  Como resultado:  Job 1 vuelve constantemente a la cola de mayor prioridad y aunque Job 0 es largo, será interrumpido frecuentemente para que Job 1 corra.

   ## Resultados
   
   ![1mldfq](https://github.com/user-attachments/assets/aa8eae57-250a-4a52-ac46-dab8684e6916)
   ![2mldfq](https://github.com/user-attachments/assets/367f2bd6-a7d0-49f1-890b-cb22cc442a02)
   
   ## ¿Qué significa?
   - Job 0 inicia primero, pero es interrumpido constantemente por Job 1, que:

   - Entra a los 10 ms.

   - Corre un ratito.

   - Hace I/O justo antes de agotar su quantum.

   - Gracias a -S, no es degradado.

   - Al volver del I/O, entra directamente a la cola de mayor prioridad.

     Esto hace que el planificador diga:

      "¡Oh! Job 1 es de alta prioridad, démosle CPU."
   
    ##  ¿Quién obtuvo el 99% de la CPU?
     - Job 1 fue quien obtuvo casi el 99% de la CPU en intervalos pequeños, aunque su tiempo total de ejecución fue bajo.
    ### ¿Por qué?
   - Porque el planificador lo favoreció repetidamente gracias a las reglas -S (4a y 4b), que impiden que Job 1 baje de nivel cuando hace I/O antes de agotar su quantum.
   - Entonces, Job 1 siempre volvía a la cola de mayor prioridad y ejecutaba inmediatamente, robando CPU constantemente a Job 0.
   - Job 1 fue el que “jugó” con el scheduler y obtuvo aproximadamente el 99% de la CPU en pequeños intervalos, gracias a las reglas 4a y 4b (-S).
   </details>
   <br>

6. Given a system with a quantum length of 10 ms in its highest queue, how often would you have to boost jobs back to the highest priority level (with the `-B` flag) in order to guarantee that a single longrunning (and potentially-starving) job gets at least 5% of the CPU?

   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

7. One question that arises in scheduling is which end of a queue to add a job that just finished I/O; the -I flag changes this behavior
for this scheduling simulator. Play around with some workloads and see if you can see the effect of this flag.

   <details>
   <summary>Answer</summary>
      
   ## Comportamiento por defecto (sin -I):
      
   Cuando un proceso termina su operación de I/O y regresa a su cola:
   - Se coloca al final de su cola actual.
   - Debe esperar su turno detrás de otros procesos ya presentes.

   ## ¿Qué hace el flag -I?
   - Los trabajos que terminan una operación de I/O son colocados al frente de su cola, no al final.
   - El proceso interactúa con el CPU de forma más inmediata, como si “interrumpiera” a los demás.
   - Esto reduce su tiempo de espera y mejora su respuesta, sobre todo si realiza I/O frecuentemente.

   ## El caso a ejecutarse es:

   ` python mlfq.py -n 2 -Q 10,20 -A 1,2 -j 2 -l 0,100,5:0,100,0 -s 2`
   
   Tiene dos trabajos:
   - Job 0: con I/O cada 5 ms.
   - Job 1: CPU-bound (sin I/O).

   Se va a probar con y sin el flag:
   1. Sin -I
      
       `python mlfq.py -n 2 -Q 10,20 -A 1,2 -j 2 -l 0,100,5:0,100,0 -s 2`
   3. Con -I
      
      `python mlfq.py -n 2 -Q 10,20 -A 1,2 -j 2 -l 0,100,5:0,100,0 -s 2 -I`

   ## ¿Qué se espera?
   1. Sin -I: el Job 0 sufrirá más al competir con el otro trabajo, ya que siempre quedará al final de la cola tras cada I/O.
   2. Con -I: el Job 0 (con I/O frecuente) tendrá menor tiempo de respuesta, menor turno de CPU, posiblemnete pueda terminar antes
  
   ## Resultados reales
   1. Sin -I
   ![6-1](https://github.com/user-attachments/assets/06212675-ef07-4dda-a5c2-fcae538061b1)
   ![6-2](https://github.com/user-attachments/assets/66ad0365-9cd8-40f5-9e32-a00e6a3c4ea2)

   2. Con -I
    ![6-3](https://github.com/user-attachments/assets/2478438a-2e06-4b04-96a7-6f7d9bc5a5f4)
   ![6-4](https://github.com/user-attachments/assets/3af9413b-281c-44ea-a742-b6c06905c6d2)

   ## ¿Qué significa esto?
    - Job 0 (el que hace I/O frecuente):
    -  Con -I termina más rápido: pasa de 265 ms a 195 ms de turnaround.

      Esto indica que recuperar el CPU más rápidamente tras I/O mejora su rendimiento general.
   
      Job 1 (CPU-bound):
      Sin -I termina antes (130 ms), pero con -I se retrasa (200 ms).
   
      ¿Por qué? Porque el Job 0 le "interrumpe" más seguido al regresar del I/O y va al frente de la cola.
        El flag -I beneficia trabajos interactivos o con muchas operaciones de I/O, ya que les permite reingresar en la CPU de inmediato, mejorando su tiempo de respuesta y turnaround.
      
      Pero esto puede perjudicar trabajos intensivos de CPU, que se ven desplazados constantemente.   
   
   </details>
   <br>

## Conclusions

Coloque aqui las conclusiones...

## Referencias 
1. https://pages.cs.wisc.edu/~remzi/OSTEP/cpu-sched-mlfq.pdf
### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
- [x] Sección con las conclusiones de los experimentos realizados.
