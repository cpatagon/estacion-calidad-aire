# 📋 Guía Práctica: Medición de WEe y AEe – Offsets Electrónicos

## 🎯 Objetivo
Medir los voltajes de offset electrónico (WEe y AEe) de una placa AFE o ISB de Alphasense **sin sensor conectado**, utilizando un multímetro común. Estas placas electrónicas se emplean junto a sensores Alphasense para convertir corriente en voltaje antes de digitalizar la señal.

---

## ⚠️ Precauciones de seguridad
- **Desconectar la alimentación** antes de conectar o desconectar componentes.
- Utilizar **protección ESD** (pulsera antiestática) cuando sea posible.
- **Verificar la polaridad** de las puntas del multímetro antes de medir.
- **No tocar** componentes expuestos con las manos durante la medición.

---

## 🛠️ Equipamiento requerido
| Ítem | Especificaciones | Notas |
| --- | --- | --- |
| **Multímetro digital** | Rango VDC: 0–2 V o 0–20 V; resolución ≤1 mV; precisión ±0,5 % | Mínimo 3,5 dígitos |
| **Fuente de alimentación** | 3,3 V o 5 V DC | Según la placa |
| **Cables de prueba** | Puntas cocodrilo o pinzas | Para conexión segura |
| **Placa AFE/ISB** | Alphasense o compatible | Sin sensor conectado; convierte corriente a voltaje |

---

## 📝 Procedimiento paso a paso

### Paso 1: Preparación del sistema
```
1. ▢ Asegurar que la placa esté APAGADA.
2. ▢ Desconectar completamente el sensor de gas.
3. ▢ Verificar que no hay cortocircuitos visibles.
4. ▢ Conectar la alimentación a la placa.
```

### Paso 2: Configuración del multímetro
```
1. ▢ Encender el multímetro.
2. ▢ Seleccionar modo "VDC" (voltaje DC).
3. ▢ Seleccionar rango "2 V DC" (preferido) o "20 V DC".
4. ▢ Verificar que muestre "0.000" en cortocircuito.
```

### Paso 3: Identificación de puntos de medición
```
┌─────────────────────┐
│    PLACA AFE/ISB    │
├─────────────────────┤
│ WE_OUT ────────○    │  ← Punta ROJA (+)
│                 │   │
│ AE_OUT ────────○    │  ← Punta ROJA (+)
│                 │   │
│ GND ───────────○    │  ← Punta NEGRA (−)
└─────────────────────┘
```

### Paso 4: Medición de WEe
```
1. ▢ Conectar punta NEGRA (−) a GND de la placa.
2. ▢ Conectar punta ROJA (+) a WE_OUT.
3. ▢ Esperar 10–15 segundos para estabilización.
4. ▢ Anotar valor estable: __________ mV ← WEe.
```

### Paso 5: Medición de AEe
```
1. ▢ Mantener punta NEGRA (−) en GND.
2. ▢ Mover punta ROJA (+) a AE_OUT.
3. ▢ Esperar 10–15 segundos para estabilización.
4. ▢ Anotar valor estable: __________ mV ← AEe.
```

---

## 📊 Valores esperados y diagnóstico

### Rango normal
| Parámetro | Valor esperado | Observaciones |
| --- | --- | --- |
| **WEe** | 200–300 mV | Valor típico Alphasense |
| **AEe** | 200–300 mV | Valor típico Alphasense |
| **Diferencia WEe−AEe** | ±50 mV | Normalmente similares |

### Diagnóstico de problemas
| Escenario | Valores medidos | Diagnóstico | Acción |
| --- | --- | --- | --- |
| ✅ Normal | WEe = 247 mV, AEe = 253 mV | Placa OK | Seguir con uso |
| ❌ Cero | WEe ≈ 2 mV, AEe ≈ 1 mV | Sin alimentación o medición incorrecta | Revisar fuente |
| ❌ Muy alto | WEe ≈ 850 mV, AEe ≈ 810 mV | Daño en componentes | Revisar placa |
| ❌ Inestable | Fluctúa ±20 mV | Ruido o mala conexión | Verificar tierra |
| ❌ Negativos | WEe = −245 mV, AEe = −251 mV | Polaridad invertida | Revisar diseño |

---

## 🧮 Ejemplo de registro
**Fecha:** ____________________  
**Operador:** ____________________  
**Placa:** AFE-12345  
**Multímetro:** Fluke 17B+

| Parámetro | Valor medido | Estado |
| --- | --- | --- |
| **WEe** | 248 mV | ✅ OK |
| **AEe** | 255 mV | ✅ OK |
| **Diferencia** | 7 mV | ✅ OK |
| **Temperatura ambiente** | 23 °C | — |

**Observaciones:** Valores dentro del rango esperado. Placa en condiciones normales.

---

## 💾 Uso de los valores

### En código (Arduino)
```cpp
// Offsets electrónicos medidos
const float WE_ELECTRONIC_OFFSET = 248.0;  // mV
const float AE_ELECTRONIC_OFFSET = 255.0;  // mV

float readCorrectedWE() {
    float raw_we = readADC(WE_CHANNEL);
    return raw_we - WE_ELECTRONIC_OFFSET;
}
```

### En configuración
```json
{
    "board_calibration": {
        "we_electronic_offset": 248.0,
        "ae_electronic_offset": 255.0,
        "measurement_date": "2024-01-15",
        "temperature": 23.0
    }
}
```

---

## 🔄 Verificación periódica
| Frecuencia | Acción | Criterio |
| --- | --- | --- |
| Inicial | Medición completa | 200–300 mV |
| Cada 6 meses | Verificación | Cambio < ±10 mV |
| Después de impactos | Verificación inmediata | Valores estables |

---

## ❓ Preguntas frecuentes
**¿Los offsets cambian con el tiempo?**  
Normalmente son estables, pero pueden variar con temperatura extrema o daños físicos.

**¿Qué hago si mis valores están fuera de rango?**  
Contactar al fabricante o revisar la placa por daños.

**¿Puedo ajustar estos offsets a cero?**  
No es recomendable; se compensan en software.

---

## ✅ Checklist final
- [ ] Multímetro calibrado y funcionando.
- [ ] Sensor desconectado.
- [ ] Puntas de prueba en buen estado.
- [ ] Valores anotados correctamente.
- [ ] Valores dentro de 200–300 mV.
- [ ] Documentación actualizada.

**¡Medición completada!** 🎉

---

*Documento versión 1.0 – Basado en AAN-803-05 de Alphasense.*  
*Última actualización: enero 2024.*
