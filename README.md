# Test quality

Para tener una cota mínima de calidad de pruebas no es suficiente medir cobertura de lineas, solamente. 

Estamos evaluando, alterar el código bajo ejercicio y analizar si existen (luego de alterarlo) pruebas que permacen en "verde"

## Coverage

[WiP]

## Mutation tests

### Prueba 1 

Al mutar `Persona` el test `test09AgregoUnaPersonaDeRiesgoYNoCircula` lanza `exception` procediendo los tests pasan. El comportamiento es el esperado y las mutaciones sobre `Persona` no produjeron mutantes.

[src](tests/test_01/tp1-000001.st)
