# Calidad de nuestras pruebas

[Como instalar Mutalk](https://fiubaar-my.sharepoint.com/:v:/g/personal/disanchez_fi_uba_ar/EfkfHHGhfCVJq_Dbkc8yqoEBO6WofQbASujaDr0JKc0BTw?e=Q2CaYz)

[Calidad de nuestras prueba - explicación](https://fiubaar-my.sharepoint.com/:v:/g/personal/disanchez_fi_uba_ar/EVSrPXvRRiNImBWb1MBtrWoB8OFs9m8_PDzCHJ7hQ_Qnyg?e=by5GlI)

## Introducción

Nuestro objetivo será definir un conjunto de criterios, iniciales, acerca de la calidad de las pruebas que escribimos. Comenzaremos haciéndonos las siguientes preguntas...

Preguntas:
- Cuando el conjunto de pruebas que hemos escrito tiene calidad? 
- Podemos dar una respuesta formal a este interrogante?
- Cuáles herramientas técnicas y/o prácticas pueden ayudarnos a calificar la calidad de las pruebas?

Diremos que un conjunto de pruebas guarda un **mínimo de calidad** si:

- **SUT** es un *caso de uso*. 
- El porcentaje de **cobertura** del código en ejercicio es mayor a %T.
- Ningún mutante sobrevive a las **pruebas de mutación**.

La estructura Arrange Act Assert no figura en la lista, ya que se verifica a través de las pruebas de mutación. 

**IMPORTANTE:**

Los criterios mencionados anteriormente no implican que el desarrollador deba omitir escribir pruebas específicas para probar condiciones de borde específicas. 

### Acerca de los requisitos del problema

Para nuestro análisis utilizaremos un problema cuyos requisitos están descritos por un conjunto de pruebas. Estas pruebas guardan una estructura AAA (Arrage, Act y Assert). Ver archivo de [requisitos](tests/TP1-Requeriments-Tests.st)

## SUT (system under test)

Tomaremos como unidad de prueba a un caso de uso. Esta estratégia reduce el número de pruebas que serán afectadas durante el proceso de refactor. Tomar el SUT una clase podrá resultar en la necesidad de eliminar/actualizar pruebas de esta clase, además de no brindar una funcionalidad que contribuya a cumplir con el caso de uso. 

## Cobertura

La herramienta de cobertura provista por el *Test runner* de [Pharo](https://pharo.org/) solo mide que el método haya sido invocado. Tener baja cobertura implica que existen clases que son son ejercitadas por ninguna de las pruebas. 

## Pruebas de mutación

Las pruebas de mutación toman un conjunto de pruebas y un código que estas pruebas ejercitan. Aplicando alteraciones al código bajo ejercicio, se crean *mutantes*. Un mutante el código original + una variación. Tomando este código *mutante* se le aplica el conjunto de pruebas. Si el conjunto de pruebas fueron ejecutadas sin reportar una falla (todas fueron verdes) decimos que el mutante sobrevivió y el conjunto de pruebas es **defectuoso**. 

La herramienta que vamos a utilizar para ejecutar estas pruebas de mutación será: [MuTalk](https://github.com/pavel-krivanek/mutalk).

Para ejecutar las pruebas de mutación y obtener el reporte necesitamos:

- Listar el conjunto de pruebas (que ejercitan a los mutantes): (1) `AlgoRemisTest`, (2) `TestValorPeajeEscalonado` y (3) `TestDestino`.
- Listar el conjunto de clases que serán mutadas: `AlgoRemis`, `Chofer`, etc.

```smalltalk
| testSuit analysis alive browser |
testSuit := {
	AlgoRemisTest.
	TestValorPeajeEscalonado .
	TestDestino.
}.
classesToMutate := { AlgoRemis  .
	Chofer.
	ChoferComun .
	ChoferElectrico .
	DescuentoHospital .
	SinDescuento  .
	Destino .
	Viaje .
	ValorPeajeEscalonado .
	ValorEstandard .
}.
analysis := MutationTestingAnalysis
    testCasesFrom: testSuit
    mutating: classesToMutate 
    using: MutantOperator contents
    with: AllTestsMethodsRunningMutantEvaluationStrategy new.
analysis run.
alive := analysis generalResult aliveMutants.

"Display result in Glamorous Browser"
browser := GLMTabulator new.
browser 
	row: #results;
	row: #diff.
browser transmit to: #results.
browser transmit to: #diff; from: #results; andShow: [ :a | 
	a diff display: [ :mutant | {((RBParser parseMethod: (mutant mutant originalSource)) formattedCode) . ((RBParser parseMethod: (mutant mutant modifiedSource)) formattedCode)}] ].
browser openOn: alive.
```
