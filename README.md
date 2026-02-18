# Stream Cipher - Laboratorio de Criptografía
##  Descripción del Proyecto

Este proyecto implementa un sistema básico de cifrado de flujo que demuestra los conceptos fundamentales de la criptografía simétrica. El sistema utiliza la operación XOR entre un mensaje en texto plano y un keystream generado pseudoaleatoriamente a partir de una clave secreta.

### Componentes Principales

1. **Generación de Keystream**: Función que genera una secuencia pseudoaleatoria de bytes usando `random.seed()` de Python.
2. **Función de Cifrado**: Aplica XOR bit a bit entre el mensaje y el keystream.
3. **Función de Descifrado**: Recupera el mensaje original aplicando XOR con el mismo keystream.

##  Instalación y Uso

### Requisitos

- Python 3.7 o superior
- Jupyter Notebook

### Instalación

1. Clonar el repositorio:
```bash
git clone https://github.com/JavierC22153/Stream-Cipher.git
cd Stream-Cipher
```

2. (Opcional) Crear un entorno virtual:
```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

3. Instalar Jupyter Notebook (si no lo tienes):
```bash
pip install jupyter
```

### Ejecución

1. Iniciar Jupyter Notebook:
```bash
jupyter notebook
```

2. Abrir el archivo `Stream-Cipher.ipynb` en el navegador

3. Ejecutar las celdas en orden secuencial (Shift + Enter)


## Ejemplos de Ejecución

### Ejemplo 1

```
Texto plano original:
  'Stream-Cipher'

Clave utilizada:
  'Tarea'

Texto cifrado (hexadecimal):
  e1bedcb354e855363bc8000b0a

Texto cifrado (base64):
  4b7cs1ToVTY7yAALCg==

Texto descifrado:
  'Stream-Cipher'
```

### Ejemplo 2

```
Texto plano original:
  'Javier Chen'

Clave utilizada:
  '22153'

Texto cifrado (hexadecimal):
  b7c190d844e55634fdde17

Texto cifrado (base64):
  t8GQ2ETlVjT93hc=

Texto descifrado:
  'Javier Chen'
```

### Ejemplo 3

```
Texto plano original:
  'Cifrado de Información'

Clave utilizada:
  'Sección10'

Texto cifrado (hexadecimal):
  1bdd6f99d0f50f19b59964636887ca7861f09cd100e914

Texto cifrado (base64):
  G91vmdD1Dxm1mWRjaIfKeGHwnNEA6RQ=

Texto descifrado:
  'Cifrado de Información'
```

##  Análisis de Seguridad

### 2.1 Variación de la Clave

Cuando se cambia la clave utilizada para generar el keystream, incluso si la modificación es mínima (por ejemplo, cambiar un solo carácter), el keystream generado es completamente diferente debido al comportamiento del generador pseudoaleatorio inicializado con esa semilla. Como consecuencia, el texto cifrado resultante también cambia totalmente.

**Implicación**: La clave debe ser exactamente la misma en el emisor y receptor. No hay forma de descifrar correctamente sin la clave exacta.

### 2.2 Reutilización del Keystream

Reutilizar el mismo keystream para cifrar dos mensajes diferentes es un error grave de seguridad. Cuando se usa la misma clave, el sistema genera exactamente el mismo flujo de números aleatorios, y eso provoca que exista una relación directa entre los dos textos cifrados.

### 2.3 Longitud del Keystream

La longitud del keystream es fundamental para la seguridad del cifrado. Si el keystream es más corto que el mensaje y se reutiliza de forma cíclica, se generan patrones repetitivos que pueden ser explotados por un atacante mediante análisis estadístico. Por otro lado, si el keystream tiene al menos la misma longitud que el mensaje y no se reutiliza, el esquema es mucho más seguro, ya que cada byte del mensaje se combina con un byte único del flujo.

**Casos analizados**:
- **Keystream más corto**: Parte del mensaje queda expuesta en texto plano → **INSEGURO**
- **Keystream igual**: Todo el mensaje está protegido → **ÓPTIMO**
- **Keystream más largo**: Seguro pero desperdicia recursos → **Funcional pero ineficiente**

### 2.4 Consideraciones Prácticas

En un entorno de producción real, la generación del keystream debe cumplir varios aspectos críticos:

1. **Generador Criptográficamente Seguro (CSPRNG)**: El generador debe ser criptográficamente seguro para evitar predictibilidad. Python's `random` NO es adecuado; se debe usar `secrets` o algoritmos como ChaCha20.

2. **No Reutilización**: Nunca debe reutilizarse la misma combinación de clave y nonce, ya que esto compromete la seguridad del sistema. Cada mensaje debe usar un nonce/IV único.

3. **Gestión de Estado e Inicialización**: Debe garantizarse una adecuada gestión del estado interno y la inicialización, evitando semillas débiles o repetidas.

4. **Protección de Claves**: Es importante proteger la clave en memoria y durante la transmisión. Usar sistemas de gestión de secretos (AWS Secrets Manager, HashiCorp Vault).

5. **Algoritmos Auditados**: Utilizar algoritmos modernos y auditados en lugar de PRNG simples, ya que estos últimos no están diseñados para resistir ataques criptográficos avanzados.
- "Understanding Cryptography" - Christof Paar, Capítulo sobre Stream Ciphers

---

⚠️ **IMPORTANTE**: Este código es solo para aprendizaje. NO usar en sistemas de producción.
