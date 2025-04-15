DOCUMENTACIÓN TÉCNICA CRYPTO-QR
Versión: 2.3
Fecha: [Fecha Actual]


1. DESCRIPCIÓN GENERAL
Sistema de encriptación cliente-side que combina:

Algoritmo híbrido: Compresión LZ77 + XOR con derivación de clave

Esteganografía: Ocultación de datos en imágenes de ruido

Multiplataforma: Funciona en navegadores y entornos JS



2. ESPECIFICACIONES TÉCNICAS
2.1 Algoritmo de Encriptación
Copy
1. Compresión: LZString → Uint8Array  
2. Generación Salt: 16 bytes (crypto.getRandomValues)  
3. Derivación de Clave: keyByte + saltByte + (índice mod 256)  
4. Cifrado XOR: byte ^ derivedByte  
5. Ofuscación: Intercalado de bytes aleatorios (33-126 ASCII)  
6. Codificación: Base64 URL-safe  
2.2 Parámetros Clave
Componente	Valor
Tamaño Salt	16 bytes
Checksum	Suma bytes mod 256
Capacidad QR	1,200 bytes
Tiempo encriptación	~5ms/KB (Chrome 120+)


3. FLUJO DE OPERACIÓN
3.1 Encriptación
plaintext
Copy
Texto → Compresión → Salt → XOR → Ofuscación → Base64 → (QR/Imagen)
3.2 Desencriptación
plaintext
Copy
Base64 → Decodificación → Filtrado → XOR → Descompresión → Texto


4. VERSIONES
Función	            Full (23KB) 	Lite (10KB)	Texto (5KB)
Encriptación texto	✓	      ✓	✓
Generación QR	        ✓	      ✓	✗
Imagen de ruido	              ✓	✗	✗
Desencriptar imagen	✓	      ✗	✗


5. USO OFFLINE
Copy
1. Requiere:  
   - qrcode.min.js (local)  
   - lz-string.min.js (local)  
2. Eliminar CDNs externos  
3. Servir desde file:// o localhost  


6. EJEMPLO DE CONFIGURACIÓN
javascript
Copy
const config = {  
    saltLength: 24,        // Bytes  
    obfuscation: 3,       // Nivel 1-5  
    noiseType: 'perlin',  // 'random'|'perlin'  
    qrErrorCorrection: 'H' // L|M|Q|H  
};  


7. CASOS DE USO
✔ Transferencia air-gapped vía QR

✔ Almacenamiento cifrado en medios físicos

✔ Protección contra análisis forense

✖ No apto para datos médicos/bancarios



8. LIMITACIONES
Máximo 1MB datos en versión Full

No resiste ataques de fuerza bruta avanzados

Depende de APIs web para generación de números aleatorios
